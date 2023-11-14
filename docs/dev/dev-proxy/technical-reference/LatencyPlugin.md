---
title: LatencyPlugin
description: LatencyPlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.contributors: garrytrinder
ms.date: 11/03/2023
ms.topic: conceptual
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
---

# LatencyPlugin

Introduces latency into requests.

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
"latencyPlugin": {
  "minMs": 200,
  "maxMs": 10000
},
```

## Configuration properties

| Property | Description | Default |
| -------- | ----------- | :-----: |
| `minMs` | The minimum amount of delay added to a request in milliseconds. |   0 |
| `maxMs` | The maximum amount of delay added to a request in milliseconds. Max value is 10000 (10 s) |  5000  |

## Command line options

None
