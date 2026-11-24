# session-query code notes

### `packages/session-query/session-query/src/observation.ts:SessionObservationReader::inspectCheckpoint`

Finding 345, dormant hop. Gives a domain read of a stored Session's cached projection rows without loading its log: it stats the Session for its header (identity witness, no event read), asks `SessionProjectionCache::cachedRows` for the lifecycle-matching record, and runs the caller's synchronous getter over those rows. It returns undefined when the Session is attached (a live cut is already cheap and fresher than any row), when persistence or the cache is not mounted, or when no record matches; the getter may also return undefined. Callers treat undefined as "use `inspect`", which is the complete path. The rows are the cache's own in-memory record and are never copied, so the getter must not mutate or retain them. Stat failures map to the same query errors as `inspect`.
