---
title: Why is proxy not intercepting requests from my .NET 4.8 app
description: How to configure the Dev Proxy for use with .NET 4.8 apps
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

# Why is proxy not intercepting requests from my .NET 4.8 app

To use `devproxy` with .NET 4.8, you need to configure it as proxy with the `WinHTTP API`.

Use the `netsh` command to configure the proxy. (run as admin):

```sh
netsh winhttp set proxy proxy-server="http=localhost:8000;https=localhost:8000"
```

When you're done, you can reset the WinHTTP settings by using:

```sh
netsh winhttp reset proxy
```
