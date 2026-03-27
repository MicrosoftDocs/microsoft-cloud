---
title: MarkdownReporter
description: MarkdownReporter reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 03/26/2026
---

<!-- INTENT: Convert report data to Markdown format -->
<!-- PLUGIN-TYPE: Reporter -->
<!-- WORKS-WITH: ExecutionSummaryPlugin, GraphMinimalPermissionsPlugin, ApiCenterOnboardingPlugin -->
<!-- USE-WHEN: Creating human-readable reports for documentation -->

# MarkdownReporter

Generates reports in Markdown format.

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.3.0/rc.schema.json",
  "plugins": [
    {
      "name": "MarkdownReporter",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ]
}
```

## Configuration properties

None

## Remarks

MarkdownReporter stores reports in files named `PluginName_MarkdownReporter.md`.
