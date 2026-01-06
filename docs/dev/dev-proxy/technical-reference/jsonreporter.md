---
title: JsonReporter
description: JsonReporter reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/06/2026
---

<!-- INTENT: Convert report data to JSON format -->
<!-- PLUGIN-TYPE: Reporter -->
<!-- WORKS-WITH: ExecutionSummaryPlugin, GraphMinimalPermissionsPlugin, ApiCenterOnboardingPlugin -->
<!-- USE-WHEN: Creating machine-readable reports for CI/CD pipelines -->

# JsonReporter

Generates reports in JSON format.

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "JsonReporter",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ]
}
```

## Configuration properties

None

## Remarks

JsonReporter stores reports in files named `PluginName_JsonReporter.json`.
