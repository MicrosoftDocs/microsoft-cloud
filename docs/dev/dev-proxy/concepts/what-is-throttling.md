---
title: What is throttling?
description: The concept of throttling in Microsoft 365 and other cloud services
author: garrytrinder
ms.author: garrytrinder
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
  - tool=devproxy
---

# What is throttling?

When your application connected to Microsoft Graph is used at scale, it can happen that it's throttled.

Throttling is a mechanism used in Microsoft 365 and other cloud services, to ensure service continuity.

When servers experience anomalous load, they start sending `429 Too many requests` responses, prompting applications to wait before issuing new requests. This behavior gives servers the chance to decrease the load and restore normal operations.

**The best way to help servers restore normal operations, is for applications to stop calling Microsoft Graph for the period specified on the 429 response. If however, applications will keep calling Microsoft Graph, throttling will continue blocking access to data.**

> [!TIP]
> If you use Microsoft Graph SDKs, your applications automatically back-off when throttled. If you don't use Microsoft Graph SDKs, you need to handle throttling, and other exceptions, yourself.
