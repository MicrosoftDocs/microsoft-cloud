---
title: Why is proxy not intercepting requests from my .NET 4.8 app
description: How to configure the Microsoft 365 Developer Proxy for use with .NET 4.8 apps
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

# Why is proxy not intercepting requests from my .NET 4.8 app

To use `m365proxy` with .NET 4.8, you need to configure it as proxy with the `WinHTTP API`.

Use the `netsh` command to configure the proxy. (run as admin):

```cmd
netsh winhttp set proxy proxy-server="http=localhost:8000;https=localhost:8000"
```

When you're done, you can reset the WinHTTP settings by using:

```cmd
netsh winhttp reset proxy
```
