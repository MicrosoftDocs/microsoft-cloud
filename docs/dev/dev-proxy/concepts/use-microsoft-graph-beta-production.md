---
title: Can I use Microsoft Graph beta endpoints in production?
description: This article explains the trade-offs that you should consider when using Microsoft Graph beta endpoints in production.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 12/18/2023
ms.topic: concept-article
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

# Can I use Microsoft Graph beta endpoints in production?

Microsoft Graph beta endpoint is a preview version of the API that allows you to test and experiment with new features before they're released to the general public. Using the Microsoft Graph beta endpoint is a great way to stay ahead of the curve and take advantage of the latest innovations. It's however important to understand that the beta endpoint isn't intended for use in production environments.

One of the main risks of using beta Graph endpoints in production apps is that the APIs and functionalities available in the beta endpoint are subject to change. As a result, features and APIs that you rely on in your app might be modified or removed without notice, potentially causing disruptions or breaking changes to your app.

In addition, the beta endpoint might not have the same level of support, reliability, or performance as the v1.0 endpoint. It can unexpectedly change or be unavailable, have slower response times, or other issues that can affect the user experience of your app.

For these reasons, we strongly recommend that developers use the v1.0 endpoint when building production apps. The v1.0 endpoint provides a stable and reliable API that Microsoft supports. By using the v1.0 endpoint, you can ensure that your app is built on a solid foundation and that your users have the best possible experience.

Use Dev Proxy to see if your application uses the Microsoft Graph beta endpoint.

> [!div class="nextstepaction"]
> [Check if my app uses Graph beta endpoints](../technical-reference/graphbetasupportguidanceplugin.md)
