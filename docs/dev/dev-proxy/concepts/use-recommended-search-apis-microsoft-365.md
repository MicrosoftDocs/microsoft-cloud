---
title: Use the recommended search APIs on Microsoft 365
description: How to use the recommended search APIs on Microsoft 365
author: waldekmastykarz
ms.author: wmastyka
ms.date: 12/12/2023
ms.topic: conceptual
ms.service: microsoft-cloud-for-developers

categories:
  - developer-tools
products:
  - microsoft-365
  - microsoft-graph
  - sharepoint-online
  - m365
ms.custom:
  - fcp
  - team=cloud_advocates
---

# Use the recommended search APIs on Microsoft 365

In the past, if you wanted to search in list and files, you used the OneDrive and SharePoint search APIs. In October '23, Microsoft released [new guidance on how to search in Microsoft 365](https://devblogs.microsoft.com/microsoft365dev/transition-to-microsoft-graph-search-endpoint-for-onedrive-and-sharepoint/). This guidance recommends using the Microsoft Graph search API instead of the OneDrive and SharePoint search APIs. While the OneDrive and SharePoint search APIs are still available, Microsoft will no longer evolve them.

To use the recommended search APIs, you need to update your code to use the Microsoft Graph search API. Start, with identifying if your application uses the OneDrive and SharePoint search APIs. Look for the following requests:

- `graph.microsoft.com/{version}/drives/{drive-id}/root/search(q='{search-text}')`
- `graph.microsoft.com/{version}/groups/{group-id}/drive/root/search(q='{search-text}')`
- `graph.microsoft.com/{version}/me/drive/root/search(q='{search-text}')`
- `graph.microsoft.com/{version}/sites/{site-id}/drive/root/search(q='{search-text}')`
- `graph.microsoft.com/{version}/users/{user-id}/drive/root/search(q='{search-text}')`
- `graph.microsoft.com/{version}/sites?search={query}`

> [!TIP]
> Use the Dev Proxy [ODSPSearchGuidancePlugin](../technical-reference/odspsearchguidanceplugin.md) to help you identify if your application uses the OneDrive and SharePoint search APIs.

Update each of these requests to use the Microsoft Search API instead. For examples of how to translate the different requests, see the [new search guidance](https://devblogs.microsoft.com/microsoft365dev/transition-to-microsoft-graph-search-endpoint-for-onedrive-and-sharepoint/).

> [!div class="nextstepaction"]
> [Use the ODSPSearchGuidancePlugin](../technical-reference/odspsearchguidanceplugin.md)
