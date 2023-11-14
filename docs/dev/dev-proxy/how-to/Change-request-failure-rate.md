---
title: Change request failure rate
description: How to configure the request failure rate
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

# Change request failure rate

By default, Dev Proxy randomly responds with [supported error codes](../technical-reference/Supported-HTTP-error-status-codes.md) with a 50% chance that the proxy will intercept a request to Microsoft Graph.

You can increase or decrease the likelihood to a higher value or lower value by changing the `failure-rate` setting value.

```sh
devproxy --failure-rate 80
```
