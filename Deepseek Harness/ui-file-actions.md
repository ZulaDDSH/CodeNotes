# ui-file-actions code notes

## Browser image reads have a byte and time budget (finding 392)

- `packages/client/ui-file-actions/src/client/files.ts:BrowserFileActions::read` - the fetch path used `response.blob()`, which buffers the whole remote body with no size or time limit, so a never-ending or very large response held memory until the plugin unloaded. The request now carries a per-request deadline (`REMOTE_IMAGE_MS`) linked to the plugin lifetime, and the body is counted while it streams (`readBounded`) against `REMOTE_IMAGE_BYTES`, cancelling the body when the budget is exceeded.
- `packages/client/ui-file-actions/src/client/files.ts:readBounded` - a declared `Content-Length` above the budget is refused before any body byte is read. A response without a readable body (the test stub shape, and some polyfills) falls back to `blob()` and checks its size afterwards, so the byte budget is then enforced after buffering only in that fallback.
- Decision: `REMOTE_IMAGE_BYTES` is 64 MiB and `REMOTE_IMAGE_MS` is 30 seconds, chosen so ordinary photos and screenshots fit; they are constants, not settings, because the browser has no deployment config for this path. Concurrent actions are not serialized; each is bounded on its own.

## Original-file menu actions read windows, not the complete file (finding 391)

- `packages/client/ui-file-actions/src/client/context-menus.tsx:registerFileMenus` - the original-byte read asked for the complete file with no range, which the server refuses above its 32 MiB complete-read cap, so Download, Save As and Copy failed for large files whose stat succeeded. The read now collects 2 MiB windows from `readOriginalBytes` (the server default window size, so no client knowledge of the cap is needed).
- `packages/client/ui-file-actions/src/client/files.ts:readOriginalBytes` - each window must report the same `absolutePath` and `version` as the first, otherwise the read fails as a changed file; it stops at `eof` or at an empty window, and refuses a total above `MAX_FILE_ACTION_BYTES`.
- `packages/client/ui-file-actions/src/client/context-menus.tsx:registerFileMenus` - when the stat reports a size above `MAX_FILE_ACTION_BYTES` the file's actions are disabled before the user acts, because a renderer-side buffer of that size is worse than a refused action. The stat size is used only when the backend reports it; the total-size check in `readOriginalBytes` covers the unknown case.
- Decision: `MAX_FILE_ACTION_BYTES` is 256 MiB, a client-side ceiling. Raising the server's complete-read cap does not change it. Streaming the file to disk (Desktop save) would lift the ceiling and is not done here.
