---
title: Test that my application handles HTTP errors properly
description: How to test that your application handles HTTP errors properly
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

# Test that my application handles HTTP errors properly

When applications use Microsoft Graph and other cloud services, it can happen, that these APIs are temporarily unavailable.

It's important, that applications consider such scenario and handle exceptions properly.

Testing exceptions in APIs you don't manage is hard, because it's hard to make the API return a specific response.

Using Dev Proxy, you can mimic erroneous responses from Microsoft Graph and test your application to see that it handles these errors properly.

To test your application, start the Dev Proxy:

```sh
devproxy --no-mocks
```
