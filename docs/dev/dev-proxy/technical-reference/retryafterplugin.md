---
title: RetryAfterPlugin
description: RetryAfterPlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/06/2026
---

<!-- INTENT: Simulate Retry-After header after throttling -->
<!-- PLUGIN-TYPE: Intercepting -->
<!-- WORKS-WITH: GraphRandomErrorPlugin, GenericRandomErrorPlugin, RateLimitingPlugin -->
<!-- USE-WHEN: Testing retry logic compliance with Retry-After headers -->

# RetryAfterPlugin

Simulates the `Retry-After` header sent by an API after throttling a request. This plugin also shows a warning when proxy detects a subsequent request to the same URL before the `Retry-After` time elapses.

:::image type="content" source="../media/retry-after-plugin.png" alt-text="Screenshot of a command prompt with Dev Proxy forcefully failing a request that's been issued before the retry-after period." lightbox="../media/retry-after-plugin.png":::

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "RetryAfterPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ]
}
```

## Configuration properties

None

## Command line options

None

## Next step

> [!div class="nextstepaction"]
> [Simulate throttling on Microsoft 365 APIs](../how-to/simulate-throttling-microsoft-365.md)
