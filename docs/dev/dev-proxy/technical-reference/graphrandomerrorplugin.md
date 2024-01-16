---
title: GraphRandomErrorPlugin
description: GraphRandomErrorPlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/16/2024
---

# GraphRandomErrorPlugin

Fails requests made to Microsoft Graph with random errors.

:::image type="content" source="../media/microsoft-graph-random-error.png" alt-text="Screenshot of a terminal with Dev Proxy simulating a random error for a Microsoft Graph request." lightbox="../media/microsoft-graph-random-error.png":::

## Plugin instance definition

```json
{
  "name": "GraphRandomErrorPlugin",
  "enabled": false,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
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
| `allowedErrors` | List of [supported HTTP status codes](./Supported-HTTP-error-status-codes.md) that developer proxy might produce. |  |

## Command line options

| Name | Description | Default |
|----------|-------------|:-------:|
| `-a, --allowed-errors` | List of [supported HTTP status codes](./Supported-HTTP-error-status-codes.md) that developer proxy might produce. | `429 500 502 503 504 507` |
