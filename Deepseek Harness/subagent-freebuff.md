# subagent-freebuff code notes

### `packages/subagent/subagent-freebuff/src/run.ts:startFreebuffRun`

`classifyScreen` reports `loginRequired` when a login marker such as `to login:` appears in the bottom rows, which an answer that ends with "run this command to login:" also does. The run therefore treats the login screen as signed out only when `loginRequired` is true and `ready` is false: the real login screen has no `/model to change` or `for history` footer, while every answer screen has one.

Drives the installed `freebuff` binary, an interactive OpenTUI application with no prompt argument, one-shot mode, ACP mode or server mode, through `ctx.subprocess.spawnTerminal`. It waits for the ready screen, pastes the task with bracketed-paste markers so embedded newlines do not submit early, presses Enter, and waits for the turn to end. The Freebuff SDK is not used: Freebuff keeps its token in the OS keychain on purpose and refuses use through the SDK, so the provider never reads, copies or proxies any credential.

Does nothing to hide that an agent is driving Freebuff. Freebuff's server may refuse agent-driven prompts under its usage policy ("Driven by another agent"); the run then fails with that `userError` text in its diagnostic, and the provider is only usable with the Freebuff developer's permission. The provider must not add anything that disguises the prompt or evades that check.

Never confirms an admission prompt (`Enter: confirm and send`, `Enter: take over`): those spend session capacity or take over another terminal's session, so the run fails with the prompt named. The trust prompt is answered `n` unless `trustAgents` is configured, in which case `--trust-agents` is passed and the prompt never appears. Loading project agents executes repository-supplied configuration, so the default stays off. Blockers and the login screen are checked again after the first busy observation, but only on frames that are not busy, so a model answer that quotes a marker does not fail the run.

Ends the turn when no busy status has been seen for `idleMs` and either the ready footer is back after a busy observation or the transcript marks the last answer complete. A product error in the transcript fails the run only once the screen is quiet, so a recoverable error that Freebuff retries is not reported early. Busy status is polled rather than awaited because the TUI redraws continuously (ads, spinner), so screen equality never settles. A paused `ask_user` question or a free-tier waiting room have no marker the provider recognises and end at `timeoutMs` or `startupTimeoutMs`. The product error text is flattened and truncated to 400 characters in the diagnostic.

Waits through one shared interrupt signal instead of racing each wait against the process failure: terminal exit, local cancel and request abort all abort it once, `pause` wakes at once, and writes race against it. The request signal given to `spawnTerminal` only covers startup; after publication the manager cancels through the run handle, so cancellation tests dispose the run. An abort that arrives while `spawnTerminal` is pending is rechecked after it resolves and terminates the new terminal.

Does not set `FREEBUFF_CONFIG_DIR`: the keychain entry is keyed by the credentials path, so relocating the config directory would sign the child out.

Starts a Windows `.cmd` or `.bat` shim from `resolveExecutable` through `packages/subagent/subagent-freebuff/src/login.ts:launchCommand`, the same way the doctor call does (`doctorCommand` is `launchCommand` with `doctor --json`): `ComSpec /d /c %DSH_FREEBUFF_DOCTOR_EXE% <args>` with the quoted shim path in that environment variable, because with `/s` or a literal quoted path cmd strips quotes and splits a path containing a space or parenthesis. An argument holding a cmd metacharacter (`& | < > ^ % "`) or a carriage return or line feed is refused for a shim because cmd would act on it. `packages/llm/llm-freebuff/src/launch.ts:freebuffLaunch` does the same job; the two packages keep their own copy so neither depends on the other. Before the shim wrapping the pseudo-terminal path was exercised once with the installed `freebuff.cmd` started directly; the wrapped form is covered by tests only. The pseudo-terminal handle reports pid 0, and the Windows session is outside Job containment, so descendant cleanup relies on the handle's own termination.

### `packages/subagent/subagent-freebuff/src/screen.ts:classifyScreen`

Classifies the rendered terminal screen, never raw escape bytes. Markers observed once on Freebuff 0.2.36 with a trivial task: the footer rows contain `/model to change` and `for history` in every frame, the busy status row reads `thinking... <n>s ■ Esc`, and the idle status row reads `✕ End session`. Markers taken from the Freebuff CLI source and not seen on a running binary: `Press ENTER to login`, `to login:`, `Waiting for login` and `Please log in again` (login), `Enter: confirm and send`, `Enter: take over`, `Enter: choose another model` and `Enter: retry` (session admission), and `Load and run these?` (project-agents trust prompt). A marker that changes in a Freebuff release makes a run fail with the screen tail in its diagnostic instead of hanging, because every wait is bounded.

Given the task searches login and admission markers only in the last 16 non-empty rows, where the status row and footer render, and ignores a marker that is part of the squashed task, because the echoed task and the model's own answer fill the upper rows and may quote a marker. Busy detection matches only the status row (`thinking...` or `working...` as a whole row, or a trailing `■ Esc`), not any line that mentions the words, and readiness uses the last 8 rows. Without a task, as during startup, the whole screen is searched. The row counts are a guess from the TUI layout, not a measurement.

### `packages/subagent/subagent-freebuff/src/screen.ts:FreebuffScreen`

Renders through `@xterm/headless` with the same lazy loading as `packages/terminal/terminal-bash/src/session.ts`, and constructs the emulator with `allowProposedApi: true` because reading `buffer` throws without it. The emulator's replies to terminal queries (cursor position, device attributes) are written back to the pseudo-terminal because OpenTUI queries terminal capabilities at startup.

### `packages/subagent/subagent-freebuff/src/run.ts:taskText`

Removes every control character except newline and tab before the task is pasted. A task containing the bracketed-paste end sequence `ESC [ 201 ~` would otherwise end the paste early and type the rest of the task as keystrokes into the TUI. It also rejects a task whose first character is `/` or `!`, because Freebuff runs those as a slash command or a shell command; the caller must rephrase instead of the provider prefixing text. A leading `@` is not rejected.

### `packages/subagent/subagent-freebuff/src/login.ts:freebuffLoginFailure`

Reads `freebuff doctor --json` and maps the finding ids `auth.signed-out`, `auth.empty`, `auth.corrupt` and `auth.unreadable` to a not-logged-in failure. The doctor report states file facts only and never contains the token. A doctor that cannot run or parse does not block the run, because the TUI login screen detected by `classifyScreen` is the authoritative signal and also catches an expired token that doctor reports as signed in.

### `packages/subagent/subagent-freebuff/src/transcript.ts:readTranscript`

Reads the persisted chat transcript `chat-messages.json` under `<config dir>/projects/<basename(cwd)>/chats/<chat id>/`, a JSON array of chat messages that starts with a `mode-divider` ai message. It is the preferred source of the final answer and of turn completion (`isComplete`, `userError`) because the TUI emits no machine-readable done signal. It reads no credential file; the config directory is only the parent of chat folders. An error followed by a later complete ai message counts as recovered and does not fail the run.

### `packages/subagent/subagent-freebuff/src/transcript.ts:findChatFile`

Pins a chat only when its last user message equals the squashed task, newest first among chats modified since the run started. Without that check a concurrent Freebuff session in the same directory could supply this run's answer. The chats folder is located by `basename(cwd)` only, because the real chat file format has not been verified to record its working directory (UNVERIFIED), so two projects with the same folder name share a chats directory; the exact-task and modified-since checks are what keep a concurrent run in a same-named directory from being mistaken for this one.

### `packages/subagent/subagent-freebuff/src/screen.ts:screenAnswer`

Is the fallback answer when no transcript is found. It cuts at the footer or status row, drops trailing boxed regions (the ad and survey boxes under the answer), and starts after the last row containing the first 30 characters of the task. When that anchor is not on the screen the answer is clipped to the viewport; `packages/subagent/subagent-freebuff/src/screen.ts:CLIPPED_NOTICE` is then prefixed so the caller knows it may be incomplete.

### `packages/subagent/subagent-freebuff/src/run.ts:productErrorDetail`

Carries Freebuff's own error or refusal text whole. It removes control characters other than newline and tab, which could drive the parent's terminal, and trims the ends, but never shortens or rewrites the words, because a usage-policy refusal has to reach the user exactly as Freebuff wrote it. The text can be long; the diagnostic is not capped.

### `packages/subagent/subagent-freebuff/src/index.ts:FreebuffProvider.start`

Refuses any parent sandbox other than `danger-full-access` with `PARENT_SANDBOX_MISMATCH`. Freebuff offers no sandbox or approval switch, so a read-only or workspace-write parent cannot be honored; refusing mirrors the Codex bypass mode instead of running an unconfined child silently.

### `packages/subagent/subagent-freebuff/src/errors.ts:FreebuffRunFailure`

Carries the stage (`startup`, `turn`, `process`, `teardown`), the category and a detail, and keeps the rendered screen rows in `screenTail` instead of in the message, so the error text that reaches the parent never contains screen content that may quote the task or the model's answer.

### `packages/subagent/subagent-freebuff/src/index.ts:FreebuffProvider`

`onError` logs the failure at warn level without screen content and writes the last screen rows (`screenTail`, 12 rows) at debug level only, because those rows can hold the user's task and the model's answer.

### `packages/subagent/subagent-freebuff/src/login.ts:launchCommand`

Starts a Windows `.cmd` or `.bat` shim through `ComSpec /d /c` with the quoted path in `DSH_FREEBUFF_DOCTOR_EXE`, and refuses any argument containing `& | < > ^ % "` or a CR or LF.

### `packages/subagent/subagent-freebuff/src/login.ts:checkFreebuffLogin`

Spawns `freebuff doctor --json` with bounded stdout and stderr and a timeout. A doctor that cannot start, times out or prints nothing parseable answers undefined, so it never blocks a run; only a signed-out finding does.

