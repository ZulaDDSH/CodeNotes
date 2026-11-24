# inspector code notes

`packages/experimental/inspector/src/worker/bridge/endpoint.ts:InspectorEndpoint.handleHttp`: the loopback listener is reachable by every local user, and the CDP socket path embeds the random target id as its only secret, so an unauthenticated `/json`, `/json/list` or `/json/version` response would hand that id to any local process (finding 208). Discovery therefore requires the target id as a `token` query parameter and answers 404 otherwise; the id is delivered only through the Host's logged DevTools URL and the in-process handle, never through an unauthenticated route.

`packages/experimental/inspector/src/host/inspection/network.ts:installFetchObserver`: the wrapper rebuilds a `Request` from the caller's input and init, which cannot carry Undici's non-standard `dispatcher` init option, so the original fetch is also given that option explicitly (finding 209).

`packages/experimental/inspector/src/host/inspection/network.ts:captureBody`: the capture branch of `response.clone()` is a tee, which queues chunks for the caller's branch whenever the capture branch reads, so a paused caller leaves up to the per-response body limit queued before the capture branch cancels at the limit (finding 207). A genuine fix needs the caller's body to be replaced by a capturing pass-through, which would lose the original Response identity, so the per-response bound is documented in the README instead.

`packages/experimental/inspector/src/host/inspection/network.ts:installFetchObserver` (forwarded dispatcher): the inspector forwards a dispatcher the caller supplied in the request's `init`, unchanged, so the transport choice stays the caller's. The caller builds its dispatcher through the proxy package, which routes by the resolved proxy policy; the inspector therefore carries the `proxy-exempt` annotation (verify-no-bare-dispatcher) and builds none of its own.

- `packages/experimental/inspector/src/host/inspection/network.ts:installFetchObserver` - the observer forwards the caller dispatcher unchanged because that dispatcher already routes by the proxy policy; the `proxy-exempt` marker records this for the bare-dispatcher gate.
