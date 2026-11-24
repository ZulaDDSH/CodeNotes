# ui-agent-team code notes

## packages/client/ui-agent-team/src/client/TeamModeToggle.tsx:RootTeamModePane

Finding 275. A Host `TEAM_MODE_PERSISTENCE_UNCONFIRMED` rejection means the selection was already appended and is live, but its flush is unconfirmed. The pane reads the live projection for the switch, so a plain toggle after that failure would request the opposite mode instead of retrying the failed save. `RootTeamModePane` now keeps the requested value in `unconfirmed` after such a failure; while it is set, a switch change resends that same value, and an inline `mode.unconfirmed` notice offers a retry button that does the same. Any other outcome clears it. Only the `unconfirmed` flag of `TeamSelectionFailure` selects this path, so ordinary write failures keep the `mode.failed` toast and leave the switch as it was.

## packages/client/ui-agent-team/src/client/mount.ts:registerAgentTeamUi

`selectTeamMode` and the disable callback now return `TeamSelectionFailure` (message plus `unconfirmed`) instead of a bare string. The flag is derived from the Remote error `code` equal to `TEAM_MODE_PERSISTENCE_UNCONFIRMED`, the only code the Host uses for an applied-but-unsaved selection.

## packages/client/ui-agent-team/src/client/TeamDisableConfirmation.tsx:DisableConfirmationForm

An unconfirmed disable keeps the dialog open with the `mode.unconfirmed` message. Its confirm button resends the disable request: the Host sees the mode already off, appends nothing, and only repeats the flush, so the retry never toggles the mode back on.

## Restart

The Host restart behaviour (replaying an appended `team/mode` event after an unconfirmed flush) is owned by the Session journal and is not exercised by a client test. UNVERIFIED for a real restart.

## Remote boundary (blocker)

`mount.ts:registerAgentTeamUi` maps `TEAM_MODE_PERSISTENCE_UNCONFIRMED` from the Remote error `code`. The Host throws this as a `TeamError` from `packages/team/agent-team/src/index.ts:setEnabled`. The Gateway dispatch `rpcFailure` in `packages/api/gateway/src/index.ts` converts any error that is not a `RemoteError` marked with `isDSHRemoteError` into `gateway/internal` with the message text, so the Client never sees the code. The unconfirmed path therefore cannot fire in the running app yet. Unblock: the agent-team owner merges `TEAM_MODE_PERSISTENCE_UNCONFIRMED` into `RemoteErrorDetailsMap` and the generated Remote wrapper in `packages/team/agent-team/lib/typert.host.js` rethrows it as a `RemoteError`. That wrapper is generated, so it needs the typert generator (not run here). The client tests use a `RemoteError` with that code cast through `as never` in `browser-plugin.client.spec.ts`, which records the intended contract, not the shipped one. Status: PARTIAL.
