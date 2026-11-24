# DSH code notes

## Withdrawn native folder requests drop their late answer (finding 393)

- `packages/client/ui-directory-picker-native/src/client/flow.ts:NativeDirectoryFlow` - each rising `open` edge now creates a request ticket; withdrawing `open` clears it, and a settlement delivers only while its ticket is still the current one, so an answer from a withdrawn chooser cannot reach a newer request.
- `packages/client/ui-directory-picker-native/src/client/flow.ts:NativeDirectoryFlow` - the ticket is separate from the `alive` ref, which still gates unmount; unmount and withdrawal discard settlements for different reasons, so one flag would hide which rule applied.
- Decision: the native chooser cannot be closed over the wire, so a withdrawn chooser stays on screen until answered and its answer is dropped. A reopened request launches a second chooser while the first may still be visible; this is the existing limit and is not changed here. Default chosen: drop the stale answer rather than apply it.
- Test: `packages/client/ui-directory-picker-native/tests/client-flow.client.spec.tsx` covers withdrawal before settlement and reopen with both settle orders.
