---
title: Why is my internet connection not working after using the proxy
description: Why is my internet connection not working after using the proxy
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

# Why is my internet connection not working after using the proxy

If you terminate the Microsoft 365 Developer Proxy process, the proxy doesn't unregister itself and your network connection on your machine is blocked.

To restore network connection, start the proxy and close it by pressing <kbd>Ctrl</kbd> + <kbd>C</kbd>. Closing proxy this way gracefully closes the proxy unregistering it on your machine and restoring the regular network connection.
