---
title: Monitor requests from specific processes
description: How to configure the proxy to monitor requests only from specific processes
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

# Monitor requests from specific processes

By default, Dev Proxy is registered as a system wide proxy and all requests made by your machine are passed through the proxy.

By default, the proxy monitors the requests that are made from your machine to the URLs configured in [devproxyrc.json](../technical-reference/devproxyrc.md) file.

However, you might also want to only monitor requests being made from specific processes such as a terminal window or web browser.

To monitor processes by their given process IDs, use the `--watch-pids` option:

```sh
devproxy –-watch-pids 870 135100
```

To monitor processes by their given process names, use the `--watch-process-names` option:

```sh
devproxy --watch-process-names msedge pwsh
```
