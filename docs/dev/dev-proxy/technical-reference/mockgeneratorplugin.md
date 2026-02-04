---
title: MockGeneratorPlugin
description: MockGeneratorPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/06/2026
---

<!-- INTENT: Auto-generate mock files from intercepted requests -->
<!-- PLUGIN-TYPE: Reporting -->
<!-- WORKS-WITH: MockResponsePlugin, GraphMockResponsePlugin -->
<!-- USE-WHEN: Creating initial mocks from real API traffic -->

# MockGeneratorPlugin

Generates Dev Proxy mocks based on the intercepted requests. Writes generated mocks to a file named `mocks-yyyyMMddHHmmss.json` in the current working folder.

:::image type="content" source="../media/mock-generator-plugin.png" alt-text="Screenshot of a command prompt with Dev Proxy generating mocks for the intercepted requests." lightbox="../media/mock-generator-plugin.png":::

To intercept requests, after starting Dev Proxy, start [recording](../how-to/record-and-export-proxy-activity.md). When you're done, stop recording. Dev Proxy will generate mocks for the intercepted requests and write them to a file named `mocks-yyyyMMddHHmmss.json` in the current working folder.

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
  "plugins": [
    {
      "name": "MockGeneratorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ]
}
```

## Configuration properties

None

## Command line options

None
