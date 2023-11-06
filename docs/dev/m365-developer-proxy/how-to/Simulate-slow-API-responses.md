In v0.12, we introduced support for delaying requests to simulate slow responses from APIs.

> ℹ️ By default the [LatencyPlugin](./LatencyPlugin) is disabled. To enable the plugin, open the [m365proxyrc](https://github.com/microsoft/m365-developer-proxy/wiki/m365proxyrc) file, search for the plugin object and change the `enabled` property to `true`. The plugin will be enabled the next time you start proxy.

The minimum and maximum delay (in milliseconds) which can be added to a response is defined in the configuration object.

```json
"latencyPlugin": {
  "minMs": 200,
  "maxMs": 10000
}
```

When a response is delayed, proxy will display the total duration it was delayed for in the console output.
