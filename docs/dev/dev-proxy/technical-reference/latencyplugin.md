---
title: LatencyPlugin
description: LatencyPlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/13/2026
---

<!-- INTENT: Add artificial latency to API responses -->
<!-- PLUGIN-TYPE: Intercepting, STDIO -->
<!-- WORKS-WITH: GenericRandomErrorPlugin, GraphRandomErrorPlugin, RateLimitingPlugin, STDIO command -->
<!-- USE-WHEN: Testing slow network conditions or UI timeout handling -->

# LatencyPlugin

Delays responses by a random number of milliseconds from the configured range. Supports both HTTP requests and STDIO communication.

:::image type="content" source="../media/latency-plugin.png" alt-text="Dev Proxy simulating latency for an API request." lightbox="../media/latency-plugin.png":::

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
  "plugins": [
    {
      "name": "LatencyPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "latencyPlugin"
    }
  ],
  "latencyPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/latencyplugin.schema.json",
    "minMs": 200,
    "maxMs": 10000
  }
}
```

## Configuration properties

| Property | Description | Default |
| -------- | ----------- | :-----: |
| `minMs` | The minimum amount of delay added to a request in milliseconds. | 0 |
| `maxMs` | The maximum amount of delay added to a request in milliseconds. | 5000 |

## Command line options

None

## STDIO support

When you use the `LatencyPlugin` with the [`STDIO` command](stdio.md), the plugin adds artificial latency to stdout responses. Simulating latency is useful for testing how your application handles slow Model Context Protocol (MCP) server responses or other STDIO-based tools.

### Configuration example for STDIO

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
  "plugins": [
    {
      "name": "LatencyPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "latencyPlugin"
    },
    {
      "name": "MockSTDIOResponsePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "mockSTDIOResponsePlugin"
    }
  ],
  "latencyPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/latencyplugin.schema.json",
    "minMs": 100,
    "maxMs": 500
  },
  "mockSTDIOResponsePlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/mockSTDIOresponseplugin.schema.json",
    "mocksFile": "STDIO-mocks.json"
  }
}
```

Then run:

```console
devproxy STDIO npx -y @modelcontextprotocol/server-filesystem
```

## Next step

> [!div class="nextstepaction"]
> [Simulate slow API responses](../how-to/simulate-slow-api-responses.md)
