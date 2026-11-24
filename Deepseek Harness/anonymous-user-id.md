# anonymous-user-id code notes

## Creation and corrupt-file repair are serialized by a lock file (finding 292)

- `packages/identity/anonymous-user-id/src/index.ts:getOrCreateAnonymousUserId` - the old path did an exclusive create, then on a failed create plus an invalid reread fell back to an unconditional truncating write. Two processes recovering the same corrupt file could both reread it as invalid, both overwrite it, and each memoize its own id, and a reader could observe the truncated file between the truncate and the write. Creation and repair now go through `publishId`, which only writes while holding the lock and always rereads under the lock first, so a valid id written by another process is adopted rather than replaced.
- `packages/identity/anonymous-user-id/src/index.ts:acquireLock` - the lock is an exclusive-create `.anonymous-user-id.lock` file next to the id file. Contention (`EEXIST`, `EPERM`, `EBUSY`) is retried with a synchronous sleep up to `lockTimeoutMs` (default 2000 ms); a lock older than 10 s is treated as left by a crashed process and removed. Any other error (read-only home, home path is a file) reports the home as unavailable immediately.
- `packages/identity/anonymous-user-id/src/index.ts:publishId` - the id is written to a temp file and renamed over the id file, so readers see either the previous content or a complete id, never a partial line. Bounded behavior: an unavailable home or a lock timeout never writes; a timeout rereads once and adopts a valid id, otherwise the fresh id is used in memory for this run only. The unavoidable remaining race is two processes both breaking the same stale lock; the identifier then can diverge for that run and converges on the next launch.

`packages/identity/anonymous-user-id/src/index.ts:acquireLock`: stale-lock removal stats the lock twice and removes it only when the inode and mtime are unchanged, which narrows (it does not close) the window where a process removes a fresh lock that another process created after breaking the same stale one; a failure of either stat means the lock vanished and the loop simply retries. Covered by `tests/lock-race.spec.ts`, which replaces the lock between the two stats.

`packages/identity/anonymous-user-id/src/index.ts:acquireLock`: the stale-lock check is split into `statIfPresent` and `removeStaleLock`, so each `try` holds one statement (verify-empty-catch). A lock file that vanishes between the create attempt and the stat, or between the stat and the removal, is treated as a retry, the same as before the split.

`packages/identity/anonymous-user-id/src/index.ts:statIfPresent`: returns undefined when the path is gone or cannot be read; the caller retries instead of failing the lock acquisition.

`packages/identity/anonymous-user-id/src/index.ts:removeStaleLock`: removes a lock only when the file's inode and mtime still match the stale observation, so a fresh lock written by another process is never removed; a failure to remove it returns and the loop retries.

### `packages/identity/anonymous-user-id/src/index.ts:surface`

Best-effort file handling used to swallow every error, so a home that could not be read, locked or written left no trace of why the id did not persist. Each fs failure now goes through `surface`, which stays silent only for the codes that mean the expected thing for that step and otherwise hands a one-line message with the error to `AnonymousUserIdOptions.onFailure`. The repo's idiom for a library without a Context is an owner-supplied callback (`onFailure`, `onReplayDegrade`, `onSpillFailure`): `llm-deepseek`, `session-telemetry-otel` and `command-feedback` pass `ctx.logger.warn`; a bare caller gets `reportToStderr`, as `subprocess-local` does for spills. The id is still returned in every case, so reporting never makes the call fatal; a reporter that throws is the caller's fault and is not contained.

### `packages/identity/anonymous-user-id/src/index.ts:readPersistedId`

Silent codes: `ENOENT` (no file yet, the first launch) and `ENOTDIR` (a parent is a file, so no id can exist; the later `mkdir` reports the real obstacle). Anything else (`EACCES`, `EISDIR`, `EIO`) is reported and the caller mints and publishes a fresh id as before.

### `packages/identity/anonymous-user-id/src/index.ts:statIfPresent`

Silent code: `ENOENT` (the lock vanished between the create attempt and the stat, so the loop retries). Other stat failures are reported and still treated as "no lock to age", so the loop retries until the timeout.

### `packages/identity/anonymous-user-id/src/index.ts:removeFile`

Shared by stale-lock removal, lock release and temp-file cleanup, which each used to let a failed `rmSync` either vanish or throw out of `getOrCreateAnonymousUserId`. `rmSync` with `force` already ignores `ENOENT`; `EBUSY` and `EPERM` are the lock-contention codes (a Windows process still holds the file) and stay silent because the lock loop retries; every other code is reported. None of them is thrown.

### `packages/identity/anonymous-user-id/src/index.ts:publishId`

`mkdirSync` and the temp write or rename failures mean the home is not writable, so the id lives only in memory for this run. No code is expected there, so every one is reported (the home path and code identify an unwritable or read-only home). Replaces the earlier empty returns.
