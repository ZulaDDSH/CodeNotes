# session-projection-cache code notes

### `packages/session/session-projection-cache/src/index.ts:SessionProjectionCache::cachedRows`

Finding 345, dormant hop. The raw-row sibling of `cachedSnapshot`: it matches the record by the header-only lifecycle identity and returns the stored rows plus the record's inherited cut, without running any wire view (a view of `subagentCatalog` would materialize every entry). A seeded header with a record that stores no cut is refused, because the cut cannot be proven. Rows are as stale as the last durable checkpoint but never ahead of the log (`write` flushes the log before the row lands). The returned rows are the in-memory record itself; callers must not mutate them.
