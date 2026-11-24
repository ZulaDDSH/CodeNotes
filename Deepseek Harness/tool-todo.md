# tool-todo code notes

## README states the per-turn projection reset (finding 151)

- `packages/todo/tool-todo/src/index.ts:apply` (the `todos` projection fold) deliberately returns `null` on `turn/start`, and `packages/todo/tool-todo/tests/projection.spec.ts` pins it. The finding is a documentation mismatch, so the README summary and call-mechanics text now separate the historical `todo/write` events (kept in the log) from the current-list projection (cleared each turn); the code is unchanged.

## Stale-list reminder counts only successful writes (finding 415)

- `packages/todo/tool-todo/src/index.ts:apply` resets the stale-list counter inside `todo_write`'s `execute`, immediately after `todo/write` is appended, instead of in the `tools/post-execute` listener. The listener used to clear the counter for every `todo_write` call, so a rejected update (for example duplicate trimmed content) postponed the six-call reminder indefinitely while the old list stayed unfinished. The reset now runs only when a new snapshot was committed; the post-execute listener still returns early for `todo_write` so a failed write is neither counted nor a reset.
- Covered by `packages/todo/tool-todo/tests/stale-reminder.spec.ts` (a rejected write between probes does not suppress the reminder).
