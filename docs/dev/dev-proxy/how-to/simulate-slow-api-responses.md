---
title: Simulate slow API responses
description: How to simulate slow API responses
author: garrytrinder
ms.author: garrytrinder
ms.date: 02/05/2025
---

# Simulate slow API responses

Dev Proxy allows you to simulate slow API responses by using the [LatencyPlugin](../technical-reference/LatencyPlugin.md).

Start, by enabling the plugin in your Dev Proxy configuration file:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.0/rc.schema.json",
  "plugins": [
    {
      "name": "LatencyPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "latencyPlugin"
    }
  ],
  "urlsToWatch": []
}
```

Next, specify the minimum and maximum delay (in milliseconds) to simulate for your API.

```json
"latencyPlugin": {
  "minMs": 200,
  "maxMs": 10000
}
```

When a response is delayed, Dev Proxy displays the total duration it was delayed for in the console output.
