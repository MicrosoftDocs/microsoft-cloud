---
title: LatencyPlugin
description: LatencyPlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 11/03/2023
ms.topic: reference
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

# LatencyPlugin

Delays responses by a random number of milliseconds from the configured range.

:::image type="content" source="../media/latency-plugin.png" alt-text="Dev Proxy simulating latency for an API request." lightbox="../media/latency-plugin.png":::

## Plugin instance definition

```json
{
  "name": "LatencyPlugin",
  "enabled": true,
  "pluginPath": "plugins\\dev-proxy-plugins.dll",
  "configSection": "latencyPlugin"
}
```

## Configuration example

```json
{
  "latencyPlugin": {
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
