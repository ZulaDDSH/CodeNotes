# attachment code notes

### `packages/attachment/attachment/src/index.ts:AttachmentStore.readHostFile`
Finding 349 seam on the abstract service. A backend whose `fileHostPath` or `imageHostPath` hands out host paths implements it so a reader that holds only such a path can have the bytes verified on the descriptor it reads from. The default answers `undefined` (not a path of this backend), so providers without host files, and readers without the seam, are unchanged. See `host-file.ts:readStoredHostFile` for the local implementation and `fs-local/src/index.ts:LocalFileSystem.verifiedHostBytes` for the consumer.

