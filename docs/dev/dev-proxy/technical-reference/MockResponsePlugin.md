---
title: MockResponsePlugin
description: MockResponsePlugin reference
author: garrytrinder
ms.author: garrytrinder
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

# MockResponsePlugin

Simulates responses.

:::image type="content" source="../media/mock-response-plugin.png" alt-text="Screenshot of a terminal with Dev Proxy simulating response for a request to GitHub API" lightbox="../media/mock-response-plugin.png":::

## Plugin instance definition

```json
{
  "name": "MockResponsePlugin",
  "enabled": true,
  "pluginPath": "plugins\\dev-proxy-plugins.dll",
  "configSection": "mocksPlugin"
}
```

## Configuration example

```json
{
  "mocksPlugin": {
    "mocksFile": "responses.json"
  }
}
```

## Configuration properties

| Property              | Description                                                        |     Default      |
| --------------------- | ------------------------------------------------------------------ | :--------------: |
| `mocksFile`             | Path to the file containing mock responses                         | `responses.json` |
| `blockUnmockedRequests` | Return `502 Bad Gateway` response for requests that aren't mocked |     `false`      |

## Command line options

| Name             | Description                                | Default |
| ---------------- | ------------------------------------------ | :-----: |
| `-n, --no-mocks` | Disable loading mock requests              | `false` |
| `--mocks-file`   | Path to the file containing mock responses |    -    |
