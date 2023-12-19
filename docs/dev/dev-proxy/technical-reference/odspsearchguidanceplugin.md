---
title: ODSPSearchGuidancePlugin
description: ODSPSearchGuidancePlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 12/18/2023
---

# ODSPSearchGuidancePlugin

Shows a warning when Dev Proxy detects a request to OneDrive and SharePoint search APIs.

This plugin follows the [Microsoft Search APIs guidance](https://devblogs.microsoft.com/microsoft365dev/transition-to-microsoft-graph-search-endpoint-for-onedrive-and-sharepoint/).

:::image type="content" source="../media/guidance-odsp-search.png" alt-text="Screenshot of Dev Proxy warning of using OneDrive and SharePoint search APIs.":::

This plugin detects the following requests:

- `graph.microsoft.com/{version}/drives/{drive-id}/root/search(q='{search-text}')`
- `graph.microsoft.com/{version}/groups/{group-id}/drive/root/search(q='{search-text}')`
- `graph.microsoft.com/{version}/me/drive/root/search(q='{search-text}')`
- `graph.microsoft.com/{version}/sites/{site-id}/drive/root/search(q='{search-text}')`
- `graph.microsoft.com/{version}/users/{user-id}/drive/root/search(q='{search-text}')`
- `graph.microsoft.com/{version}/sites?search={query}`

## Plugin instance definition

```json
{
  "name": "ODSPSearchGuidancePlugin",
  "enabled": true,
  "pluginPath": "plugins\\dev-proxy-plugins.dll",
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
