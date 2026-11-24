# ui-permission-presets code notes

## packages/client/ui-permission-presets/src/client/settings-store.ts:PermissionSettingsController.select

Finding 256. The mutation `await` sat in `try/finally`, so a thrown invocation (for example Gateway has no Connection at assembly) cleared the private `saving` flag but never left the published `saving` status, and the row stayed disabled with no error. A thrown write now publishes the same error state as a returned refusal. The mirror face has no refresh method, so an uncertain write is reconciled by the next Host invalidation read rather than an explicit re-read here.

## packages/client/ui-permission-presets/src/client/PermissionSelect.tsx:PermissionSelect

Finding 257. `submit` swallowed every rejection and silently fell back to the projected mode. A rejected or `false` switch now records the failed preset and renders an alert naming the mode that is still active, with a Retry button that resubmits the same preset; the alert clears when the projection reaches that preset or a new submission starts. While the command is pending the trigger keeps showing the confirmed mode, the trigger is disabled and a `switching` status line appears, so an optimistic choice is never presented as confirmed (finding 257 fix; supersedes the earlier optimistic-label decision).
