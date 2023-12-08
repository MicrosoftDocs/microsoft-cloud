---
title: Why do all requests fail with gateway timeout
description: How to fix all requests failing with gateway timeout
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

# Why do all requests fail with gateway timeout

If you're running the proxy for the first time, it can happen that the network access configuration didn't propagate timely and the proxy started without access to your network.

Close the proxy by pressing <kbd>Ctrl</kdb> + <kbd>C</kdb> in proxy's window and restart the proxy and it should be working as intended.
