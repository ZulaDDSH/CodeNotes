### `packages/api/session-controller/src/catalog.ts:missingCredentialProvider`
- `description`: Checks whether a provider's configured API-key reference currently resolves to a configured credential.
- `params.ctx`: Host context providing credential services and provider settings.
- `params.provider`: Provider route id to inspect.
- `returns`: True when the provider has a named API-key reference with no configured credential; false for providers without such a reference.

### `packages/api/session-controller/src/client/sessions/session.ts:Session.configureSubagent`
- `params.parentAvailable`: Whether the addressed parent Agent is available, used to update this Session's subagent transport state.

### `packages/api/session-controller/src/client/sessions/session.ts:Session.handleSubagentParentAvailable`
- `description`: Updates the cached availability of a subagent's parent Agent and notifies snapshot observers when it changes.
- `params.available`: Whether the parent Agent is currently available.

### `packages/api/session-controller/src/index.ts:SessionController.teamRoleDefaultOf`
- `description`: Resolves the effective fixed route for a Team role from the Session's inherited subagent setup.
- `params.session`: Session whose setup and ancestor chain provide the role route.
- `params.role`: Team role whose fixed route is requested.
- `returns`: The configured fixed route, or undefined when no fixed route is available.

### `packages/api/session-controller/src/index.ts:SessionController.missingCredentialProvider`
- `description`: Checks whether a provider route has a named API-key reference without a configured credential.
- `params.provider`: Provider route id to inspect.
- `returns`: True when its credential is missing; false when the provider has no named API-key reference or its credential is configured.

### `packages/api/session-controller/src/run-options.ts:runOptionsProjection`
- `description`: Defines the per-Session run-options projection, storing the latest run-options event and exposing its current value on the wire.

### `packages/api/session-controller/src/subagent-setup.ts:SUBAGENT_TEAM_ROLES`
- `description`: Ordered Team delegation roles that can receive fixed subagent routes.

### `packages/api/session-controller/src/subagent-setup.ts:SubagentTeamRole`
- `description`: Union of the Team delegation role names accepted by the session controller.

### `packages/api/session-controller/src/subagent-setup.ts:SubagentTeamRoleRoute`
- `description`: Route shape selecting either a Claude CLI model or an LLM provider model for a Team delegation role.

### `packages/api/session-controller/src/subagent-setup.ts:subagentTeamRoleRouteOf`
- `description`: Reads a Team role's fixed route from an enabled, schema-valid subagent setup.
- `params.value`: Candidate setup value to validate.
- `params.role`: Team role name to look up.
- `returns`: The role's fixed route, or undefined when the setup is invalid, disabled, the role is unknown, or its setting is not fixed.

### `packages/api/session-controller/src/team-role-defaults.ts:teamRoleDefaultOf`
- `description`: Resolves a Team role's fixed route from the nearest Session setup, then the global default at the root.
- `params.ctx`: Host context providing Session projections and loaded Session lookup.
- `params.session`: Starting Session whose setup or ancestors are searched.
- `params.role`: Team role whose route is requested.
- `params.globalDefault`: Candidate global setup used when the root Session has no setup.
- `returns`: The route from the nearest Session with a setup, or the global default at the root only when no Session setup is present; undefined when the selected setup has no fixed route; throws if an ancestor is unavailable or the chain exceeds eight Sessions.

### `packages/client/ui-chat/src/client/contract/process-groups.ts:RunningProcessActivity`
- `description`: Process activity category that can represent a live running tool, excluding the context-only category.

### `packages/client/ui-chat/src/client/contract/process-groups.ts:activity`
- `description`: Maps a tool name to its display activity category using the Chat process-group naming rules.
- `params.name`: Tool name to classify.
- `returns`: The matching running activity category, or the general tools category when no specialized name matches.

### `packages/client/ui-chat/src/client/contract/process-groups.ts:ProcessActivitySummary`
- `description`: Aggregated activity counts, targets, live task detail, and write/edit line totals for one process group.

### `packages/client/ui-chat/src/client/conversation-nodes/process-activity.ts:processActivity`
- `description`: Aggregates unique process activities, changed-file targets, line totals, and the latest running activity with bounded detail.

### `packages/client/ui-settings-account/src/index.ts:Config`
- `description`: Account settings client configuration containing the public contact questionnaire URL and source value.

### `packages/client/web/src/boot.ts:AppWebEntry.setBootHint`
- `description`: Replaces the text shown on the boot page to identify the current startup phase.
- `params.text`: Startup hint to display.

### `packages/llm/llm-pi-ai/src/stream.ts:toStreamChunks`
- `params.onUsageAboveWindow`: Optional callback invoked with input-token usage when a successful stop or tool-call result exceeds the resolved context window.
- `params.prefixDigest`: Optional digest of the request prefix recorded in replay state for the completed turn.

## Account notification contracts

### `packages/credentials/deepseek-account/src/index.ts:DeepSeekAccount.getNotifications`
- `description`: Queries unconsumed notifications in server order without reading profile data.
- `params.client`: Identity of the requesting UI; its language selects the server-authored message.
- `returns`: Notifications with their next query delay, or null when the account grant is absent or changed.

### `packages/credentials/deepseek-account/src/index.ts:DeepSeekAccount.getNotificationContent`
- `description`: Queries localized content for a notification, including one already consumed.
- `params.id`: Notification whose content is requested.
- `params.client`: Identity of the requesting UI; its language selects the localized content.
- `returns`: Content without changing consumption, or null when the account grant is absent or changed.

### `packages/credentials/deepseek-account/src/index.ts:DeepSeekAccount.reportNotificationEvent`
- `description`: Reports a notification interaction; Platform decides whether the notification is consumed.
- `params.id`: Notification the user interacted with.
- `params.event`: Displayed frame, non-dismiss action click, or explicit dismissal.
- `params.client`: Identity of the requesting UI for this call.
- `returns`: True when Platform accepts the event; false when the account grant is absent or changed.

### `packages/api/session-controller/src/subagent-setup.ts:SubagentRoleSetting`
- `description`: Defines a fixed route for a configured subagent role.

### `packages/client/ui-settings-models/src/client/search-entries.ts:modelsSearchEntries`
- `description`: Builds localized search entries for the Models settings page.

### `packages/subagent/tool-subagent/src/index.ts:CLAUDE_CLI_PROVIDER`
- `description`: Names the default Claude Code provider used for delegated CLI roles.

### `packages/boot/app-boot/src/profile-resolution/resolver.ts:isInternalModuleUnavailable`
- `description`: Recognizes Node module-loading failures that identify the requested internal module as unavailable.
- `params.error`: Thrown value from the module-loading attempt.
- `params.moduleId`: Internal module identifier requested by the caller.
- `returns`: True only when a recognized Node error identifies the requested module itself as unavailable; false for unrelated or transitive failures.
### `packages/bundle/web-app/src/memorix.ts:BACKUP_SUFFIX`
- `description`: Suffix for the one-time database backup before Memorix 1.9.10 can update existing data.

### `packages/bundle/web-app/src/memorix.ts:backupExistingDatabase`
- `description`: Keeps one copy of an existing Memorix database and its SQLite sidecars.
- `params.dataDirectory`: Database directory, defaulting to MEMORIX_DATA_DIR or ~/.memorix/data.
- `returns`: The backup path, or undefined when no copy is needed.

### `packages/terminal/terminal-bash/src/session.ts:LocalPtySession.watchLine`
- `description`: Watches the sanitized output stream for an exact complete line.
- `params.line`: Complete output line to observe.
- `returns`: A handle to query the observation and remove the watch.

### `packages/terminal/terminal/src/index.ts:TerminalSessionService.watchLine`
- `description`: Watches an owned terminal for a complete output line when its backend supports it.
- `params.owner`: Agent that owns the terminal.
- `params.id`: Owned terminal identity.
- `params.line`: Complete output line to observe.
- `returns`: A watch handle, or undefined when the backend does not support watching.

### `packages/terminal/terminal-bash/src/sanitize.ts:TerminalSanitizer.resetPromptTracking`
- `description`: Discards a prompt-tail candidate observed before the active send's readiness boundary.

### `packages/acp/acp/src/session.ts:UPDATE_DELIVERY_TIMEOUT_MS`
- `description`: Default time in milliseconds one queued Session update may take to be delivered before the output is treated as failed.

### `packages/acp/acp/src/session.ts:MAX_PENDING_UPDATE_BYTES`
- `description`: Default cap on the estimated bytes of Session updates queued behind a slow notifier before the output is treated as failed.

### `packages/api/gateway/src/stream-server.ts:MAX_STREAMS_PER_CONNECTION`
- `description`: Maximum number of logical streams one Remote connection may hold open at once.

### `packages/api/gateway/src/stream-server.ts:MAX_CONNECTION_INBOX_BYTES`
- `description`: Minimum buffered uplink byte budget shared by all logical streams of one Remote connection.

### `packages/api/remote-access/src/index.ts:RemoteAccessService.changes`
- `description`: Emits once each time the access state visible to the principal changes; the owner principal never observes a change and the iterator waits until cancelled.
- `params.principal`: Principal whose visible access state is followed.
- `params.signal`: Cancels the subscription; the service lifetime also ends it.
- `returns`: An async iterable that yields once per change.

### `packages/api/session-controller/src/history/controller.ts:HistoryController.outline`
- `description`: Returns one page of the turn outline (prompt and response previews) for a Session address, walking backwards from the requested sequence.
- `params.request`: Session address, optional exclusive upper sequence bound and optional page size.
- `params.signal`: Cancels the observation.
- `returns`: The outline page.

### `packages/api/session-controller/src/history/controller.ts:validateOutlineRequest`
- `description`: Rejects an outline request whose `beforeSeq` is not a non-negative safe integer or whose `maxEntries` is not a positive safe integer within the page limit.
- `params.request`: Outline request to validate.
- `returns`: Nothing; throws a `gateway/bad-request` RemoteError on an invalid field.

### `packages/api/session-controller/src/history/outline.ts:OUTLINE_PAGE_LIMIT`
- `description`: Largest number of turn entries one outline page may carry.

### `packages/api/session-controller/src/history/outline.ts:OUTLINE_SCAN_LIMIT`
- `description`: Largest number of events one outline page scan walks backwards before it stops and reports more history.

### `packages/api/session-controller/src/history/outline.ts:outlinePage`
- `description`: Builds one outline page from the events before an end sequence, scanning backwards until the requested number of turns is found or the scan limit is reached.
- `params.events`: Session events in sequence order.
- `params.end`: Exclusive upper sequence bound of the scan.
- `params.maxEntries`: Number of turn entries to return.
- `params.scanLimit`: Most events to scan backwards from `end`.
- `returns`: Turn entries oldest first, whether older history remains, and the sequence to continue before.

### `packages/api/session-controller/src/index.ts:SessionController.outline`
- `description`: Remote method returning one page of a Session's turn outline.
- `params.request`: Session address, optional exclusive upper sequence bound and optional page size.
- `params.signal`: Cancels the read.
- `returns`: The outline page.

### `packages/api/session-controller/src/types.ts:SessionOutlineRequest`
- `description`: Request for one page of a Session's turn outline, newest turns first, optionally continuing before a sequence number.

### `packages/api/session-controller/src/types.ts:SessionOutlineEntry`
- `description`: One turn in a Session outline with the sequence of its start and short previews of the prompt and the response.

### `packages/api/session-controller/src/types.ts:SessionOutlinePage`
- `description`: One outline page: its entries, whether older history remains, and the sequence to continue before.

### `packages/api/workspace-controller/src/feed.ts:WorkspaceFeed.follow`
- `params.refreshes`: Optional source of refresh triggers; each value it yields pushes a refresh frame to the follower, and it is cancelled when the follower ends.

### `packages/api/workspace-files/src/changes.ts:WorkspaceChangeFeed.follow`
- `params.admitsOutside`: Optional predicate deciding whether a resolved target that is not inside the workspace root may still be watched; without it such targets are rejected as outside the workspace.

### `packages/attachment/attachment-local/src/compression-limiter.ts:CompressionLimiter.run`
- `description`: Runs one task after an instance slot becomes available; a task that must wait joins the bounded waiting line, and a waiting task whose abort signal fires is dropped and rejects with a compression failure.
- `params.task`: Compression operation occupying one slot until settlement.
- `params.options`: Optional abort `signal` that drops the task while it waits for a slot, and the input `bytes` the task counts against the queue byte bound.
- `returns`: The task result, or a rejection with a compression failure.

### `packages/attachment/attachment-local/src/file-quota.ts:FileStoreQuota`
- `description`: Byte quota for verbatim file objects: caps one file and the total stored under `file-objects/`, reserving in-flight upload bytes so concurrent uploads cannot jointly exceed the cap.

### `packages/attachment/attachment-local/src/file-quota.ts:FileStoreQuota.reserve`
- `description`: Reserves room for more bytes of one upload, loading current usage on first call; throws when the file or the stored total would exceed its limit.
- `params.already`: Bytes already reserved for this upload.
- `params.bytes`: Further bytes to reserve.
- `returns`: Resolves once the bytes are reserved.

### `packages/attachment/attachment-local/src/file-quota.ts:FileStoreQuota.settle`
- `description`: Releases an upload's reservation and records the bytes of a newly published object.
- `params.reserved`: Bytes reserved for the upload.
- `params.newObjectBytes`: Bytes added to the store, zero when the object already existed or the upload failed.

### `packages/attachment/attachment-local/src/file-quota.ts:FileStoreQuota.meter`
- `description`: Passes a body through while reserving each chunk against the quota, so an oversized stream fails before it completes.
- `params.data`: Body chunks to meter.
- `params.progress`: Running count of reserved bytes, updated per chunk and read by the caller to settle.
- `returns`: The same chunks once each is reserved.

### `packages/attachment/attachment-local/src/file-store.ts:saveFileVerbatim`
- `params.quota`: Optional quota; a new object reserves its size before it is written and settles afterwards.

### `packages/attachment/attachment-local/src/file-store.ts:saveFileStreamVerbatim`
- `params.quota`: Optional quota; chunks are metered while streaming and the reservation is settled once the digest is known.

- `packages/attachment/attachment-local/src/index.ts:DEFAULT_MAX_FILE_BYTES` - Default size limit in bytes for one verbatim file; the constant is module-private.

- `packages/attachment/attachment-local/src/index.ts:DEFAULT_MAX_STORED_FILE_BYTES` - Default limit in bytes on the total size of stored verbatim file objects; the constant is module-private.

### `packages/client/connection/src/http-bridge.ts:BUFFERED_BODY_BUDGET_FACTOR`
- `description`: Multiple of the per-request body limit that sets the byte budget shared by buffered request bodies of one connection.

### `packages/client/connection/src/http-bridge.ts:BufferedBodyBudget`
- `description`: Shared byte budget for request bodies buffered by the HTTP bridge.

### `packages/client/connection/src/http-bridge.ts:BufferedBodyBudget.reserve`
- `description`: Takes bytes from the budget when they fit.
- `params.bytes`: Bytes to reserve.
- `returns`: True when reserved; false when the budget would be exceeded.

### `packages/client/connection/src/http-bridge.ts:BufferedBodyBudget.release`
- `description`: Returns previously reserved bytes to the budget.
- `params.bytes`: Bytes to release.

### `packages/client/store/src/index.ts:browserStorage`
- `description`: Returns the browser's `localStorage`, or undefined when it is missing or access throws.
- `returns`: The storage, or undefined.

### `packages/client/ui-chat/src/client/chat/turn-rail-items.ts:OutlineEntry`
- `description`: One outlined turn: its number, start sequence and prompt and response previews.

### `packages/client/ui-chat/src/client/chat/turn-rail-items.ts:KnownOutline`
- `description`: Outline entries known so far, keyed by turn number.

### `packages/client/ui-chat/src/client/chat/turn-rail-items.ts:absorbOutline`
- `description`: Narrows wire outline entries and records each valid one by turn number, dropping malformed entries.
- `params.known`: Outline map updated in place.
- `params.wire`: Untrusted wire value expected to hold outline entries.

### `packages/client/ui-chat/src/client/chat/turn-rail-items.ts:oldestKnownSeq`
- `description`: Finds the lowest start sequence among the known outline entries.
- `params.known`: Outline entries to search.
- `returns`: The lowest sequence, or undefined when no entry is known.

### `packages/client/ui-chat/src/client/chat/use-known-outline.ts:useKnownOutline`
- `description`: Accumulates a Session's outline entries and pages older ones in through `loadOutline` while history remains; state resets when the Session changes and a failed page is not retried for the same first sequence.
- `params.sessionId`: Session whose outline is tracked.
- `params.outline`: Wire outline carried by the Session snapshot.
- `params.firstSeq`: Sequence of the first loaded event, or null when none is loaded.
- `params.hasMore`: Whether older history remains.
- `params.loadOutline`: Loads one older outline page before a sequence; resolves null on failure.
- `returns`: Known outline entries.

### `packages/client/ui-deliverables/src/changes.ts:workspaceDiffUrl`
- `params.path`: Workspace-relative file path to compare.

### `packages/client/ui-deliverables/src/client/workspace-diff.ts:WorkspaceDiffStore.load`
- `params.path`: Workspace-relative file path to compare.

### `packages/client/ui-deliverables/src/client/workspace-diff.ts:WorkspaceDiffStore.forget`
- `description`: Drops every cached comparison for one workspace.
- `params.workspaceId`: Workspace whose cached comparisons are dropped.

### `packages/client/ui-file-actions/src/client/files.ts:REMOTE_IMAGE_BYTES`
- `description`: Largest number of bytes read when downloading a remote original image.

### `packages/client/ui-file-actions/src/client/files.ts:REMOTE_IMAGE_MS`
- `description`: Time in milliseconds a remote original image download may take before it is aborted.

### `packages/client/ui-file-actions/src/client/files.ts:MAX_FILE_ACTION_BYTES`
- `description`: Default limit in bytes on a file read in full for a file action.

### `packages/client/ui-file-actions/src/client/files.ts:OriginalWindow`
- `description`: One window of an original file: its absolute path, version, the bytes read, and whether the end of the file was reached.

### `packages/client/ui-file-actions/src/client/files.ts:readOriginalBytes`
- `description`: Reads a file window by window until its end, failing when the limit is exceeded or the version changes mid-read.
- `params.readWindow`: Reads one window starting at a byte offset.
- `params.signal`: Cancels the read.
- `params.limit`: Largest total number of bytes accepted.
- `returns`: The concatenated file bytes.

### `packages/client/ui-memory-workspace/src/automation.ts:SessionTurns.discard`
- `description`: Clears the recorded prompts and outcome of the turn in progress.

### `packages/client/ui-memory-workspace/src/automation.ts:MemoryAutomation.store`
- `description`: Stores one observation through the memory provider, serialized with other automation writes.
- `params.provider`: Memory provider used for the write.
- `params.cwd`: Project directory the observation belongs to, when known.
- `params.signal`: Cancels the write.
- `params.args`: Observation arguments passed to the provider.
- `params.subject`: Subject named in failure messages.
- `returns`: The stored observation.

### `packages/client/ui-memory-workspace/src/automation.ts:MemoryAutomation.archive`
- `description`: Marks observations as archived through the memory provider, serialized with other automation writes.
- `params.provider`: Memory provider used for the write.
- `params.cwd`: Project directory the observations belong to, when known.
- `params.signal`: Cancels the write.
- `params.ids`: Observation ids to archive.
- `params.subject`: Subject named in failure messages.
- `returns`: Resolves once the observations are archived.

### `packages/client/ui-message-feedback/src/client/controller.ts:MessageFeedbackController.watched`
- `description`: Whether any listener is currently subscribed to this controller.

### `packages/client/ui-message-feedback/src/client/controller.ts:MessageFeedbackController.evict`
- `description`: Resets an unwatched controller to its cold state so the next watcher reloads; does nothing while listeners are subscribed or after disposal.

### `packages/client/ui-message-feedback/src/client/controller.ts:MessageFeedbackController.retract`
- `params.localOnly`: When true the deletion is requested as local-only rather than also removing a shared copy.

### `packages/client/ui-message-feedback/src/client/dialog.ts:FeedbackSharingLoad`
- `description`: Loads the Session's feedback sharing state, resolving null when it cannot be read.

### `packages/client/ui-message-feedback/src/client/dialog.ts:exportMode`
- `description`: Chooses how feedback is exported: off when sharing is disabled or none, local when the user asked for local-only, to when a destination is known, otherwise generic.
- `params.sharing`: Current sharing state, or null when unknown.
- `params.localOnly`: Whether the user chose to keep this feedback local.
- `returns`: The export mode.

### `packages/client/ui-message-feedback/src/client/dialog.ts:FeedbackDialogController.refreshSharing`
- `description`: Reloads the sharing state into the dialog state, treating a failed load as unknown.
- `returns`: Resolves once the state is updated.

### `packages/client/ui-provider-quota/src/client/usage-prompt.ts:BACK_DISMISSAL_HOLD_MS`
- `description`: Time in milliseconds past a window's reset during which a dismissed switch-back offer stays dismissed.

### `packages/client/ui-settings-plugin-inventory/src/client/hook-writes.ts:HookSettingsRemote`
- `description`: Settings remote used to read a plugin namespace with its revision and to write path operations against an expected revision.

### `packages/client/ui-settings-plugin-inventory/src/client/hook-writes.ts:enabledAfterSwitch`
- `description`: Computes the enabled hook keys after switching some keys on or off, expanding a stored wildcard into the visible keys when switching off.
- `params.stored`: Currently stored enabled keys, possibly the wildcard.
- `params.visible`: Keys currently listed in the inventory.
- `params.keys`: Keys being switched.
- `params.on`: True to enable the keys, false to disable them.
- `returns`: The next enabled key list.

### `packages/client/ui-settings-plugin-inventory/src/client/hook-writes.ts:enabledAfterEnableAll`
- `description`: Computes the enabled hook keys after enabling every visible key; a stored wildcard stays a wildcard.
- `params.stored`: Currently stored enabled keys, possibly the wildcard.
- `params.visible`: Keys currently listed in the inventory.
- `returns`: The next enabled key list.

### `packages/client/ui-settings-plugin-inventory/src/client/hook-writes.ts:enabledAfterDisableAll`
- `description`: Computes the enabled hook keys after disabling every visible key; a stored wildcard becomes empty.
- `params.stored`: Currently stored enabled keys, possibly the wildcard.
- `params.visible`: Keys currently listed in the inventory.
- `returns`: The next enabled key list.

### `packages/client/ui-settings-plugin-inventory/src/client/hook-writes.ts:createHookWrites`
- `description`: Creates the hook setting writers: one rewrites the enabled hook list from the stored value and one sets descriptions, both retrying a bounded number of times when the namespace revision changes underneath them.
- `params.settings`: Settings remote used to read and mutate the namespace.
- `returns`: The `updateEnabledHooks` and `setHookDescriptions` writers.

### `packages/client/ui-sidebar-documentpreview/src/client/excel/xlsx-archive.ts:XlsxArchiveBudget`
- `description`: Optional limits on a workbook archive: entry count, bytes of one entry, and total expanded bytes.

### `packages/client/ui-tool/src/client/tool/models/office-card-model.ts:officeImageCard`
- `description`: Builds the image card for a fully rendered `office_render` result, or null for any other tool, an error, or an incomplete render.
- `params.toolName`: Name of the tool that produced the result.
- `params.block`: Tool call block carrying the result.
- `params.cwd`: Working directory used to shorten paths.
- `params.home`: Home directory used to shorten paths.
- `returns`: The card model, or null.

- `packages/compaction/compaction-image-offload/src/image-offload.ts:restoreOffloadedImages` - Restores image blocks that earlier offload projections replaced, for the user messages and tool results among the given source events; returns true when a restore projection was recorded. Module-internal, not a package export.

### `packages/compaction/compaction-image-offload/src/projection.ts:imageOffloadProjection`
- `description`: Message projection that replaces the selected image blocks with an offloaded placeholder.

### `packages/compaction/compaction-image-offload/src/projection.ts:imageRestoreProjection`
- `description`: Message projection that puts back the image blocks an offload projection replaced.

### `packages/compaction/compaction-tool-result-pruner/src/config.ts:codePointOffset`
- `params.text`: Text to measure.
- `params.points`: Number of Unicode code points to skip from the start.
- `returns`: The UTF-16 offset in `text` after that many code points, clamped to the text length.

### `packages/context/time-context/src/request-zone.ts:BROWSER_POLICY_PREFIX`
- `description`: Prefix of the request policy line that reports the browser time zone.

### `packages/core/agent-default-model/src/index.ts:AgentDefaultModelConfig.saveInitialSelection`
- `description`: Saves the resolved model as this Agent's default only when no override is stored and no selection was submitted while it resolved.
- `params.resolve`: Resolves the initial selection, or undefined when none applies.
- `returns`: Resolves once the selection is saved or skipped.

### `packages/core/tool-executor-ptc/src/ptc.ts:jsonByteSize`
- `description`: Estimates the serialized byte size of a JSON value, stopping early once the limit is exceeded.
- `params.value`: JSON value to measure.
- `params.limit`: Size after which counting stops.
- `returns`: The estimated size, which exceeds `limit` when the value is larger than it.

### `packages/core/tools/src/contracts.ts:ToolPreflightResult`
- `description`: Outcome of a tool preflight: allowed, or not allowed with the reason the call would be refused.

### `packages/core/tools/src/index.ts:ToolsService.preflight`
- `description`: Runs the pre-dispatch checks of a tool call without executing it.
- `params.input`: Tool call to check.
- `returns`: Whether the call would dispatch, with a reason when it would not.

### `packages/core/tools/src/pipeline.ts:ToolExecutionPipeline.preflight`
- `description`: Prepares a tool call and reports whether it would dispatch, deriving the refusal reason from the prepared error or text result.
- `params.input`: Tool call to check.
- `returns`: Whether the call would dispatch, with a reason when it would not.

### `packages/credentials/credentials-local/src/windows-acl.ts:system32`
- `description`: Resolves a tool inside the Windows System32 directory.
- `params.tool`: Executable file name.
- `returns`: Absolute path of the tool.

### `packages/credentials/credentials-local/src/windows-acl.ts:windowsAclViolations`
- `description`: Lists trustees on a file's access list other than the current user, SYSTEM and Administrators.
- `params.filename`: File whose access list is read.
- `returns`: The violating trustees; empty when the file is owner-only.

### `packages/credentials/credentials-local/src/windows-acl.ts:restrictToOwnerWindows`
- `description`: Removes inherited access and grants full control only to the current user, SYSTEM and Administrators.
- `params.filename`: File to restrict.
- `returns`: Resolves once the access list is rewritten.

### `packages/credentials/credentials-local/src/windows-acl.ts:ownerOnlyFixCommand`
- `description`: Builds the `icacls` command that restricts a file to the current user, SYSTEM and Administrators.
- `params.filename`: File to restrict.
- `returns`: The command line to show the user.

### `packages/credentials/deepseek-account/src/index.ts:DeepSeekAccount.getRevocationStatus`
- `description`: Reports the state of the most recent logout grant revocation.
- `returns`: `none` when no revocation ran, `pending` while one is in flight, otherwise its outcome.

### `packages/credentials/deepseek-account/src/logout.ts:RevocationOutcome`
- `description`: Result of revoking a logout grant: revoked, failed, or unconfirmed when the outcome could not be established.

### `packages/deliverables/workspace-changes/src/git.ts:TreeSnapshot`
- `description`: A work tree snapshot: the tree object id, whether the snapshot is incomplete, and the paths skipped.

### `packages/deliverables/workspace-changes/src/store.ts:sessionDirectory`
- `description`: Maps a Session id to its directory under the retention root, hashing ids that are not plain names, and throws when the result would leave the root.
- `params.root`: Retention root directory.
- `params.sessionId`: Session id.
- `returns`: The Session directory path.

### `packages/deliverables/workspace-changes/src/store.ts:migrateLegacyDirectory`
- `description`: Renames a Session's legacy directory to its current name, ignoring a missing source or a conflicting existing target.
- `params.root`: Retention root directory.
- `params.sessionId`: Session id.
- `returns`: Resolves once the rename is done or skipped.

### `packages/deliverables/workspace-changes/src/store.ts:ensurePrivateDirectory`
- `description`: Creates a directory with owner-only permissions, enforcing the mode on non-Windows platforms.
- `params.path`: Directory to create.
- `returns`: Resolves once the directory exists.

### `packages/deliverables/workspace-changes/src/store.ts:removeRecord`
- `description`: Deletes one stored record; a missing record is not an error.
- `params.directory`: The Session's durable directory.
- `params.seq`: The announcing event's sequence.
- `returns`: Resolves once the record is gone.

### `packages/deliverables/workspace-changes/src/store.ts:pruneRoot`
- `params.isLive`: Marks a Session directory as in use; live Sessions are never pruned and count toward the retention budgets.

### `packages/document/office-kit/src/outputs.ts:OutputReservation`
- `description`: Claim on a pending output save held by an owner until `finish` releases it.

### `packages/document/office-kit/src/outputs.ts:OutputStore.reserve`
- `description`: Registers a pending output for an owner so it can be awaited before the store is torn down.
- `params.owner`: Name of the party holding the reservation.
- `returns`: The reservation; call `finish` to release it.

### `packages/document/office-kit/src/outputs.ts:OutputStore.save`
- `params.signal`: Cancels the save while it waits its turn or runs.
- `params.reservation`: Reservation taken earlier with `reserve`; when omitted the save takes and releases its own.

### `packages/experimental/browser-use-runtime/src/mcp.ts:rejectSecretBearingEndpoint`
- `description`: Rejects an attach-mode browser endpoint that carries credentials, a query string or a fragment, because the provider passes it as a process argument.
- `params.config`: Browser MCP configuration to check.
- `returns`: Nothing; throws when the endpoint is not acceptable.

### `packages/experimental/browser-use-stagehand-native/src/error.ts:StagehandTimeoutError`
- `description`: Failure of a Stagehand Worker request that did not answer within its time limit.

### `packages/experimental/browser-use-stagehand-native/src/worker-rpc.ts:request`
- `params.timeoutMs`: Time in milliseconds after which the request fails with a `StagehandTimeoutError`; omitted means no time limit.

### `packages/experimental/client-ui-voice-input/src/client/audio.ts:recordingLimitSeconds`
- `description`: Computes the longest recording in seconds that satisfies both the duration limit and the audio byte limit of 16-bit 16 kHz mono PCM after a 44-byte header.
- `params.maxDurationSeconds`: Configured duration limit.
- `params.maxAudioBytes`: Configured size limit of the encoded recording.
- `returns`: The recording limit in seconds.

### `packages/experimental/creation-recommendations/src/generated-bundle.ts:BundleLimits`
- `description`: Limits on a generated bundle: entry count, directory depth, total bytes, bytes per file and bytes of metadata files.

### `packages/experimental/creation-recommendations/src/generated-bundle.ts:BUNDLE_LIMITS`
- `description`: Default limits applied to a generated bundle.

### `packages/experimental/creation-recommendations/src/generated-bundle.ts:validateGeneratedBundle`
- `params.limits`: Size and depth limits to enforce; defaults to `BUNDLE_LIMITS`.

### `packages/experimental/forge-session/src/commands.ts:commandEvidence`
- `params.source`: Shell command text to inspect.

### `packages/experimental/forge-session/src/commands.ts:createdChangeRequestUrls`
- `description`: Collects the distinct change request URLs a creation command printed, one per output line.
- `params.text`: Command output.
- `returns`: The distinct URLs in order of appearance.

### `packages/experimental/forge-session/src/limiter.ts:Limiter`
- `description`: Concurrency limiter with a bounded wait queue.

### `packages/experimental/forge-session/src/limiter.ts:Limiter.run`
- `description`: Runs a task when a slot is free or after waiting for one, rejecting when too many tasks are already waiting or the signal aborts.
- `params.task`: Work to run.
- `params.signal`: Cancels the wait for a slot.
- `returns`: The task result.

### `packages/experimental/forge-session/src/reference-lookup.ts:observeReferences`
- `params.limiter`: Bounds how many reference lookups run at once and how many may wait.

### `packages/experimental/timeline/src/controls.ts:oldestResolvedAnnotation`
- `description`: Finds the first annotation that already has a reply or a control outcome.
- `params.annotations`: Annotations ordered oldest first.
- `returns`: The index of that annotation, or -1 when none is resolved.

### `packages/experimental/webhook-github/src/body.ts:readBoundedUtf8Body`
- `params.timeoutMs`: Time in milliseconds the whole body may take to arrive before the request fails with 408; omitted means no limit.

### `packages/experimental/webhook-github/src/handler.ts:DEFAULT_MAX_CONCURRENT_REQUESTS`
- `description`: Default number of webhook requests handled at once.

### `packages/experimental/webhook-github/src/handler.ts:DEFAULT_BODY_TIMEOUT_MS`
- `description`: Default time in milliseconds a request body may take to arrive.

### `packages/experimental/webhook-github/src/handler.ts:DEFAULT_REPLAY_WINDOW_MS`
- `description`: Default time in milliseconds a delivery id is remembered to refuse a replayed delivery.

### `packages/experimental/webhook/src/journal.ts:MAX_JOURNAL_ENTRIES`
- `description`: Most deliveries the journal holds at once; a full journal drops a settled one or refuses a new delivery.

### `packages/experimental/webhook/src/journal.ts:MAX_REPLAY_ATTEMPTS`
- `description`: Number of interrupted replays after which a delivery is marked failed for a rule.

### `packages/experimental/webhook/src/journal.ts:JournalRuleState`
- `description`: Replay state of one delivery for one rule: attempts made, whether it is pending or failed, and the last error.

### `packages/experimental/webhook/src/journal.ts:JournalEntry`
- `description`: One journaled delivery with its id and the replay state per rule.

### `packages/experimental/webhook/src/journal.ts:WebhookJournal`
- `description`: Durable append-only journal of verified deliveries awaiting rule handling, replayed after a restart and compacted periodically.

### `packages/experimental/webhook/src/journal.ts:WebhookJournal.add`
- `description`: Records a verified delivery as pending for each rule, first dropping a settled delivery when the journal is full.
- `params.delivery`: Verified delivery to record.
- `params.rules`: Rule ids that must handle it.
- `returns`: The new journal entry id.

### `packages/experimental/webhook/src/journal.ts:WebhookJournal.pendingFor`
- `description`: Lists deliveries still pending for a rule.
- `params.rule`: Rule id.
- `returns`: The pending entries.

### `packages/experimental/webhook/src/journal.ts:WebhookJournal.attempt`
- `description`: Records one handling attempt of a delivery by a rule.
- `params.id`: Journal entry id.
- `params.rule`: Rule id.
- `returns`: The number of attempts recorded so far.

### `packages/experimental/webhook/src/journal.ts:WebhookJournal.done`
- `description`: Records that a rule finished a delivery.
- `params.id`: Journal entry id.
- `params.rule`: Rule id.

### `packages/experimental/webhook/src/journal.ts:WebhookJournal.fail`
- `description`: Records that a rule gave up on a delivery, keeping a truncated error.
- `params.id`: Journal entry id.
- `params.rule`: Rule id.
- `params.error`: Failure message.

### `packages/experimental/webhook/src/types.ts:WebhookRuntimeConfig`
- `description`: Runtime configuration of the webhook service: the absolute journal file path, or false to run without a journal.

### `packages/experimental/webworker-runtime/src/storage/image-gzip.ts:inflateDigestedArchive`
- `description`: Inflates a lazily loaded image archive after checking that its bytes match the SHA-256 digest named in the archive file name.
- `params.bytes`: Compressed archive bytes.
- `params.name`: Archive file name carrying the expected SHA-256 digest.
- `returns`: The inflated bytes; throws when the name carries no digest or the digest differs.

### `packages/feedback/command-feedback/src/index.ts:SessionFeedbackService.sharing`
- `description`: Remote method reporting the Session feedback sharing policy and destination of the telemetry backend, or `none` when no backend is installed.
- `returns`: The sharing policy and optional destination.

### `packages/feedback/command-feedback/src/types.ts:SessionFeedbackSharingPolicy`
- `description`: How much Session data telemetry shares: full, feedback only, disabled, or none when no backend exists.

### `packages/feedback/command-feedback/src/types.ts:SessionFeedbackSharing`
- `description`: Feedback sharing policy with the destination feedback goes to, when the backend names one.

### `packages/fs/fs-local/src/fsio.ts:assertVersion`
- `description`: Throws `FS_STALE_VERSION` when the file's current version differs from the version the caller read.
- `params.absolutePath`: Absolute path of the file.
- `params.displayPath`: Path shown in the error message.
- `params.version`: Version the caller read.
- `params.verb`: Action named in the error message, such as edit or delete.
- `returns`: Resolves when the version still matches.

### `packages/fs/fs-local/src/fsio.ts:restoreEditedText`
- `description`: Maps an edit made on LF-normalized text back onto the raw text, keeping the original line endings outside the changed region.
- `params.original`: Raw text, its LF-normalized content and the detected line endings.
- `params.after`: Edited normalized text.
- `returns`: The edited raw text.

### `packages/fs/fs-patch/src/apply.ts:describeHunkFailure`
- `description`: Gives the human-readable reason for a hunk failure stage.
- `params.failure`: Failed hunk result.
- `returns`: The reason text.

### `packages/fs/tool-fs-search/src/search-core.ts:undecodableNote`
- `description`: Builds the note appended to search output that counts items whose bytes were not valid UTF-8.
- `params.count`: Number of affected items.
- `params.what`: Plural noun naming the items.
- `returns`: The note text.

### `packages/fs/tool-fs-search/src/search-core.ts:REPLACEMENT`
- `description`: The Unicode replacement character that stands for undecodable bytes in search output.

### `packages/goal/goal/src/domain.ts:GoalRefundChangeMeta`
- `description`: Durable change record of a refunded goal round: the goal snapshot, the rounds started after the refund, the round that was refunded, and the timestamps.

### `packages/goal/goal/src/index.ts:GoalService.refundRound`
- `description`: Gives back the most recent started round of the current goal, bumping its revision and recording a refund change; rejects a completed goal or one with no round started.
- `params.agent`: Agent owning the goal.
- `params.ref`: Reference of the goal to refund; must be current.
- `returns`: The goal view after the refund.

### `packages/goal/goal/src/runtime.ts:GOAL_REFUND_CHANGE_VERSION`
- `description`: Version number written to and accepted for goal refund change records.

- `packages/guard/timeout-policy/src/index.ts:TOOL_TIMEOUT_UNSETTLED` - Error code of a tool call that timed out and then did not stop within the settle grace period, so its effects are unknown. Module-private constant.

### `packages/host/authentication-token/src/browser-auth.ts:PENDING_API_PEER_DEADLINE_MS`
- `description`: Time in milliseconds an API peer may stay pending before it is dropped; also the basis of the retry delay sent to refused peers.

### `packages/host/authentication-token/src/browser-auth.ts:MAX_PENDING_API_PEERS`
- `description`: Most API peers that may be pending authorization at once.

### `packages/host/authentication-token/src/browser-auth.ts:BrowserAuth.create`
- `params.generation`: Credential generation the stored secret must carry; a stored secret of another generation is replaced with a fresh one.

### `packages/host/authentication-token/src/browser-auth.ts:BrowserAuth.sessions`
- `description`: Lists the browser sessions that have not expired.
- `returns`: Each live session with its id, authority and issue and expiry times.

### `packages/host/authentication-token/src/browser-auth.ts:BrowserAuth.revoke`
- `description`: Revokes one browser session and persists the change.
- `params.id`: Session id.
- `returns`: True when the session existed.

### `packages/host/authentication-token/src/browser-auth.ts:BrowserAuth.revokeAll`
- `description`: Replaces the signing secret and clears every session and revocation, invalidating all issued tokens.
- `returns`: Resolves once the new secret is stored.

### `packages/host/authentication-token/src/browser-auth.ts:BrowserAuth.settled`
- `description`: Waits for queued session persistence writes.
- `returns`: Resolves when the queued writes have settled.

### `packages/identity/anonymous-user-id/src/index.ts:getOrCreateAnonymousUserId`
- `params.options`: Environment, UUID generator and lock wait overrides; defaults read `process.env`.
- `returns`: The persisted anonymous user id, created and published under a lock when none exists.

### `packages/jobs/jobs/src/index.ts:JobRegistry.peek`
- `description`: Reads the job's pending output without advancing the model cursor.
- `params.id`: Job to read.
- `params.caller`: Reading session checked against the owner.
- `returns`: The same shape as `read`, leaving the cursor unchanged.

### `packages/knowledge/knowledge-source-git/src/index.ts:GitOperationOptions`
- `description`: Cancellation signal, time limit and output byte cap applied to git operations.

### `packages/knowledge/knowledge-source-git/src/index.ts:resolveGitSource`
- `params.options`: Cancellation signal, time limit and output cap for the git commands.

### `packages/knowledge/knowledge-source-git/src/index.ts:listSourceFiles`
- `params.options`: Cancellation signal, time limit and output cap for the git commands.

### `packages/llm/llm-deepseek/src/bounded-body.ts:MAX_ERROR_BODY_BYTES`
- `description`: Most bytes read from an error response body.

### `packages/llm/llm-deepseek/src/bounded-body.ts:MAX_FILES_RESPONSE_BYTES`
- `description`: Most bytes read from a Files API response body.

### `packages/llm/llm-deepseek/src/bounded-body.ts:BoundedText`
- `description`: Text read from a response and whether the body was cut at the byte limit.

### `packages/llm/llm-deepseek/src/bounded-body.ts:readBoundedText`
- `description`: Reads a response body as text up to a byte limit, cancelling the rest; a declared length above the limit yields no text and truncated.
- `params.response`: Response whose body is read.
- `params.maxBytes`: Most bytes to read.
- `returns`: The text read and whether it was truncated.

### `packages/llm/llm-deepseek/src/upload-index.ts:DeepSeekUploadIndex.markOwned`
- `description`: Records that this harness uploaded a file, dropping expired ownership entries.
- `params.scope`: File scope the upload belongs to.
- `params.fileId`: Uploaded file id.
- `params.expiresAt`: Time after which the ownership entry may be dropped.
- `params.now`: Current time.
- `returns`: Resolves once the index is saved.

### `packages/llm/llm-deepseek/src/upload-index.ts:DeepSeekUploadIndex.ownedIds`
- `description`: Lists the file ids of a scope that this harness uploaded, from ownership entries and cached upload records.
- `params.scope`: File scope to list.
- `returns`: The owned file ids.

### `packages/llm/llm-deepseek/src/upload-index.ts:DeepSeekUploadIndex.forgetOwned`
- `description`: Removes ownership entries for files that no longer exist.
- `params.scope`: File scope of the entries.
- `params.fileIds`: File ids to forget.
- `returns`: Resolves once the index is saved.

### `packages/llm/llm-pi-ai/src/no-redirect-fetch.ts:noRedirectFetch`
- `description`: Fetch that fails on any HTTP redirect instead of following it.
- `params.input`: Request URL or Request object.
- `params.init`: Request options; `redirect` is forced to `error`.
- `returns`: The response of the underlying fetch.

### `packages/sandbox/sandbox-windows-acl/src/grant.ts:AclWriteGrant.restore`
- `description`: Revokes the write grant of a SID on one path.
- `params.writeSid`: SID the grant was made to.
- `params.path`: Path to restore.
- `params.api`: Win32 bindings to use; defaults to the real ones.
- `returns`: True when a grant was removed.

### `packages/schedule/schedule/src/delivery-history.ts:MAX_HISTORY_PROMPT_BYTES`
- `description`: Most bytes of a sent prompt kept in a task's delivery history.

### `packages/schedule/schedule/src/runtime.ts:DELIVERY_STEP_TIMEOUT_MS`
- `description`: Time in milliseconds one step of a scheduled delivery may take before it is abandoned.

### `packages/schedule/schedule/src/update.ts:MAX_SCHEDULE_PROMPT_BYTES`
- `description`: Most bytes accepted for a schedule's prompt.

### `packages/schedule/tool-schedule/src/index.ts:Config`
- `description`: Schedule tool configuration; `administrator` enables the tools that manage every task.

### `packages/sdk/client/src/api.ts:HarnessSession.cancel`
- `description`: Cancels work running in this session, starting the harness when needed.
- `returns`: True when running work was cancelled.

### `packages/sdk/client/src/client.ts:NotificationOverflowError`
- `description`: Error raised to a notification subscriber whose queue overflowed.

### `packages/sdk/client/src/client.ts:HarnessClient.cancelSession`
- `description`: Requests cancellation of a session's running work and validates the acknowledgement.
- `params.sessionId`: Session to cancel.
- `returns`: True when work was cancelled; throws when the server returns no acknowledgement.

### `packages/sdk/protocol/src/transport.ts:MAX_FRAME_CHARS`
- `description`: Default largest accepted frame length in characters.

### `packages/sdk/protocol/src/transport.ts:MAX_OUTPUT_QUEUE_BYTES`
- `description`: Default cap on bytes queued for writing.

### `packages/sdk/protocol/src/transport.ts:JsonRpcLineTransportOptions`
- `description`: Overrides of the transport frame length and output queue limits.

### `packages/sdk/protocol/src/transport.ts:JsonRpcLineTransport.onFailure`
- `description`: Sets the handler called when the transport fails.
- `params.handler`: Called with the failure.

### `packages/sdk/protocol/src/types.ts:SessionCancelParams`
- `description`: Parameters of `session/cancel`: the session to cancel.

### `packages/sdk/protocol/src/types.ts:SessionCancelResult`
- `description`: Result of `session/cancel`: whether work was cancelled.

### `packages/sdk/server/src/server.ts:HarnessSdkJsonRpcServer.cancel`
- `description`: Aborts a session's pending admission and cancels its root Agent and descendant Agents; throws before initialization and during shutdown.
- `params.params`: Session to cancel.
- `returns`: Whether a running root Agent was cancelled.

### `packages/session-query/session-log-export/src/archive.ts:sessionLogExists`
- `description`: Tells whether a Session log can be opened for reading.
- `params.persistence`: Persistence backend to ask.
- `params.id`: Session id.
- `params.signal`: Cancels the lookup.
- `returns`: True when the log opens; false when it does not exist.

### `packages/session/session-persistence-jsonl/src/index.ts:JsonlRetention`
- `description`: Retention policy: delete Sessions older than `maxAgeDays`, sweeping every `intervalHours` when given.

### `packages/session/session-persistence-jsonl/src/index.ts:JsonlRetentionReport`
- `description`: Result of one retention sweep: deleted ids, skipped ids with the reason, and failures with their messages.

### `packages/session/session-telemetry-otel/src/redaction.ts:redactSecretsInString`
- `description`: Replaces every secret pattern match in a string.
- `params.text`: Text to redact.
- `returns`: The redacted text.

### `packages/session/session-telemetry-otel/src/redaction.ts:redactSecrets`
- `description`: Redacts secrets in every string of a nested value.
- `params.body`: Value to redact.
- `returns`: A redacted copy with the same shape.

### `packages/session/session-telemetry-otel/src/redaction.ts:localOnlyFeedbackBody`
- `description`: Reduces a feedback event body marked local-only to just that marker so no content is exported; other events pass through.
- `params.eventType`: Session event type.
- `params.body`: Event body.
- `returns`: The body to export.

### `packages/session/session-telemetry/src/index.ts:SessionTelemetryBackend.destination`
- `description`: Where the backend sends records, shown to users deciding whether to share feedback; absent when the backend names none.

### `packages/session/session-turn-outline/src/projection.ts:OUTLINE_ENTRY_LIMIT`
- `description`: Most turn entries the outline projection retains; older turns are dropped first.

### `packages/skill/tool-workspace-dependencies/src/index.ts:installPrimaryRuntime`
- `params.options`: `leaseWaitMs` bounds how long to wait for the install lease on the destination root.

### `packages/ssh/fs-ssh/src/outcome.ts:FsOutcomeUnknownError`
- `description`: Filesystem error raised when a remote operation may or may not have taken effect, for example after the connection dropped.

### `packages/ssh/fs-ssh/src/outcome.ts:FsOutcomeUnknownError.outcomeUnknown`
- `description`: Always true; marks that the outcome of the operation is unknown.

### `packages/ssh/fs-ssh/src/outcome.ts:isOutcomeUnknown`
- `description`: Narrows an error to `FsOutcomeUnknownError`.
- `params.error`: Value to test.
- `returns`: True when the error is an unknown-outcome filesystem error.

### `packages/storage/storage-json/src/dirsync.ts:fsyncDirectory`
- `params.path`: Directory whose entries are flushed to disk; a no-op on Windows.

### `packages/storage/storage-json/src/lock.ts:acquireUnitLock`
- `params.path`: Lock file path.
- `params.unit`: Unit name used in the error message.
- `returns`: A release function that frees the lock; throws a `locked` StorageError at once when another host holds it.

### `packages/storage/storage-sqlite/src/schema.ts:openDatabase`
- `params.exclusive`: When true, ownership of the database file is claimed so another process cannot open it; defaults to true.

### `packages/subagent/subagent/src/external-cwd.ts:assertExternalCwdWithinParentWorkspace`
- `description`: Requires an external subagent's working directory to resolve inside the parent Session's workspace or its writable roots, unless the sandbox allows full access.
- `params.ctx`: Host context providing the sandbox policy.
- `params.session`: Parent Session whose policy applies.
- `params.cwd`: Requested working directory.
- `returns`: Resolves when allowed; throws a SubagentError otherwise.

### `packages/subprocess/subprocess-local/src/retention.ts:MIN_SPILL_AGE_MS`
- `description`: Youngest age in milliseconds at which a spill file may be removed.

### `packages/subprocess/subprocess-local/src/retention.ts:activeSpills`
- `description`: Paths of spill files still being written, which sweeps never remove.

### `packages/subprocess/subprocess-local/src/retention.ts:SpillRetention`
- `description`: Spill file retention: maximum age, maximum total size and minimum age before removal.

### `packages/subprocess/subprocess-local/src/retention.ts:SweepOptions`
- `description`: Retention overrides plus the directory to scan, a directory to keep, and the current time.

### `packages/subprocess/subprocess-local/src/retention.ts:spillRetentionFromEnv`
- `description`: Reads the spill retention from `DSH_SUBPROCESS_SPILL_MAX_AGE_HOURS` (default 168) and `DSH_SUBPROCESS_SPILL_MAX_TOTAL_MB` (default 1024).
- `returns`: The retention policy.

### `packages/subprocess/subprocess-local/src/retention.ts:sweepSpillFiles`
- `description`: Removes spill files that exceed the retention age or total size, skipping active spills and the kept directory.
- `params.options`: Retention overrides, scan root, directory to keep and current time.
- `returns`: How many files were removed and how many bytes were freed.

### `packages/subprocess/subprocess-local/src/retention.ts:sweepSpillFilesThrottled`
- `description`: Starts a background sweep unless one is running or the sweep interval has not passed; failures are ignored.
- `params.keepDirectory`: Spill directory that must not be removed.

### `packages/team/agent-team/src/index.ts:TeamService.pendingHistoricalMessages`
- `description`: Lists historical team messages of the caller's root Session that are not yet acknowledged.
- `params.caller`: Agent whose root Session is read.
- `returns`: The unacknowledged messages.

### `packages/team/agent-team/src/index.ts:TeamService.recoverHistoricalMessage`
- `description`: Replays or discards one historical team message from the root Session and records the acknowledgement after a replayed send was accepted.
- `params.caller`: Agent calling; must be the root Agent.
- `params.messageId`: Message to recover.
- `params.decision`: Whether to replay or discard it.
- `params.signal`: Cancels the replay.
- `returns`: The outcome: replayed, discarded, already delivered or already resolved.

### `packages/telemetry/otel/src/session-log.ts:SESSION_LOG_DEFAULT_QUEUE_BYTES`
- `description`: Default cap on bytes the session log processor queues before it drops records.

### `packages/test-support/session-snapshot/src/staged-write.ts:StagedWrite`
- `description`: One file to publish: its path and content.

### `packages/test-support/session-snapshot/src/staged-write.ts:publishStagedWrites`
- `description`: Writes every file to a staging name first and publishes them together, removing staged files when any write fails.
- `params.writes`: Files to write.
- `returns`: Resolves once all files are published.

### `packages/util/atomic-write/src/index.ts:AtomicWriteSyncError`
- `description`: Error raised when a file was replaced but its directory could not be synced, so durability of the rename is not guaranteed.

### `packages/util/atomic-write/src/index.ts:AtomicWriteSyncError.path`
- `description`: Path of the file that was replaced.

### `packages/util/chunked-list/src/index.ts:ChunkedBlock`
- `description`: Sealed block of a chunked list: a leaf of values at height 0, or a node of child blocks.

### `packages/util/http-proxy/src/policy.ts:PROXY_FAIL_CLOSED_ENV`
- `description`: Name of the environment variable that makes an unusable proxy configuration fail instead of connecting directly.

### `packages/util/native-command/src/desktop-entry.ts:DesktopIconOptions`
- `description`: Cancellation signal and byte limit for reading a desktop entry icon.

### `packages/util/time/src/cron.ts:CRON_MAX_EXPRESSION_LENGTH`
- `description`: Longest accepted cron expression in characters.

### `packages/util/time/src/rrule.ts:RRULE_MAX_LENGTH`
- `description`: Longest accepted recurrence rule in characters.

### `packages/web/web-search-exa/src/provider.ts:mapExaResult`
- `description`: Maps one Exa result to a web search source, joining the non-empty highlights into the snippet and omitting empty fields.
- `params.result`: Exa result.
- `params.highlights`: Most highlights kept per result.
- `returns`: The web search source.

### `packages/web/web-search-exa/src/provider.ts:mapExaResponse`
- `description`: Maps an Exa search response to a web search result.
- `params.response`: Exa response.
- `params.highlights`: Most highlights kept per result.
- `returns`: The result with its sources, never truncated.

### `packages/web/web/src/safety.ts:WEB_RESPONSE_MAX_BYTES`
- `description`: Default most bytes read from a web provider response body.

### `packages/web/web/src/safety.ts:WEB_SEARCH_FIELD_MAX_CHARS`
- `description`: Most characters kept in each text field of a search result.

### `packages/web/web/src/safety.ts:WEB_SEARCH_URL_MAX_CHARS`
- `description`: Longest source URL kept in a search result; longer ones are dropped.

### `packages/web/web/src/safety.ts:readBoundedJson`
- `description`: Parses a response body as JSON, failing with `WEB_RESPONSE_TOO_LARGE` when it exceeds the byte limit.
- `params.response`: Response to read.
- `params.signal`: Cancels the read.
- `params.maxBytes`: Most bytes accepted.
- `returns`: The parsed JSON.

### `packages/web/web/src/safety.ts:boundSearchResult`
- `description`: Truncates each field of a search result to its limit and drops sources whose URL is too long.
- `params.result`: Search result to bound.
- `returns`: The bounded result.

### `packages/web/web/src/safety.ts:redactEndpoint`
- `description`: Removes credentials, query string and fragment from an endpoint URL for display.
- `params.endpoint`: Endpoint URL.
- `returns`: The redacted URL, or a placeholder when it cannot be parsed.

### `packages/api/session-controller/src/client/sessions/projection-store.ts:ProjectionValueStore.append`
- `description`: Appends items to a sequenced array row when the row currently holds exactly `from` items, advancing its sequence and notifying observers.
- `params.key`: Projection row key.
- `params.from`: Length the stored array must have for the append to apply.
- `params.items`: Items to append.
- `params.seq`: Sequence the row reaches after the append.
- `returns`: True when the append applied or the row already reached `seq`; false when the row is missing, is not sequenced, or does not hold `from` items.

### `packages/attachment/attachment-local/src/store.ts:reclaimAbandonedStaging`
- `description`: Deletes staging files older than the stale-staging age that no active upload uses.
- `params.staging`: Staging directory.
- `returns`: How many files were removed and how many bytes were freed.

### `packages/llm/llm-deepseek/src/file-store.ts:uploadIdentityOf`
- `description`: Derives the stable content identity of an image variant from its variant id and bytes, memoized per attachment object.
- `params.version`: Request image attachment.
- `returns`: The `sha256:` identity.

### `packages/schedule/schedule/src/runtime.ts:MAX_CONCURRENT_DELIVERIES`
- `description`: Most scheduled deliveries a runtime runs at once.

### `packages/session/session-projection/src/index.ts:ProjectionChangeEntry`
- `description`: One projection change of an event: a replacement value, or an append to the stored value.

### `packages/session/session-projection/src/index.ts:ProjectionEventChangeListener`
- `description`: Listener called with the projection changes one event produced and its sequence.

### `packages/session/session-projection/src/index.ts:ProjectionValueChange`
- `description`: One whole-value change of an event: the projection key and its new value.

### `packages/session/session-projection/src/index.ts:ProjectionAppendEventChangeListener`
- `description`: Listener called with the append-aware change entries one event produced and its sequence.

### `packages/session/session-projection/src/index.ts:SessionProjectionRegistry.onAppendEventChanged`
- `description`: Subscribes a listener to the append-aware change feed. The listener runs once per committed event that produced at least one change and receives each change as a replacement value or an append to the stored value. The subscription is an effect on the calling context's fiber; the returned disposer removes it.
- `params.listener`: Called with the Session, the event's change entries, and the event sequence. Errors thrown by a listener are collected and rethrown after every listener has run.
- `returns`: The exact disposer that unsubscribes.

### `packages/util/chunked-list/src/index.ts:chunkedListLength`
- `description`: Counts the values of a chunked list.
- `params.head`: The list, or undefined for the empty list.
- `returns`: The number of values.

### `packages/util/chunked-list/src/index.ts:chunkedListSuffix`
- `description`: Collects up to `count` values from the end of a chunked list.
- `params.head`: The list, or undefined for the empty list.
- `params.count`: Most values to collect.
- `returns`: The values, newest first.

### `packages/context/harness-dev-context/src/source-root.ts:isSourceCheckout`
- `description`: Reports whether a root is an editable DeepSeek Harness source checkout, by the presence of the workspace file and this package's manifest.
- `params.root`: Absolute installation or checkout root to test.
- `returns`: True for a source checkout; false for an installed distribution.

### `packages/context/harness-dev-context/src/source-root.ts:sourceRootContext`
- `description`: Describes the installation root for the runtime context, stating whether it is an editable checkout.
- `params.root`: Absolute installation or checkout root.
- `returns`: One sentence naming the root and its kind, with the root JSON-quoted.

### `packages/attachment/attachment-local/src/compression-limiter.ts:compressionFailure`
- `description`: Wraps a rejection reason as an Error: an Error passes through unchanged, and any other value becomes the cause of a new Error.
- `params.reason`: Rejection reason of a compression task or an admission error.
- `returns`: An Error suitable for promise rejection.

### `packages/attachment/attachment-local/src/compression-limiter.ts:CompressionQueueLimits`
- `description`: Bounds of the compression waiting line: the maximum number of waiting tasks and the maximum total of their input bytes.

### `packages/attachment/attachment-local/src/compression-limiter.ts:CompressionTaskOptions`
- `description`: Options of one compression task: an optional abort signal that drops the task while it waits for a slot, and the input bytes it counts against the queue byte bound.

### `packages/attachment/attachment-local/src/compression-limiter.ts:CompressionLimiter`
- `description`: Instance-owned FIFO limiter for asynchronous compression work: at most `concurrency` tasks run at once, and the waiting line is bounded by `queue`.

### `packages/attachment/attachment-local/src/compression-limiter.ts:CompressionLimiter.queued`
- `description`: Number of tasks waiting for a slot.

### `packages/attachment/attachment-local/src/compression-limiter.ts:CompressionLimiter.admit`
- `description`: Throws an image-queue-full error when admitting these tasks would exceed the waiting line's task or byte bound.
- `params.sizes`: Input byte sizes of the tasks to admit; only those beyond the free slots would wait.

### `packages/attachment/attachment-local/src/file-store.ts:verifiedStoredFilePath`
- `description`: Returns the stored file's path when it still matches the reference's size and SHA-256 digest, caching the verified identity so unchanged files are not rehashed; returns undefined when the file is missing or does not match.
- `params.root`: Root directory of the verbatim file store.
- `params.ref`: File reference whose stored object is checked.
- `returns`: The verified path, or undefined.

### `packages/client/ui-chat/src/client/contract/outline.ts:OutlinePage`
- `description`: One wire page of a Session outline: raw entries, whether older history remains, and the sequence to continue before.

### `packages/host/authentication-token/src/browser-auth.ts:BrowserSocket`
- `description`: The socket surface the browser-auth host tracks per session: whether it is destroyed, a way to destroy it, and a one-time close listener.

### `packages/host/authentication-token/src/browser-auth.ts:BrowserAuth.browserSessionId`
- `description`: Returns the session id of a request's browser cookie when the cookie authenticates and its payload is the current version; otherwise undefined.
- `params.request`: Connection trust request carrying the cookie.
- `params.secure`: Whether the request arrived over a secure transport; defaults to false.
- `returns`: The browser session id, or undefined.

### `packages/host/authentication-token/src/browser-auth.ts:BrowserAuth.trackSocket`
- `description`: Registers an open socket under its browser session so revoking the session can destroy it; already destroyed sockets are ignored, and the entry is removed on close.
- `params.sid`: Browser session id the socket belongs to.
- `params.socket`: Socket to track.

### `packages/schedule/schedule/src/storage.ts:promptIdOf`
- `description`: Content address of a delivery prompt: the hex SHA-256 digest of its text, which keys the stored prompt snapshot.
- `params.prompt`: Prompt text.
- `returns`: The hex digest.

### `packages/web/web-deepseek-api/src/fetch.ts:DeepSeekFetchApiProvider`
- `description`: Fetch page Markdown through the DeepSeek Fetch API. The API reports neither the target's status nor a final URL, and it fails when the target yields no usable content, so a success is the requested page retrieved with HTTP 200.

### `packages/subagent/subagent-freebuff/src/errors.ts:FREEBUFF_LOGIN_COMMAND`
- `description`: Command the user runs in a terminal to sign in to Freebuff.

### `packages/subagent/subagent-freebuff/src/errors.ts:FreebuffNotInstalledError`
- `description`: Failure raised when the configured Freebuff executable cannot be found.

### `packages/subagent/subagent-freebuff/src/errors.ts:FreebuffNotLoggedInError`
- `description`: Failure raised when Freebuff shows a login screen or its doctor report says it is signed out; carries the detection reason.

### `packages/subagent/subagent-freebuff/src/errors.ts:FreebuffFailureStage`
- `description`: Run stage in which a Freebuff failure happened.

### `packages/subagent/subagent-freebuff/src/errors.ts:FreebuffFailureCategory`
- `description`: Classification of a Freebuff run failure.

### `packages/subagent/subagent-freebuff/src/errors.ts:FreebuffRunFailure`
- `description`: Product failure of a Freebuff run, carrying stage, category, optional detail and the last screen rows.

### `packages/subagent/subagent-freebuff/src/index.ts:Config`
- `description`: Deployment-owned executable, working directory, environment, login-check and timing settings of the Freebuff provider.

### `packages/subagent/subagent-freebuff/src/login.ts:LoginCheck`
- `description`: Inputs of the pre-run `freebuff doctor --json` sign-in check.

### `packages/subagent/subagent-freebuff/src/login.ts:DoctorCommand`
- `description`: Argument vector and extra environment used to run `freebuff doctor --json`.

### `packages/subagent/subagent-freebuff/src/login.ts:launchCommand`
- `description`: Builds the command line for a Freebuff process, wrapping a Windows `.cmd` or `.bat` shim in the command interpreter and passing its quoted path through an environment variable so a path with spaces survives; refuses shim arguments that hold a cmd metacharacter.
- `params.executable`: Freebuff executable path or name.
- `params.args`: Arguments that follow the executable.
- `params.platform`: Platform the command will run on.
- `params.comSpec`: Command interpreter path on Windows; cmd.exe when undefined.
- `returns`: The argument vector and the environment entries it needs.

### `packages/subagent/subagent-freebuff/src/login.ts:doctorCommand`
- `description`: Builds the doctor command line, wrapping a Windows `.cmd` or `.bat` shim in the command interpreter and passing its quoted path through an environment variable so a path with spaces survives.
- `params.executable`: Freebuff executable path or name.
- `params.platform`: Platform the command will run on.
- `params.comSpec`: Command interpreter path on Windows; cmd.exe when undefined.
- `returns`: The argument vector and the environment entries it needs.

### `packages/subagent/subagent-freebuff/src/login.ts:freebuffLoginFailure`
- `description`: Extracts the signed-out finding id from a `freebuff doctor --json` report.
- `params.stdout`: Raw doctor standard output, possibly with text before the JSON.
- `returns`: The signed-out finding id, or undefined when signed in or the report is unreadable.

### `packages/subagent/subagent-freebuff/src/login.ts:checkFreebuffLogin`
- `description`: Runs `freebuff doctor --json` with a timeout and reports whether Freebuff is signed out; a doctor that cannot run never blocks the run.
- `params.check`: Spawn function, executable, working directory, environment, limits and platform of the check.
- `returns`: The signed-out finding id, or undefined when signed in or undeterminable.

### `packages/subagent/subagent-freebuff/src/run.ts:DEFAULT_DISPOSE_GRACE_MS`
- `description`: Default grace in milliseconds between terminal termination tiers.

### `packages/subagent/subagent-freebuff/src/run.ts:DEFAULT_STARTUP_TIMEOUT_MS`
- `description`: Default milliseconds to wait for the ready screen and for the task to start.

### `packages/subagent/subagent-freebuff/src/run.ts:DEFAULT_IDLE_MS`
- `description`: Default milliseconds without a busy marker before a turn counts as finished.

### `packages/subagent/subagent-freebuff/src/run.ts:DEFAULT_TIMEOUT_MS`
- `description`: Default wall-clock limit in milliseconds for one published run.

### `packages/subagent/subagent-freebuff/src/run.ts:FreebuffRunSpec`
- `description`: Resolved executable, environment, timing and terminal-spawn settings of one Freebuff run.

### `packages/subagent/subagent-freebuff/src/run.ts:taskText`
- `description`: Flattens the text blocks of a one-shot prompt into the task pasted into Freebuff, removing control characters and rejecting tasks Freebuff would run as a command.
- `params.prompt`: Prompt content blocks; only text blocks are accepted.
- `returns`: The trimmed task text.

### `packages/subagent/subagent-freebuff/src/run.ts:startFreebuffRun`
- `description`: Starts Freebuff in a pseudo-terminal, waits for the ready screen, pastes the task and returns a published run that settles with the answer or a diagnosed failure.
- `params.request`: Subagent start request with the prompt and the startup abort signal.
- `params.spec`: Resolved run settings.
- `returns`: The published subagent run.

### `packages/subagent/subagent-freebuff/src/screen.ts:CLIPPED_NOTICE`
- `description`: Notice prefixed to a screen-derived answer whose task echo is not visible, meaning the answer may be incomplete.

### `packages/subagent/subagent-freebuff/src/screen.ts:ScreenState`
- `description`: Classification of one rendered Freebuff screen: login, trust prompt, blocker, busy and ready.

### `packages/subagent/subagent-freebuff/src/screen.ts:ScreenAnswer`
- `description`: Answer text read from the screen and whether it may be clipped because the task echo was not found.

### `packages/subagent/subagent-freebuff/src/screen.ts:classifyScreen`
- `description`: Classifies a rendered screen; with a task, markers are searched only in the last rows and ignored when they are part of the task.
- `params.text`: Rendered screen text.
- `params.task`: Task pasted into Freebuff, used to ignore markers echoed from it.
- `returns`: The classification of the screen.

### `packages/subagent/subagent-freebuff/src/screen.ts:screenAnswer`
- `description`: Extracts the reply that follows the task echo from the screen, dropping footer, status line, trailing boxes and chrome rows.
- `params.text`: Rendered screen text.
- `params.task`: Task whose first line anchors the start of the reply.
- `returns`: The reply text and whether it may be clipped.

### `packages/subagent/subagent-freebuff/src/screen.ts:FreebuffScreen`
- `description`: Headless terminal emulator that renders Freebuff output into screen text and answers terminal queries.

### `packages/subagent/subagent-freebuff/src/screen.ts:FreebuffScreen.write`
- `description`: Feeds terminal output into the emulator.
- `params.data`: Raw terminal output.

### `packages/subagent/subagent-freebuff/src/screen.ts:FreebuffScreen.text`
- `description`: Renders the current screen as text with trailing blank rows removed.
- `returns`: The screen text, one line per row.

### `packages/subagent/subagent-freebuff/src/screen.ts:FreebuffScreen.dispose`
- `description`: Releases the emulator and its reply subscription.

### `packages/subagent/subagent-freebuff/src/transcript.ts:FreebuffTranscript`
- `description`: Completion state, answer and error of the last turn in a Freebuff chat transcript.

### `packages/subagent/subagent-freebuff/src/transcript.ts:freebuffConfigDir`
- `description`: Resolves the Freebuff config directory that holds chat transcripts.
- `params.env`: Environment that may name an absolute config directory.
- `params.home`: Home directory used when none is configured.
- `returns`: The config directory path.

### `packages/subagent/subagent-freebuff/src/transcript.ts:findChatFile`
- `description`: Finds the transcript file of this run: the newest chat modified since the run started whose last user message equals the task.
- `params.configDir`: Freebuff config directory.
- `params.cwd`: Working directory whose basename keys the chat folder.
- `params.since`: Earliest modification time in milliseconds.
- `params.task`: Task pasted into Freebuff.
- `returns`: The transcript path, or undefined when no chat matches.

### `packages/subagent/subagent-freebuff/src/transcript.ts:readTranscript`
- `description`: Reads the last turn of a chat transcript; an error followed by a complete answer counts as recovered.
- `params.file`: Transcript file path.
- `returns`: The turn state, or undefined when the file is unreadable or has no user message.


### `packages/llm/llm-freebuff/src/adapter.ts:MODEL_ID`
- `description`: Model id of the single Freebuff route; Freebuff chooses the actual model itself.

### `packages/llm/llm-freebuff/src/adapter.ts:FreebuffReplay`
- `description`: Replay state carried on a Freebuff assistant message: the chat transcript id and the id of the last chat row already consumed.

### `packages/llm/llm-freebuff/src/adapter.ts:promptOf`
- `description`: Extracts the prompt of a turn: the text of the newest user-sourced message, rejecting slash and bang commands, images and files.
- `params.messages`: Request messages of the turn.
- `returns`: The prompt text to paste into the Freebuff terminal.

### `packages/llm/llm-freebuff/src/adapter.ts:resumeOf`
- `description`: Finds the Freebuff replay state on the newest assistant message that carries one.
- `params.messages`: Request messages of the turn.
- `returns`: The replay state, or undefined for a new conversation.

### `packages/llm/llm-freebuff/src/adapter.ts:FreebuffAdapter`
- `description`: Model adapter that runs each turn through a pooled Freebuff terminal and streams the rendered chat rows.

### `packages/llm/llm-freebuff/src/auth.ts:PROVIDER`
- `description`: Provider id of the Freebuff route and quota source.

### `packages/llm/llm-freebuff/src/auth.ts:FREEBUFF_KEY`
- `description`: Credential key under which the Freebuff sign-in marker is stored.

### `packages/llm/llm-freebuff/src/auth.ts:FreebuffAccount`
- `description`: Freebuff sign-in state: reads the doctor report, keeps the stored marker in step with it, and offers the sign-in authorization flow.

### `packages/llm/llm-freebuff/src/auth.ts:FreebuffAccount.status`
- `description`: Reports whether Freebuff is signed in, reusing a recent doctor result unless forced.
- `params.force`: Skip the cached doctor result.
- `returns`: The doctor status.

### `packages/llm/llm-freebuff/src/auth.ts:FreebuffAccount.reconcile`
- `description`: Runs the doctor check and writes or clears the stored sign-in marker to match it.
- `params.force`: Skip the cached doctor result.
- `returns`: The doctor status.

### `packages/llm/llm-freebuff/src/auth.ts:FreebuffAccount.signedOut`
- `description`: Clears the stored sign-in marker after a turn found Freebuff signed out.

### `packages/llm/llm-freebuff/src/auth.ts:FreebuffAccount.flow`
- `description`: Builds the authorization flow that runs `freebuff login` in a terminal and relays its first sign-in URL.
- `returns`: The authorization flow registered under the Freebuff credential key.

### `packages/llm/llm-freebuff/src/chat.ts:ChatBlock`
- `description`: One block of a Freebuff chat message as stored in the chat transcript file.

### `packages/llm/llm-freebuff/src/chat.ts:ChatMessage`
- `description`: One row of a Freebuff chat transcript file.

### `packages/llm/llm-freebuff/src/chat.ts:squash`
- `description`: Collapses every whitespace run to one space and trims the ends.
- `params.text`: Text to normalize.
- `returns`: The normalized text.

### `packages/llm/llm-freebuff/src/chat.ts:chatsDirOf`
- `description`: Locates the directory where Freebuff stores the chats of a working directory, keyed by its basename.
- `params.configDir`: Freebuff configuration directory.
- `params.cwd`: Working directory of the session.
- `returns`: The chats directory path.

### `packages/llm/llm-freebuff/src/chat.ts:listChats`
- `description`: Lists the chat ids stored in a chats directory.
- `params.chatsDir`: Directory returned by `chatsDirOf`.
- `returns`: The chat ids, or an empty list when the directory is missing.

### `packages/llm/llm-freebuff/src/chat.ts:readChat`
- `description`: Reads the messages of one chat transcript.
- `params.chatsDir`: Directory returned by `chatsDirOf`.
- `params.chatId`: Id of the chat.
- `returns`: The messages, or undefined when the transcript is missing or unreadable.

### `packages/llm/llm-freebuff/src/chat.ts:rowText`
- `description`: Extracts the visible text of a chat row.
- `params.message`: Chat row.
- `returns`: The row text, empty when the row has none.

### `packages/llm/llm-freebuff/src/chat.ts:bindChat`
- `description`: Finds the chat created for a submitted prompt by comparing the chat list with the list taken before submitting.
- `params.chatsDir`: Directory returned by `chatsDirOf`.
- `params.before`: Chat ids that existed before the prompt was submitted.
- `params.prompt`: Prompt text that was submitted.
- `returns`: The id of the chat whose first user row matches the prompt, or undefined when none does yet.

### `packages/llm/llm-freebuff/src/doctor.ts:DoctorStatus`
- `description`: Sign-in state from `freebuff doctor --json`: signed in, signed out, or unknown when the report cannot be read.

### `packages/llm/llm-freebuff/src/doctor.ts:DoctorRun`
- `description`: Inputs of one `freebuff doctor --json` run: the spawn function, executable, arguments, environment, working directory and time limits.

### `packages/llm/llm-freebuff/src/doctor.ts:doctorFindings`
- `description`: Reads the `auth.ok` finding from a doctor report.
- `params.stdout`: Standard output of `freebuff doctor --json`.
- `returns`: Signed in or signed out when the report names the finding, otherwise unknown.

### `packages/llm/llm-freebuff/src/doctor.ts:doctorStatus`
- `description`: Runs `freebuff doctor --json` with time and output limits and classifies the result.
- `params.run`: Doctor run inputs.
- `returns`: The doctor status; unknown when the run fails, times out or prints no report.

### `packages/llm/llm-freebuff/src/environment.ts:FreebuffTiming`
- `description`: Time limits and polling intervals of the Freebuff terminal driver, in milliseconds, plus the terminal cap.

### `packages/llm/llm-freebuff/src/environment.ts:DEFAULT_TIMING`
- `description`: Production values of `FreebuffTiming`.

### `packages/llm/llm-freebuff/src/environment.ts:FreebuffEnvironment`
- `description`: Everything the driver needs from the host: executable lookup, arguments, environment, configuration and home directories, platform, command interpreter and timing.

### `packages/llm/llm-freebuff/src/environment.ts:defaultEnvironment`
- `description`: Builds the production environment from the subprocess service and the process environment.
- `params.ctx`: Plugin context providing the subprocess service.
- `returns`: The Freebuff environment.

### `packages/llm/llm-freebuff/src/environment.ts:probeSignIn`
- `description`: Runs the doctor check inside the given environment.
- `params.ctx`: Plugin context providing the subprocess service.
- `params.environment`: Freebuff environment.
- `params.signal`: Aborts the check.
- `returns`: The doctor status.

### `packages/llm/llm-freebuff/src/install.ts:PLUGIN_NAME`
- `description`: Plugin name; also the settings namespace fallback of the Freebuff provider row.

### `packages/llm/llm-freebuff/src/install.ts:DEFAULT_IDLE_REAP_MS`
- `description`: Default idle time in milliseconds after which an unused Freebuff terminal is closed.

### `packages/llm/llm-freebuff/src/index.ts:Config`
- `description`: Deployment-owned idle timeout of pooled Freebuff terminals.

### `packages/llm/llm-freebuff/src/install.ts:FreebuffWiring`
- `description`: Host seams of the plugin: the Freebuff environment, the terminal spawner and, optionally, the quota reading options.

### `packages/llm/llm-freebuff/src/install.ts:installFreebuff`
- `description`: Registers the Freebuff sign-in flow, model route, settings row, terminal pool disposer and, when wired, the quota source.
- `params.ctx`: Plugin context.
- `params.config`: Plugin configuration.
- `params.wiring`: Host seams.
- `returns`: The terminal pool.

### `packages/llm/llm-freebuff/src/launch.ts:FreebuffLaunch`
- `description`: Argument vector and extra environment entries that start Freebuff.

### `packages/llm/llm-freebuff/src/launch.ts:freebuffLaunch`
- `description`: Builds the Freebuff command line, wrapping a Windows `.cmd` or `.bat` shim in the command interpreter and rejecting arguments that the interpreter would reinterpret.
- `params.executable`: Freebuff executable path or name.
- `params.args`: Arguments after the executable.
- `params.platform`: Platform the command runs on.
- `params.comSpec`: Command interpreter path on Windows.
- `returns`: The argument vector and the environment entries it needs.

### `packages/llm/llm-freebuff/src/launch.ts:freebuffConfigDir`
- `description`: Locates the Freebuff configuration directory.
- `params.env`: Process environment.
- `params.home`: User home directory.
- `returns`: The `.config/manicode` directory under the home directory.

### `packages/llm/llm-freebuff/src/launch.ts:stripAnsi`
- `description`: Removes terminal escape sequences from text.
- `params.text`: Text that may contain escape sequences.
- `returns`: The text without escape sequences.

### `packages/llm/llm-freebuff/src/markdown.ts:RenderedBlock`
- `description`: One text or reasoning block rendered from Freebuff chat rows.

### `packages/llm/llm-freebuff/src/markdown.ts:renderBlocks`
- `description`: Renders chat rows as text and reasoning blocks, fencing tool output and bounding its length.
- `params.messages`: Chat rows to render.
- `returns`: The rendered blocks in row order.

### `packages/llm/llm-freebuff/src/quota.ts:SESSION_URL`
- `description`: Freebuff usage endpoint that reports the hours left.

### `packages/llm/llm-freebuff/src/quota.ts:QUOTA_TTL_MS`
- `description`: Milliseconds a usage reading is reused before the endpoint is called again.

### `packages/llm/llm-freebuff/src/quota.ts:TokenReader`
- `description`: Reads the Freebuff token on demand; resolves undefined when Freebuff is not signed in.

### `packages/llm/llm-freebuff/src/quota.ts:FreebuffQuotaOptions`
- `description`: Token reader plus optional clock, time zone and not-configured message for the Freebuff quota source.

### `packages/llm/llm-freebuff/src/quota.ts:OpenHour`
- `description`: An open hour of the day: freebucks held and the instant it ends.

### `packages/llm/llm-freebuff/src/quota.ts:Usage`
- `description`: Daily and monthly freebucks parsed from the usage endpoint.

### `packages/llm/llm-freebuff/src/quota.ts:hours`
- `description`: Converts freebucks to whole hours at five freebucks per hour.
- `params.freebucks`: Freebucks; negative values count as zero.
- `returns`: The whole hours.

### `packages/llm/llm-freebuff/src/quota.ts:hoursMinutes`
- `description`: Formats freebucks as hours and minutes, rounding down to the minute.
- `params.freebucks`: Freebucks; negative values count as zero.
- `returns`: The time as `h:mm`.

### `packages/llm/llm-freebuff/src/quota.ts:decodeToken`
- `description`: Decodes a Credential Manager blob as UTF-8, or as UTF-16 when it contains NUL characters, and trims it; `quota.ts:decodeToken` returns the `authToken` field of a JSON blob.
- `params.blob`: Credential blob bytes.
- `returns`: The token, or undefined when `quota.ts:decodeToken` finds none.

### `packages/llm/llm-freebuff/src/quota.ts:credentialTarget`
- `description`: Builds the Credential Manager target under which Freebuff stores its token.
- `params.configDir`: Freebuff config directory for `quota.ts:credentialTarget` and `quota.ts:windowsTokenReader` (`FREEBUFF_CONFIG_DIR` or the default under the home directory).
- `returns`: The target name.

### `packages/llm/llm-freebuff/src/quota.ts:RunPowerShell`
- `description`: Runs a PowerShell script with extra environment entries and resolves its standard output.

### `packages/llm/llm-freebuff/src/quota.ts:ExecFile`
- `description`: The part of `execFile` that the PowerShell runner uses, so a test can stand in for the process.

### `packages/llm/llm-freebuff/src/quota.ts:runPowerShell`
- `description`: Runs an encoded PowerShell script with extra environment entries and resolves its standard output; any failure rejects with a fixed message that carries neither output nor error text.
- `params.script`: PowerShell source.
- `params.env`: Environment entries added to the process environment.
- `params.exec`: Process runner, `execFile` by default.
- `returns`: The standard output.

### `packages/llm/llm-freebuff/src/quota.ts:windowsTokenReader`
- `description`: Builds a token reader that reads the Freebuff credential through PowerShell without putting the token on a command line.
- `params.configDir`: Freebuff config directory for `quota.ts:credentialTarget` and `quota.ts:windowsTokenReader` (`FREEBUFF_CONFIG_DIR` or the default under the home directory).
- `params.run`: PowerShell runner.
- `returns`: The token reader.

### `packages/llm/llm-freebuff/src/quota.ts:defaultQuotaOptions`
- `description`: Chooses the production quota options: the Windows reader, or a reader that reports not configured on other platforms.
- `params.platform`: Platform name.
- `returns`: The quota options.

### `packages/llm/llm-freebuff/src/quota.ts:parseUsage`
- `description`: Validates the usage endpoint response and extracts its daily and monthly freebucks.
- `params.body`: Parsed JSON body.
- `returns`: The usage, or undefined when the daily figures are missing.

### `packages/llm/llm-freebuff/src/quota.ts:quotaWindows`
- `description`: Builds the `daily` and `monthly` quota windows, counting the unused part of each open hour as still left.
- `params.usage`: Parsed usage.
- `params.now`: Current time in milliseconds.
- `returns`: The windows keyed by window id.

### `packages/llm/llm-freebuff/src/quota.ts:createFreebuffQuotaSource`
- `description`: Builds the quota source that reads the hours left with the stored Freebuff token and never exposes it.
- `params.options`: Token reader and clock seams.
- `returns`: The quota source with id `freebuff`.

### `packages/llm/llm-freebuff/src/screen.ts:ScreenState`
- `description`: Classification of the Freebuff terminal screen: login prompt, trust prompt, blocker text, busy and ready.

### `packages/llm/llm-freebuff/src/screen.ts:classifyScreen`
- `description`: Classifies the rendered terminal text.
- `params.text`: Rendered screen text.
- `params.task`: Prompt being worked on, used to tell the echoed prompt from a blocker.
- `returns`: The screen state.

### `packages/llm/llm-freebuff/src/screen.ts:FreebuffScreen`
- `description`: Headless terminal that renders the Freebuff screen from its output stream.

### `packages/llm/llm-freebuff/src/screen.ts:FreebuffScreen.write`
- `description`: Feeds terminal output to the renderer.
- `params.data`: Output bytes as text.

### `packages/llm/llm-freebuff/src/screen.ts:FreebuffScreen.text`
- `description`: Reads the rendered screen.
- `returns`: The visible rows joined by newlines.

### `packages/llm/llm-freebuff/src/screen.ts:FreebuffScreen.dispose`
- `description`: Releases the renderer.

### `packages/llm/llm-freebuff/src/session.ts:TurnRequest`
- `description`: One turn: the prompt, working directory, resumed chat and consumed marker, and the abort signal.

### `packages/llm/llm-freebuff/src/session.ts:TurnEnd`
- `description`: Chat id and consumed marker at the end of a turn, returned in the replay state.

### `packages/llm/llm-freebuff/src/session.ts:PoolOptions`
- `description`: Environment, account, terminal spawner and idle reap time of the terminal pool.

### `packages/llm/llm-freebuff/src/session.ts:FreebuffSession`
- `description`: One Freebuff terminal and chat bound to a harness session.

### `packages/llm/llm-freebuff/src/session.ts:FreebuffSession.chatId`
- `description`: Id of the Freebuff chat this session writes to, once known.

### `packages/llm/llm-freebuff/src/session.ts:FreebuffSession.consumed`
- `description`: Id of the last chat row already returned to the harness.

### `packages/llm/llm-freebuff/src/session.ts:FreebuffSession.lastUsed`
- `description`: Time in milliseconds of the last use, for idle reaping and eviction.

### `packages/llm/llm-freebuff/src/session.ts:FreebuffSession.running`
- `description`: Whether a turn is running.

### `packages/llm/llm-freebuff/src/session.ts:FreebuffSession.occupied`
- `description`: Whether the session holds or is starting a terminal.

### `packages/llm/llm-freebuff/src/session.ts:FreebuffSession.turn`
- `description`: Runs one turn: submits the prompt and yields rendered blocks as chat rows appear.
- `params.request`: The turn to run.
- `returns`: An async generator of rendered blocks that returns the chat id and consumed marker.

### `packages/llm/llm-freebuff/src/session.ts:FreebuffSession.kill`
- `description`: Terminates the terminal of this session.

### `packages/llm/llm-freebuff/src/session.ts:FreebuffPool`
- `description`: Bounded set of Freebuff sessions keyed by harness session id.

### `packages/llm/llm-freebuff/src/session.ts:FreebuffPool.session`
- `description`: Finds or creates the session of a harness session id.
- `params.id`: Harness session id.
- `returns`: The Freebuff session.

### `packages/llm/llm-freebuff/src/session.ts:FreebuffPool.makeRoom`
- `description`: Closes the least recently used idle terminal when the terminal cap is reached.
- `params.requester`: Session that needs a terminal and is never evicted.
- `returns`: A promise that settles once a slot is free.

### `packages/llm/llm-freebuff/src/session.ts:FreebuffPool.dispose`
- `description`: Terminates every terminal and stops idle reaping.
