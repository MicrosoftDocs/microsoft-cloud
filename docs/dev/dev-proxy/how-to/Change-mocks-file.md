---
title: Change mocks file
description: How to specify which mocks file to use
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

# Change mocks file

By default, the proxy looks for a file called `responses.json` in the directory that the proxy is started from.

To tell proxy to use a file with a different name, use:

```sh
devproxy --mocks-file mock.json
```
