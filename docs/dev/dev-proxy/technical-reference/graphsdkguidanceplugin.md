---
title: GraphSdkGuidancePlugin
description: GraphSdkGuidancePlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/06/2026
---

<!-- INTENT: Suggest using Microsoft Graph SDK -->
<!-- PLUGIN-TYPE: Intercepting -->
<!-- WORKS-WITH: GraphSelectGuidancePlugin, GraphClientRequestIdGuidancePlugin, GraphBetaSupportGuidancePlugin -->
<!-- USE-WHEN: Encouraging SDK adoption for better Graph API usage -->

# GraphSdkGuidancePlugin

Shows a tip when proxy intercepts a request to Microsoft Graph that hasn't been issued by a Microsoft Graph SDK.

:::image type="content" source="../media/microsoft-graph-sdk-guidance.png" alt-text="Screenshot of a command prompt with Dev Proxy showing a tip suggesting using the Microsoft Graph SDK after intercepting a request to Microsoft Graph that doesn't use an SDK." lightbox="../media/microsoft-graph-sdk-guidance.png":::

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "GraphSdkGuidancePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ],
  "urlsToWatch": [
    "https://graph.microsoft.com/v1.0/*",
    "https://graph.microsoft.com/beta/*",
    "https://graph.microsoft.us/v1.0/*",
    "https://graph.microsoft.us/beta/*",
    "https://dod-graph.microsoft.us/v1.0/*",
    "https://dod-graph.microsoft.us/beta/*",
    "https://microsoftgraph.chinacloudapi.cn/v1.0/*",
    "https://microsoftgraph.chinacloudapi.cn/beta/*"
  ]
}
```

## Configuration properties

None

## Command line options

None

## Next step

> [!div class="nextstepaction"]
> [Update my application code to use Microsoft Graph JavaScript SDK](../how-to/update-my-application-code-to-use-microsoft-graph-javascript-sdk.md)
