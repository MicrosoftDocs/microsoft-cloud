---
title: PlainTextReporter
description: PlainTextReporter reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/06/2026
---

<!-- INTENT: Convert report data to plain text format -->
<!-- PLUGIN-TYPE: Reporter -->
<!-- WORKS-WITH: ExecutionSummaryPlugin, UrlDiscoveryPlugin, GraphMinimalPermissionsPlugin -->
<!-- USE-WHEN: Creating simple text reports for logging or quick review -->

# PlainTextReporter

Generates reports in plain-text format.

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "PlainTextReporter",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ]
}
```

## Configuration properties

None

## Remarks

PlainTextReporter stores reports in files named `PluginName_PlainTextReporter.txt`.
