---
title: Console output ASCII icons
description: Get started with Microsoft 365 Developer Proxy
author: garrytrinder
ms.author: garrytrinder
ms.contributors: garrytrinder
ms.date: 11/03/2023
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

You can configure the proxy to use ASCII icons instead of text labels in the console output.

The proxy uses the following icons:

| Icon | Description |
| ----- | ------------ |
|`← ←`| Proxy intercepted the request |
|`↑ ↑`| Proxy ignored a request and passed it through |
|`× →`| Proxy simulated an API error |
|`/!\`| Proxy returned a performance warning |
|`o →`| Proxy mocked the response |
|`! →`| Request failed |
|`(i)`| Proxy provided developer guidance |

To use icons instead of labels, follow the [Change console output to use icons](./Change-console-output-to-use-icons.md) guide.
