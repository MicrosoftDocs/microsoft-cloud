---
title: GraphSdkGuidancePlugin
description: GraphSdkGuidancePlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 04/30/2025
---

# GraphSdkGuidancePlugin

Shows a tip when proxy intercepts a request to Microsoft Graph that hasn't been issued by a Microsoft Graph SDK.

:::image type="content" source="../media/microsoft-graph-sdk-guidance.png" alt-text="Screenshot of a command prompt with Dev Proxy showing a tip suggesting using the Microsoft Graph SDK after intercepting a request to Microsoft Graph that doesn't use an SDK." lightbox="../media/microsoft-graph-sdk-guidance.png":::

## Plugin instance definition

```json
{
  "name": "GraphSdkGuidancePlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
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

## Configuration example

None

## Configuration properties

None

## Command line options

None

## Next step

> [!div class="nextstepaction"]
> [Update my application code to use Microsoft Graph JavaScript SDK](../how-to/update-my-application-code-to-use-microsoft-graph-javascript-sdk.md)
