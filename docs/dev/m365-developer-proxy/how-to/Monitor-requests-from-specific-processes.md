---
title: Monitor requests from specific processes
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

By default, the developer proxy is registered as a system wide proxy and all requests made by your machine are passed through the proxy. 

By default, the proxy monitors the requests that are made from your machine to the URLs configured in [m365proxyrc.json](./m365proxyrc) file.

However, you might also want to only monitor requests being made from specific processes such as a terminal window or web browser.

To monitor processes by their given process IDs, use the `--watch-pids` option:

```cmd
m365proxy –-watch-pids 870 135100
```

To monitor processes by their given process names, use the `--watch-process-names` option:

```cmd
m365proxy --watch-process-names msedge pwsh
```
