---
title: Change request failure rate
description: How to configure the request failure rate
author: garrytrinder
ms.author: garrytrinder
ms.date: 11/03/2023
ms.topic: how-to
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
  - tool=devproxy
---

# Change request failure rate

By default, Dev Proxy randomly responds with [supported error codes](../technical-reference/Supported-HTTP-error-status-codes.md) with a 50% chance to intercept a request.

You can change the likelihood to a higher or lower value by changing the `failure-rate` setting, for example to increase the likelihood to 80%, start proxy with:

```sh
devproxy --failure-rate 80
```
