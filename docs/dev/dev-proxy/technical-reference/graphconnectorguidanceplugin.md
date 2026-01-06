---
title: GraphConnectorGuidancePlugin
description: GraphConnectorGuidancePlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/06/2026
---

<!-- INTENT: Provide guidance for Microsoft Graph connector development -->
<!-- PLUGIN-TYPE: Intercepting -->
<!-- WORKS-WITH: GraphMockResponsePlugin -->
<!-- USE-WHEN: Building Microsoft Graph connectors for Copilot -->

# GraphConnectorGuidancePlugin

Shows contextual guidance for working with Microsoft Graph connectors.

:::image type="content" source="../media/graph-connector-guidance-plugin.png" alt-text="Screenshot of Dev Proxy warning of submitting an external connection schema without the required semantic labels." lightbox="../media/graph-connector-guidance-plugin.png":::

This plugin detects the following issues:

Issue | Request | Level
--- | --- | ---
Schema missing one or more semantic labels required for Microsoft Copilot for Microsoft 365 | `PATCH https://graph.microsoft.com/v1.0/external/connections/{id}/schema` | Failure

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "GraphConnectorGuidancePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ],
  "urlsToWatch": [
    "https://graph.microsoft.com/*/external/connections/*/schema",
    "https://graph.microsoft.us/*/external/connections/*/schema",
    "https://dod-graph.microsoft.us/*/external/connections/*/schema",
    "https://microsoftgraph.chinacloudapi.cn/*/external/connections/*/schema"
  ]
}
```

## Configuration properties

None

## Command line options

None
