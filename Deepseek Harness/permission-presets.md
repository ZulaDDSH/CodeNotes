# permission-presets code notes

## packages/interaction/permission-presets/src/index.ts:PermissionPresetService.recordSwitch (finding 422)

A switch used to append `permission/preset`, then `sandbox/mode`, then `approval/policy` as separate records, so a crash or a torn JSONL batch between them could restore a preset that disagreed with its knobs. `recordSwitch` now brackets the two knob writes with an intent record `{ preset, intent: true }` before them and a completion record `{ preset }` after them. Persisted-shape change: `permission/preset` gains the optional `intent: true` field, and one switch now appends two `permission/preset` records around the two knob events where it appended one before, so the event sequence of every transition changes (the permission parity spec pins the new sequence).

## packages/interaction/permission-presets/src/index.ts:applyPermissionEvent (finding 422)

The `permissions` projection's `switching` flag is true exactly between an intent record and its completion record, so a log ending in an intent without completion is visible as a torn switch. The projection `stateVersion` is 3 (was 2) and `switching` is part of the persisted projection state; a projection state written at version 2 is rebuilt from the log.

## packages/interaction/permission-presets/src/index.ts:PermissionPresetService.pinInitialPermission (finding 422)

A torn switch (intent recorded, completion missing) is rolled forward: the knobs of the recorded preset are rewritten and the completion record is appended, rather than rolled back to the previous preset, because the intent is the user's request. Decision: roll forward, not back. The recovery calls `resolve` on the recorded preset, so an intent naming a preset that is no longer configured throws instead of writing knobs for an unknown preset. Logs written before this change carry no intent markers, so a torn switch in them is indistinguishable from a complete one and is not recovered (known limit, UNVERIFIED for real user logs).
