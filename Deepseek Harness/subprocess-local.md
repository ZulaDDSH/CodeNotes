
`packages/subprocess/subprocess-local/src/retention.ts:sweepSpillFiles` (empty directory removal): `rmdir` fails on a non-empty directory, which is then kept for the next sweep. The catch continues the loop, which is the only action the loop body would take.

### `packages/subprocess/subprocess-local/src/retention.ts:sweepSpillFiles`

The sweep used to swallow every error in its `lstat`, `unlink`, `readdir` and `rmdir` calls and in the throttled background run, so a root that could not be read or files that could not be removed left spill files accumulating with no trace. Each failure now goes through `surface`: silent for the codes that mean the expected thing for that step, otherwise one call to the sweep's `onFailure` reporter and the sweep carries on, so nothing is fatal. Silent codes: `ENOENT` for `lstat`, `unlink`, `readdir` and `rmdir` (another process swept it first); `ENOTEMPTY` and `EEXIST` for `rmdir` (a directory that still holds a file is kept for the next sweep); `EBUSY` and `EPERM` for `unlink` and `rmdir` (a Windows process still has the file open, the next sweep retries). Everything else (`EACCES`, `EIO`, `EROFS`, an unreadable tmpdir) is reported.

### `packages/subprocess/subprocess-local/src/retention.ts:SweepFailureReporter`

Separate from `SpillFailureReporter` (whose message is about a stream losing its spill file). `packages/subprocess/subprocess-local/src/output.ts:prepareManagedProcessBinding` takes an optional `onSweepFailure` from the owner and defaults to a stderr line; `LocalSubprocessRuntime` passes `logSweepFailure(this.ctx.logger, 'subprocess-local')`, one warn-level line carrying the operation, the path and the error. The throttle in `sweepSpillFilesThrottled` still starts at most one sweep per minute, so a persistent failure logs at most once per failing entry per minute. Note: the sweep trigger is `privateSpillDir`, which is reached from `prepareManagedProcessBinding`, so the reporter is available at the same place.
