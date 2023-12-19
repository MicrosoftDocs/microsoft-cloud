---
title: MockGeneratorPlugin
description: MockGeneratorPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 12/18/2023
---

# MockGeneratorPlugin

Generates Dev Proxy mocks based on the intercepted requests. Writes generated mocks to a file named `mocks-yyyyMMddHHmmss.json` in the current working folder.

:::image type="content" source="../media/mock-generator-plugin.png" alt-text="Screenshot of a terminal with Dev Proxy generating mocks for the intercepted requests." lightbox="../media/mock-generator-plugin.png":::

To intercept requests, after starting Dev Proxy, start [recording](../how-to/record-and-export-proxy-activity.md). When you're done, stop recording. Dev Proxy will generate mocks for the intercepted requests and write them to a file named `mocks-yyyyMMddHHmmss.json` in the current working folder.

## Plugin instance definition

```json
{
  "name": "MockGeneratorPlugin",
  "enabled": true,
  "pluginPath": "plugins\\dev-proxy-plugins.dll"
}
```

## Configuration example

None

## Configuration properties

None

## Command line options

None
