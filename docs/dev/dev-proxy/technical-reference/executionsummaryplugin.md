---
title: ExecutionSummaryPlugin
description: ExecutionSummaryPlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 04/30/2025
---

# ExecutionSummaryPlugin

Creates a summary of the requests that pass through the proxy.

> [!TIP]
> Use a reporter plugin in your configuration to save the summary to a file.

:::image type="content" source="../media/execution-summary-1.png" alt-text="Screenshot of a command prompt with the Dev Proxy execution summary with information about intercepted requests." lightbox="../media/execution-summary-1.png":::

:::image type="content" source="../media/execution-summary-2.png" alt-text="Screenshot of a command prompt with the summary of the requests intercepted by Dev Proxy." lightbox="../media/execution-summary-2.png":::

## Plugin instance definition

```json
{
  "name": "ExecutionSummaryPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
  "configSection": "executionSummaryPlugin"
}
```

## Configuration example

```json
{
  "executionSummaryPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/executionsummaryplugin.schema.json",
    "groupBy": "url"
  }
}
```

## Configuration properties

| Property | Description | Default |
|----------|-------------|:-------:|
| `groupBy` | How proxy should group the information in the summary. Available options: `url`, `messageType` | `url` |

## Command line options

| Name | Description | Default |
|----------|-------------|:-------:|
| `--summary-group-by` | How proxy should group the information in the summary. Available options: `url`, `messageType` | `url` |

## Next step

> [!div class="nextstepaction"]
> [Record and export proxy activity](../how-to/record-and-export-proxy-activity.md)
