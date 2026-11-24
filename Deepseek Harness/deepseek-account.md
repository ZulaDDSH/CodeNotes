# deepseek-account code notes

## Remote logout outcome is reported (finding 59)

- `packages/credentials/deepseek-account/src/logout.ts:revokeAccount` - returns `revoked` after one accepted request, `failed` when every bounded attempt failed, and `unconfirmed` when the provider lifetime ended first (shutdown, aborted request or delay). It still never touches local account state and never restores the grant.
- `packages/credentials/deepseek-account/src/index.ts:DeepSeekAccount::revoke` - records the outcome as `revocationStatus` (`none`, `pending`, then the outcome), emits `deepseek-account/revocation` with it, and logs a warning for `failed` and `unconfirmed` so an operator can revoke the token issuer-side. The token is deliberately not retained for a later retry: persisting or holding a revoked-from-local credential would reverse the sign-out guarantee; the issuer-side recovery path is the documented retry.
- `packages/credentials/deepseek-account/src/index.ts:DeepSeekAccount::getRevocationStatus` - Host-only (not a Remote) read of the latest outcome. Status is process memory only; a restart forgets it, like the pending revocation itself.

## packages/credentials/deepseek-account/src/index.ts:Events['deepseek-account/revocation']
The event JSDoc carries description, `@mode emit` and `@param outcome` because the cordis catalog generator rejects events without them. Listeners are not awaited (emit mode).

## Account notification polling has a floor (finding 381)

- `packages/credentials/deepseek-account/src/notifications.ts:readNotifications` - raises a server `next_poll_after` below `MIN_NEXT_POLL_SECONDS` (10 seconds) up to that floor instead of rejecting the whole list, so a misconfigured server cannot turn polling into a request loop while the notifications it sent still display. NEEDS NOMAD: confirm the 10 second floor against the server's documented poll policy; the default is a conservative choice, not a server-published value.

## Duplicate notification IDs are dropped at the Host (finding 383)

- `packages/credentials/deepseek-account/src/notifications.ts:readNotifications` - keeps the first entry for each notification ID and skips later entries with that ID under a diagnostic. The client keys cards and its displayed-report set by ID, so two entries sharing an ID would let one card's dismissal or display report stand in for the other.

## Concurrent list reads share one account request (finding 384)

- `packages/credentials/deepseek-account/src/index.ts:DeepSeekAccount::sharedNotificationRead` - joins an in-flight list read when the grant, lifetime and client headers all match, and clears the slot when that read settles, so it never serves a finished response from memory.
- `packages/credentials/deepseek-account/src/index.ts:DeepSeekAccount::getNotifications` - uses the shared read, so repeated settings refreshes produce one account request at a time per grant and locale.
- The client-side controller still starts a new read on every refresh and fences the superseded reply; `ui-account-notifications` tests pin that a refresh does not wait for an earlier read, so the client does not coalesce. The superseded RPC still completes on the Host, which is bounded by the request timeout.

## Notification reads are cancellable (findings 382, 384, remaining part)

### `packages/credentials/deepseek-account/src/index.ts:DeepSeekAccount::getNotifications`
Takes a trailing `signal` (Typert forwards cancellation only for a parameter named `signal` typed as the global `AbortSignal`). The caller's signal ends that caller's wait and, through `sharedNotificationRead`, its claim on the shared account request. A cancelled caller returns null instead of an error because the client that cancelled it has already discarded the reply.

### `packages/credentials/deepseek-account/src/index.ts:DeepSeekAccount::getNotificationContent`
Takes a trailing `signal` and adds it to the request signal, so a content read abandoned by a newer language generation or sign-out stops its account request instead of running to the request timeout.

### `packages/credentials/deepseek-account/src/index.ts:DeepSeekAccount::sharedNotificationRead`
The shared request now has its own controller and a count of joined callers. A caller leaving does not abort the request while another caller still waits (another window, or the refresh that replaced the cancelled poll with the same grant and headers); the request aborts only when the last caller has left, and the slot is cleared at once so a caller arriving after that starts a fresh request instead of joining one that is being cancelled.

### `packages/credentials/deepseek-account/src/notifications.ts:readNotifications`
The list is cut to the first `MAX_NOTIFICATIONS` (50) valid, distinct entries in server order (finding 382). Notifications past the cap stay unconsumed on the server and are offered by a later poll once earlier ones are consumed or expire, so no notice is lost, only deferred. This replaces the earlier decision to leave the card count unbounded: the cap bounds the content refresh fan-out, the card list and the per-card scan, and a signed-in user sees one foreground card at a time anyway.
