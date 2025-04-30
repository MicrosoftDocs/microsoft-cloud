---
title: LatencyPlugin
description: LatencyPlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 04/30/2025
---

# LatencyPlugin

Delays responses by a random number of milliseconds from the configured range.

:::image type="content" source="../media/latency-plugin.png" alt-text="Dev Proxy simulating latency for an API request." lightbox="../media/latency-plugin.png":::

## Plugin instance definition

```json
{
  "name": "LatencyPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
  "configSection": "latencyPlugin"
}
```

## Configuration example

```json
{
  "latencyPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.27.0/latencyplugin.schema.json",
    "minMs": 200,
    "maxMs": 10000
  }
}
```

## Configuration properties

| Property | Description | Default |
| -------- | ----------- | :-----: |
| `minMs` | The minimum amount of delay added to a request in milliseconds. |   0 |
| `maxMs` | The maximum amount of delay added to a request in milliseconds. Max value is 10000 (10 s) |  5000  |

## Command line options

None

## Next step

> [!div class="nextstepaction"]
> [Simulate slow API responses](../how-to/simulate-slow-api-responses.md)
