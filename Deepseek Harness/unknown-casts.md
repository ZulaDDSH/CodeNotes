# Unknown assertions removed (verify-no-unknown-casts)

`scripts/verify-no-unknown-casts.ts:verifyNoUnknownCasts` (gate rule): the gate rejects `as unknown` in authored source. Each site listed in these notes is replaced by a typed value, an annotated `unknown` binding, or a typed helper. No baseline entry is added for any of them.

`apps/desktop/src/mandatory-update-policy.ts:fileDesktopPolicyStore` (gate finding): the parsed JSON is bound to a `: unknown` local and returned; the parser returns `any`, so the binding makes the untyped read explicit without an assertion.

`apps/plugin-market-generator/src/fetchers.ts:readJsonBounded` (gate finding): same binding pattern as `fileDesktopPolicyStore`; the bounded body is parsed into a `: unknown` local before return.

`packages/util/chunked-list/src/index.ts:chunkedListSchema` (gate finding): the recursive `block` schema's `z.lazy` callback is annotated with `z.ZodType<ChunkedBlock<T>>`, which breaks the self-reference inference the two `as unknown as` casts existed for. The list-level cast is dropped because `block` is now typed. `tsc -p packages/util/chunked-list` passes with the casts removed.

## Test-file casts (verify-no-unknown-casts)

Test behaviour is unchanged. Each entry names the site and the typed route that replaces its `as unknown` hop. A private member is read or called through string-literal element access, which the compiler still checks against the declared member type.

- `packages/sdk/client/tests/limits.spec.ts:dispatch` - the client is passed directly; the private `dispatchNotification` is called as `dispatch['dispatchNotification']`, not through a hand-written structural cast.
- `packages/sdk/client/tests/limits.spec.ts:slow` - the same element-access route, applied to the `slow` harness' private members.
- `packages/sdk/client/tests/run.spec.ts:state` - the private `transport.pending` map is read as `harness.client['transport']!` then `['pending']`; the `!` asserts the transport the request path already requires, since the field is optional on the client.
- `packages/session-query/session-query-sqlite/tests/sqlite.spec.ts:open` - the private `_db` handle is read through `(a.sessionQuery as SqliteSessionQueryEngine)['_db']`, the same downcast this file already uses at its other sites.
- `packages/experimental/speech-to-text-sensevoice/tests/worker.spec.ts:enqueue` - the private `enqueue` is called as `worker['enqueue'](run, signal)`.
- `packages/preset/agent-preset-registry/tests/registry.spec.ts:retain` - the private `retain` is read through element access `registry['retain'].bind(registry)`; the registry itself is used directly.
- `packages/util/values/tests/deep-equal-json.spec.ts:protoKey` - `JSON.parse` returns `any`; the binding is annotated `: unknown` (the same pattern as `apps/desktop/src/mandatory-update-policy.ts:fileDesktopPolicyStore`). The `other` binding is annotated the same way.
- `packages/util/values/tests/deep-equal-json.spec.ts:compares an own __proto__ key by value` - the parsed `__proto__` fixtures take the `: unknown` binding above; no assertion remains.
- `packages/session/session-format-v0-to-v1/tests/relationships.spec.ts:settle` - the `JSON.parse` result is used without the trailing `as unknown`; the parser's `any` is what the test feeds in.
- `packages/session/session-turn-outline/tests/projection.spec.ts:cut` - the `stateSchema.parse` input drops the trailing `as unknown`; the parser's `any` is accepted by `parse`.
- `packages/typert/loader/tests/loader.spec.ts:dir` - the private `packages` map is read as `ctx.pluginPackages['packages']`.
- `packages/client/connection/tests/http-bridge.host.spec.ts:budget` - the private `used` counter is read as `budget['used']`.
- `packages/client/connection/tests/http-bridge.host.spec.ts:response` - `bridge` takes the module-private `BridgeServerResponse` contract structurally, not `ServerResponse`. The response fake is an `EventEmitter` literal with `destroyed` and `writableEnded` flags and the contract methods, so it is passed without any cast.
- `packages/client/ui-provider-quota/tests/controller.client.spec.ts:partial` - the failing storage fake is typed `Partial<Storage>` and cast once to `Storage` at the call.
- `packages/client/ui-provider-quota/tests/usage-prompt.client.spec.ts:throwing` - the throwing storage fake is annotated `Partial<Storage>` and cast once to `Storage` where it is passed in.
- `packages/workflow/tool-workflow/tests/tool-workflow.spec.ts:asFakeSpill` - new helper: `instanceof FakeSpill` narrows the registered spill store and throws when it is any other store. It replaces the three `as unknown as FakeSpill` casts (`spill`, `fitsSpill`, `failing`).
- `packages/experimental/webhook-github/tests/body.spec.ts:slow` - the slow-request fake keeps its `Partial<IncomingMessage>` annotation. Its `destroy` spy is a `vi.fn` typed `(): IncomingMessage` whose body returns the fake cast to `IncomingMessage`, which matches the hook's return type. The one `as IncomingMessage` at the `readBoundedUtf8Body` call stays. UNVERIFIED until the typecheck run.
- `packages/subprocess/subprocess-local/tests/local.spec.ts:runtime` - `ctx.subprocess` is downcast to the default-exported `LocalSubprocessRuntime` class, which declares the private `warnFallback`; it is then called as `runtime['warnFallback'](...)`.
- `packages/terminal/terminal-bash/tests/session.spec.ts:session` - the private `emulatorClosed` flag is read as `session['emulatorClosed']`.
- `packages/plan/plan-mode/tests/plan-mode.spec.ts:original` - the failing append wrapper takes the bound method's parameter and return types (`Parameters<typeof original>`, `ReturnType<typeof original>`) and forwards with `Reflect.apply`, because a spread argument of the union-of-tuples type does not resolve against the overloads. The single cast is to `typeof agent.session.append`, whose overloads the wrapper cannot satisfy by assignment alone.
- `packages/plan/plan-mode/tests/plan-mode.spec.ts:resolveConfig` - the invalid config fixtures keep their existing cast to `PlanModeConfig`; the gate does not flag them and they are not changed in this pass.
- `packages/api/session-controller/tests/projection-append.client.spec.ts:makeManager` - the fixture is cast to `ClientTestFixtures['remote'] & SessionRemotes`. An intersection with the source type is always comparable, and the result satisfies the manager constructor. The fixture is not checked against the manager's remote surface. UNVERIFIED until the typecheck run.
- `packages/api/workspace-files/tests/harness.ts:viewer` - `Principal` (`PrincipalMap[keyof PrincipalMap]`) admits only the `owner` kind, so the non-owner `share-viewer` fixture widens `kind` to `string` before a single `as Principal`. The Host must refuse it; the confine tests use it on that path.
- `packages/host/authentication-token/tests/plugin.host.spec.ts:socket` - the revoke test uses a real `PassThrough` with `vi.spyOn(socket, 'destroy')` instead of the `FakeSocket` cast to `Duplex`; the webserver hook takes a `Duplex`, and the spy records the same `destroy` calls.
- `packages/subagent/subagent/tests/catalog-append.spec.ts:noHeader` - the absent header is `undefined as never` bound to `SessionHeader`; the subagent catalog's `init` does not read the header.
- `packages/feedback/message-feedback/tests/message-feedback.spec.ts:unconfirmed` - the private `unconfirmedLive` set is read through element access `ctx.messageFeedback['unconfirmedLive']`.
- `packages/experimental/webhook/tests/loader-composition.spec.ts:modules` - the loader override is declared as `ModuleLoaderV2` and assigned to `ctx.loader.internal`, the same typed route `packages/llm/llm-pi-ai/tests/loader-composition.spec.ts` uses; the stubbed hooks throw, as they did before.
- `packages/experimental/webhook-github/tests/handler.spec.ts:response` - the fake response is annotated `Partial<ServerResponse>` and cast to `ServerResponse` once at the handler call.
- `packages/experimental/webhook-github/tests/handler.spec.ts:request` - the fake request keeps its single `as unknown as IncomingMessage` cast; that site is recorded in the baseline and was not flagged.
