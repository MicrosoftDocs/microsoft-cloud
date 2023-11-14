---
title: GraphRandomErrorPlugin
description: GraphRandomErrorPlugin reference
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

# GraphRandomErrorPlugin

Fails requests made to Microsoft Graph with random errors.

## Plugin instance definition

```json
{
  "name": "GraphRandomErrorPlugin",
  "enabled": false,
  "pluginPath": "plugins\\dev-proxy-plugins.dll",
  "configSection": "graphRandomErrorsPlugin"
}
```

## Configuration example

```json
{
  "graphRandomErrorsPlugin": {
    "allowedErrors": [ 429, 500, 502, 503, 504, 507 ]
  }
}
```

## Configuration properties

| Property | Description | Default |
|----------|-------------|:-------:|
| AllowedErrors | List of [supported HTTP status codes](./Supported-HTTP-error-status-codes.md) that developer proxy might produce. |  |

## Command line options

| Name | Description | Default |
|----------|-------------|:-------:|
| `--allowed-errors` | List of [supported HTTP status codes](./Supported-HTTP-error-status-codes.md) that developer proxy might produce. |  |
