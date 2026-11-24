# agent-loop code notes

## packages/core/agent-loop/src/agent.ts:Agent::kick (issue 24)

`turn()` decides "no pending input" inside its async body, but the driver only commits the idle phase in `kick`'s `finally`, at least one microtask later. A settlement notice (`inject`, no wake) or a `steer` landing in that gap saw a running, unaborted driver: inject has no wake, and `wakeDriver` deliberately does not latch for a live driver. The message then sat queued until unrelated activity opened a turn.

Fix: `turn()` records `drained` when it ended because the inbox was empty. `kick` re-checks `inbox.hasPending` synchronously after the await and before the idle commit (no await between the re-check and the `finally`), and rearms the phase for another turn when input arrived. Only a drained end re-checks, so blocked/empty/error endings keep their existing no-restart behaviour, and a disposed agent never restarts.

Regression: `packages/core/agent-loop/tests/retirement-race.spec.ts`.

## packages/core/agent-loop/src/agent.ts:Agent::rearm (issue 24)

Shared by the in-turn continuation and `kick`'s retirement re-check. A fresh `AbortController` makes a wake latch set on the previous controller stale, because the live driver claims the queue itself; step restarts at 0.

## packages/core/agent-loop/src/index.ts:AgentLoop (released inbox checkpoint)

`AgentLoop[Service.init]` registers the inbox projection, and unloading the loop releases that unit. A released projection unit stays in the registry as a predecessor (`packages/session/session-projection/src/index.ts`), and `checkpoint()` keeps the last row of each Session that had one, so after `loop.dispose()` the disposed Agent's session still reports `inbox` (empty queues, `seq` -1) while a Session created afterwards has none. `packages/core/agent-loop/tests/resource-lifecycle.spec.ts` therefore asserts `toHaveProperty('inbox')` for the disposed Agent and `not.toHaveProperty('inbox')` for a fresh Session, the form upstream settled on in eb1713f856. The earlier `toBeUndefined` assertion was reintroduced by the merge a4ae6f2a6b and is not a disposal-ordering regression from the persistence-drain fiber or the kick rearm loop, which do not write the checkpoint.

## packages/core/agent-loop/src/inbox.ts:ReactLoopInbox::claim (issue 424)

Claim still removes pending input durably before routing, which keeps the durable `agent/inbox/spliced` fold unchanged. The result is grouped as `ClaimedInput` (`next-step` and `next-turn`) so a failed step can put each message back into its original list in order. The next-turn removal runs inside a try: if it throws after the next-step removal succeeded, the next-step part is restored before the error propagates, so a failed claim never leaves a half-removed batch.

## packages/core/agent-loop/src/inbox.ts:ReactLoopInbox::restore (issue 424)

Restore prepends next-turn first and next-step second, so the original order (step input before the queued turn) is kept at the head of the pending lists. Restore is an in-process recovery only. The durable log still shows the claim, so a crash between the durable claim and user-message admission loses that input on restart; closing that gap needs a persisted claim record or a schema change to `agent/inbox/spliced`, and is a NEEDS NOMAD decision.

## packages/core/agent-loop/src/agent.ts:ReactLoopAgent::preStep (issue 424)

Route, assembly, and pre-step failures all go through `releaseClaim`. A pre-step `reject` goes to `discardClaim` because the rejection is an intentional block, not a transient failure, and the turn ends with `blocked`. Discarded messages are announced with `agent/inbox/discarded` so listeners can observe the drop.

## packages/core/agent-loop/src/agent.ts:ReactLoopAgent::releaseClaim (issue 424)

Cancellation discards instead of restoring. An aborted turn's queue must not be replayed on the next wake; the existing contract (`cancel({ keepInbox: true })` does not restore work already claimed by a waking send) depends on this. Restore failures are wrapped in an `AggregateError` that keeps both the original cause and the restore error, so neither is hidden.

## packages/core/agent-loop/src/agent.ts:ReactLoopAgent::discardClaim (issue 424)

Discard emits one `agent/inbox/discarded` per claimed message, with the next-step messages before the next-turn messages, matching the claim order.

## packages/core/agent-loop/src/agent.ts:ReactLoopAgent::unadmittedInput (issue 424)

Restoration is id-based: a message is restored only when its id is absent from `PreparedStep.admitted`. A message already appended as `user/message` is never put back, even if the step later fails in `llm/stream`, which avoids duplicate model input. A pre-step hook that rewrites the batch to an empty `enter` is an intentional drop, so the claimed messages are not restored in that case.

Regression tests: `packages/core/agent-loop/tests/claimed-input-recovery.spec.ts` (route, assembly, pre-step, request preparation, step append, cancellation, reject, admitted-then-failed, hook drop). Existing expectations that encoded the lost-prompt behaviour were updated in `packages/core/agent-loop/tests/interception.spec.ts`, `packages/core/agent-loop/tests/loop.spec.ts`, and `packages/core/agent-loop/tests/resume.spec.ts`.

## packages/core/agent-loop/src/agent.ts:ReactLoopAgent::releaseClaim (bounded restores)

Each message id may be restored `MAX_CLAIM_RESTORES` (3) times in a row across wakes; the count is of consecutive failures only. `packages/core/agent-loop/src/agent.ts:ReactLoopAgent::preStep` clears the counts of the claimed messages once routing, assembly and the pre-step waterfall succeed (the claim is held), so a failure after that point starts a new run of one, where the earlier code kept adding to the total of every pre-admission failure the message had seen. A second count, `MAX_TOTAL_CLAIM_RESTORES` (three times the consecutive bound), is never reset by a held claim, so a failure that persists past the hold (request preparation, step append) still discards the message after nine restores instead of leaving it at the inbox head for ever. Admission, discard and removal from the inbox clear both counts. The restored message sits at the head of next-turn, so a persistent pre-admission failure (a poisoned prompt) would otherwise be reclaimed first on every wake and starve the queue behind it. On the next failure after the limit the message is discarded with `agent/inbox/discarded` and the error handed to `agent/error` is replaced by one that says how many messages were discarded and carries the original failure as `cause`. Counts live in `restoreCounts` (process-local, cleared on discard and on admission as `user/message`); a message removed from the inbox by someone else leaves a stale entry until the agent is collected.

## packages/goal/goal-round-driver/src/index.ts:apply (pre-step throw)

A throwing downstream pre-step hook makes the agent restore the whole claimed batch (`releaseClaim`). The goal round message is bound to a reservation that the driver clears in that path, so restoring it is wrong: it would sit in next-turn as a stale round and break the fail-closed guarantee. The driver records its id in `DriverState.abandoned` before rethrowing; the `agent/inbox/inserted` listener removes it from the inbox when the agent restores it. Other claimed messages (not goal rounds) are restored by the agent as intended.


packages/core/agent-loop/src/index.ts:AgentLoop::initializeAgent (finding 281, retryable teardown): the per-Agent teardown was memoized for its whole life, so a rejected disposal stayed rejected and a caller that retried `AgentHandle.dispose()` never reached the failed step again. The memo now resets when an attempt rejects, so a later call re-runs the work that has not yet succeeded. The reverse steps are tracked separately so a retry never repeats a side effect on a Session that is already closed. `cancelAttempted` is set before the disposed-cause cancel, so the queue clear runs at most once; its failures are retained and rethrown by every later attempt, because the machine is gone once the scope is released and the queue cannot be cleared again. `scopeReleased` gates the scope dispose and `sessionClosed` gates `handle.close()`; a close that failed is retried, a close that succeeded is not, which keeps a retry from appending to a closed Session. Detach and registry release run once, after the failing steps, as before.

Limit: a queue-clear failure stays reported on every later dispose of that handle, by design. The Agent's teardown cannot prove its pending inbox was cleared, so it does not report success on retry.

- `packages/test-support/agent-loop-testkit/src/index.ts:mountAgentLoopTestHarness` - `AgentLoopTestHarness.claim` flattens the record that `Inbox.claim` returns (`{ next-step, next-turn }`, changed by commit e0b65acdd0) into the `UserMessage[]` the harness type declares, `next-step` first and then `next-turn`. Passing the record through made every caller that iterates the claim fail with "claimed is not iterable".
