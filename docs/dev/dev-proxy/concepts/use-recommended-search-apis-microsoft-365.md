---
title: Use the Recommended Search APIs for Microsoft 365
description: This article explains how to use the recommended search APIs for Microsoft 365.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/26/2024
---

# Use the recommended search APIs for Microsoft 365

In the past, if you wanted to search lists and files, you used the OneDrive and SharePoint search APIs. In October 2023, Microsoft released [guidance on how to search in Microsoft 365](https://devblogs.microsoft.com/microsoft365dev/transition-to-microsoft-graph-search-endpoint-for-onedrive-and-sharepoint/). This guidance recommends that you use the Microsoft Graph search API instead of the OneDrive and SharePoint search APIs. The OneDrive and SharePoint search APIs are still available, but Microsoft no longer updates them.

To use the recommended search APIs, you need to update your code to use the Microsoft Graph search API. Start by identifying if your application uses the OneDrive and SharePoint search APIs. Look for the following requests:

- `graph.microsoft.com/{version}/drives/{drive-id}/root/search(q='{search-text}')`
- `graph.microsoft.com/{version}/groups/{group-id}/drive/root/search(q='{search-text}')`
- `graph.microsoft.com/{version}/me/drive/root/search(q='{search-text}')`
- `graph.microsoft.com/{version}/sites/{site-id}/drive/root/search(q='{search-text}')`
- `graph.microsoft.com/{version}/users/{user-id}/drive/root/search(q='{search-text}')`
- `graph.microsoft.com/{version}/sites?search={query}`

> [!TIP]
> Use the Dev Proxy [ODSPSearchGuidancePlugin](../technical-reference/odspsearchguidanceplugin.md) to identify if your application uses the OneDrive and SharePoint search APIs.

Update each of these requests to use the Microsoft Search API instead. For examples of how to translate the different requests, see the [search guidance](https://devblogs.microsoft.com/microsoft365dev/transition-to-microsoft-graph-search-endpoint-for-onedrive-and-sharepoint/).

## Next step

> [!div class="nextstepaction"]
> [Use the ODSPSearchGuidancePlugin](../technical-reference/odspsearchguidanceplugin.md)
