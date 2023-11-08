In v0.12, we introduced support for delaying requests to simulate slow responses from APIs.

> ℹ️ By default the [LatencyPlugin](../technical-reference/LatencyPlugin.md) is disabled. To enable the plugin, open the [m365proxyrc](../technical-reference/m365proxyrc.md) file, search for the plugin object and change the `enabled` property to `true`. The plugin will be enabled the next time you start proxy.

The minimum and maximum delay (in milliseconds) which can be added to a response is defined in the configuration object.

```json
"latencyPlugin": {
  "minMs": 200,
  "maxMs": 10000
}
```

When a response is delayed, proxy displays the total duration it was delayed for in the console output.
