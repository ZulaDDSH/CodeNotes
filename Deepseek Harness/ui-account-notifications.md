# ui-account-notifications code notes

## Expiry is scheduled without a poll (finding 380)

- `packages/client/ui-account-notifications/src/client/notification-controller.ts:createNotificationController` - schedules one timer for the earliest `displayUntil` among published notices and, when it fires, removes the expired notices and promotes the next card, so an expired card leaves the stack without waiting for a poll, rerender or interaction. The timer delay is capped to the browser timer maximum and re-armed by the sweep.
- `packages/client/ui-account-notifications/src/client/AccountNotifications.tsx:NoticeCard` - rechecks liveness before running a non-dismiss action, so a card still on screen at its deadline does not open Account settings or report a click.

## Content refresh is bounded (finding 382)

- `packages/client/ui-account-notifications/src/client/notification-controller.ts:createNotificationController::refreshContent` - runs at most four content reads at once and publishes each completed update as it arrives, so one slow card does not hold back the others; a newer language generation stops new reads from starting, while requests already in flight are fenced by the generation check when they settle.
- The card count is not capped on the client: the server owns unconsumed notifications, and the stack shows only live cards, so a cap would hide server-owned notices. This remains a documented limit.
