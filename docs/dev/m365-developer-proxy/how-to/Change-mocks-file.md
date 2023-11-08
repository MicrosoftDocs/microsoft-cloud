---
title: Change mocks file
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

By default, the proxy looks for a file called `responses.json` in the directory that the proxy is started from.

To tell proxy to use a file with a different name, use:

```sh
m365proxy --mocks-file mock.json
```
