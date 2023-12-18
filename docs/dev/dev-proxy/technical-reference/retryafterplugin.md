---
title: RetryAfterPlugin
description: RetryAfterPlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 11/03/2023
---

# RetryAfterPlugin

Simulates the `Retry-After` header sent by an API after throttling a request. This plugin also shows a warning when proxy detects a subsequent request to the same URL before the `Retry-After` time elapses.

:::image type="content" source="../media/retry-after-plugin.png" alt-text="Screenshot of a terminal with Dev Proxy forcefully failing a request that's been issued before the retry-after period." lightbox="../media/retry-after-plugin.png":::

## Plugin instance definition

```json
{
  "name": "RetryAfterPlugin",
  "enabled": true,
  "pluginPath": "plugins\\dev-proxy-plugins.dll"
}
```

## Configuration example

None

## Configuration properties

None

## Command line options

None
