---
title: Simulate slow API responses
description: How to simulate slow API responses
author: garrytrinder
ms.author: garrytrinder
ms.date: 11/03/2023
ms.topic: how-to
ms.service: microsoft-cloud-for-developers

categories:
  - developer-tools
products:
  - microsoft-365
  - microsoft-graph
  - sharepoint-online
  - m365
ms.custom:
  - fcp
  - team=cloud_advocates
  - tool=devproxy
---

# Simulate slow API responses

In v0.12, we introduced support for delaying requests to simulate slow responses from APIs.

> [!NOTE]
> By default the [LatencyPlugin](../technical-reference/LatencyPlugin.md) is disabled. To enable the plugin, open the [devproxyrc.json](../technical-reference/devproxyrc.md) file, search for the plugin object and change the `enabled` property to `true`. The plugin will be enabled the next time you start proxy.

The minimum and maximum delay (in milliseconds) which can be added to a response is defined in the configuration object.

```json
"latencyPlugin": {
  "minMs": 200,
  "maxMs": 10000
}
```

When a response is delayed, proxy displays the total duration it was delayed for in the console output.
