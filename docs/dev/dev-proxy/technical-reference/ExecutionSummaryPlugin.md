---
title: ExecutionSummaryPlugin
description: ExecutionSummaryPlugin reference
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

# ExecutionSummaryPlugin

Generates a summary report of the requests that pass through the proxy.

:::image type="content" source="../media/execution-summary-1.png" alt-text="Screenshot of a terminal with the Dev Proxy execution summary with information about intercepted requests." lightbox="../media/execution-summary-1.png":::

:::image type="content" source="../media/execution-summary-2.png" alt-text="Screenshot of a terminal with the summary of the requests intercepted by Dev Proxy." lightbox="../media/execution-summary-2.png":::

## Plugin instance definition

```json
{
  "name": "ExecutionSummaryPlugin",
  "enabled": false,
  "pluginPath": "plugins\\dev-proxy-plugins.dll",
  "configSection": "executionSummaryPlugin"
}
```

## Configuration example

```json
{
  "executionSummaryPlugin": {
    "groupBy": "url"
  }
}
```

## Configuration properties

| Property | Description | Default |
|----------|-------------|:-------:|
| `filePath` | Path to the file where the summary should be saved. If not specified, the summary is printed to the console. Path can be absolute or relative to the current working directory. | No default |
| `groupBy` | How proxy should group the information in the summary. Available options: `url`, `messageType` | `url` |

## Command line options

| Name | Description | Default |
|----------|-------------|:-------:|
| `--summary-file-path` | Path to the file where the summary should be saved. If not specified, the summary is printed to the console. Path can be absolute or relative to the current working directory. | No default |
| `--summary-group-by` | How proxy should group the information in the summary. Available options: `url`, `messageType` | `url` |
