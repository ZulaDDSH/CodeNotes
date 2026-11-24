# ui-settings-general code notes

## Product analytics control (finding 375)

### `packages/client/ui-settings-general/src/client/ProductAnalyticsRow.tsx:ProductAnalyticsRow`
General settings row that turns Desktop product analytics on or off. It follows `ChatMemoryRow`: the accepted value comes from the `product-analytics` config form, a failed write keeps the shown value and says so, and the switch is disabled while a write is in flight. Unlike Chat memory, the displayed value defaults to false while the form has not loaded, because collection is opt-in; a row that briefly read true would misstate what is collected. The description lists the data categories (interaction events, device and account identifiers, session identifiers, model and plugin names, app version, operating system and hardware), names only what is guaranteed never sent (prompts and replies), and warns that a package name or version entered in the plugin installer is included in install events because finding 378 (a credential-shaped installer input reaching analytics) is not fixed here, so the row does not claim that API keys are excluded, says that the default is off, and says that turning it off stops new events at once while events already queued for delivery can still be sent. The strings are in every dictionary of `locales.ts`, translated for each language.

### `packages/client/ui-settings-general/src/client/index.ts:apply`
Registers the row with `configForms.whileServed(['product-analytics'])`, so it exists only while the Host serves that form: Desktop mounts the service, ordinary Web does not. The write is `set('enabled', value)` on the form and throws when the Host refuses it.

### `packages/client/ui-settings-general/src/client/search-entries.ts:ROWS`
Adds the row to Settings search by its `index.productAnalytics.*` copy.
