---
title: Simulate slow API responses
description: How to simulate slow API responses
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/06/2026
---

<!-- INTENT: Add artificial latency to API responses for testing -->
<!-- SOLUTION: Enable LatencyPlugin with min/max delay settings -->
<!-- RESULT: API responses delayed by configured time -->
<!-- PLUGINS: LatencyPlugin -->
<!-- JOB: test-error-handling -->
<!-- TIME: 5 minutes -->

# Simulate slow API responses

> **At a glance**  
> **Goal:** Add artificial latency to API responses for testing  
> **Time:** 5 minutes  
> **Plugins:** [LatencyPlugin](../technical-reference/latencyplugin.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

Dev Proxy allows you to simulate slow API responses by using the [LatencyPlugin](../technical-reference/LatencyPlugin.md).

Start, by enabling the plugin in your Dev Proxy configuration file:

**File:** devproxyrc.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
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

**File:** devproxyrc.json

```json
"latencyPlugin": {
  "minMs": 200,
  "maxMs": 10000
}
```

The complete configuration file looks like this.

**File:** devproxyrc.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "LatencyPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "latencyPlugin"
    }
  ],
  "urlsToWatch": [
    "https://api.example.com/*"
  ],
  "latencyPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/latencyplugin.schema.json",
    "minMs": 200,
    "maxMs": 10000
  }
}
```

When a response is delayed, Dev Proxy displays the total duration it was delayed for in the console output.

## See also

- [LatencyPlugin](../technical-reference/latencyplugin.md) - Full reference
- [Test my app with random errors](./test-my-app-with-random-errors.md) - Simulate API failures
- [Glossary](../concepts/glossary.md) - Dev Proxy terminology
