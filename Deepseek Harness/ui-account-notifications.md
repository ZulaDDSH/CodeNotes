# ui-account-notifications code notes

## Expiry is scheduled without a poll (finding 380)

- `packages/client/ui-account-notifications/src/client/notification-controller.ts:createNotificationController` - schedules one timer for the earliest `displayUntil` among published notices and, when it fires, removes the expired notices and promotes the next card, so an expired card leaves the stack without waiting for a poll, rerender or interaction. The timer delay is capped to the browser timer maximum and re-armed by the sweep.
- `packages/client/ui-account-notifications/src/client/AccountNotifications.tsx:NoticeCard` - rechecks liveness before running a non-dismiss action, so a card still on screen at its deadline does not open Account settings or report a click.

## Content refresh is bounded (finding 382)

- `packages/client/ui-account-notifications/src/client/notification-controller.ts:createNotificationController::refreshContent` - runs at most four content reads at once and publishes each completed update as it arrives, so one slow card does not hold back the others; a newer language generation stops new reads from starting, while requests already in flight are fenced by the generation check when they settle.
- The card count is not capped on the client: the server owns unconsumed notifications, and the stack shows only live cards, so a cap would hide server-owned notices. This remains a documented limit.

## Superseded reads are cancelled (findings 382, 384, remaining part)

### `packages/client/ui-account-notifications/src/client/notification-controller.ts:createNotificationController::restartPolling`
Each poll owns an AbortController. `stopPolling` aborts it, so a refresh or sign-out cancels the list read of the poll it replaces instead of only fencing the reply. The generation check stays for replies that settle anyway. The replacing read is issued after the abort, so on the Host it joins no request that is being cancelled.

### `packages/client/ui-account-notifications/src/client/notification-controller.ts:createNotificationController::refreshContent`
Each content generation owns an AbortController. A newer language generation, a lifecycle end, and the next refresh abort the previous generation's in-flight content reads, so repeated language changes leave at most one generation's four reads in flight rather than four per change. An aborted read is not counted as a failure and does not log.

### `packages/client/ui-account-notifications/src/client/index.ts:apply`
`read` and `readContent` pass the controller's signal to the Remote call, which cancels the Host request. A Remote call that takes a `signal` parameter is exempt from the gateway's unary deadline (`packages/api/gateway/tests/unary-deadline.client.spec.ts`), so `withDeadline` combines the controller signal with `AbortSignal.timeout(30_000)`; without it a never-answered request would hold its poll forever and polling would never resume.
