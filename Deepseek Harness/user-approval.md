# user-approval code notes

## packages/interaction/user-approval/src/types.ts:ApprovalRequestEvent (finding 140)

`operation` is an optional bounded, redacted summary of the operation's arguments produced by the asker (the tool pipeline). It travels on the `approval/request` waterfall only; `ApprovalService.request` does not copy it into the `approval/asked` audit event, so session logs keep the reason text and nothing else. Answerers must treat it as display text, never as a decision input.

## packages/interaction/user-approval/src/index.ts:ApprovalService.request (finding 421)

A prompt that settled `allowed-once` after the Session's policy had been switched to `never` was recorded as a grant, even though the switch was meant to stop approvals. `request` now re-reads `effectivePolicy(session)` at record time and writes `rejected` for such a settlement, so `approval/decided` carries the enforced outcome. A prompt already on screen is not withdrawn when the policy changes, so the user can still answer it, but the answer cannot grant once the policy is `never`. Decision: enforcement happens at record time, not at prompt time, so a prompt is decided by its answer and then clamped.
