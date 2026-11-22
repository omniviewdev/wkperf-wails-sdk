# WKPerf Wails SDK

Instrument selected calls in a stock Wails v3 app and correlate browser and Go handler spans by transaction ID. The Go host package uses only the standard library. The browser adapter works with the Wails runtime already in your app; neither Wails nor its runtime needs a patch.

## Install

From your app's Go module, set `WKPERF_WAILS_SDK_VERSION` to a published SDK tag or commit and run:

```sh
go get "github.com/omniviewdev/wkperf-wails-sdk/pkg/wailsprobe@$WKPERF_WAILS_SDK_VERSION"
```

Follow the [integration guide](docs/WAILS-SDK.md) to register the Go service and load the matching browser adapter. If migrating from the WKPerf module, change the Go import and replace any old frontend adapter with `wailsprobe.ClientJS()` or `pkg/wailsprobe/client.js` from the same SDK revision. Regenerate service bindings if your frontend uses them. The default Wails binding prefix is now `github.com/omniviewdev/wkperf-wails-sdk/pkg/wailsprobe.Service`.

Enable the probe only in development builds with trusted frontend content. It does not collect WebKit Inspector samples. Use [WKPerf](https://github.com/omniviewdev/wkperf) to record or analyze separate WebKit evidence.

## Verify

With Go 1.26.1 or newer and Node.js 22 or newer:

```sh
GOWORK=off go vet ./...
GOWORK=off go test ./... -count=1
node --test examples/wailsprobe/client.test.mjs
```

The [stock Wails example](examples/wailsprobe) is a separate Go module. On macOS with a local GUI session and `jq`, run the [native smoke](docs/WAILS-SDK.md#run-the-native-smoke-with-public-tools):

```sh
(cd examples/wailsprobe && GOWORK=off go build -mod=readonly -o /tmp/wailsprobe-smoke .)
sh examples/wailsprobe/smoke.sh /tmp/wailsprobe-smoke
```
