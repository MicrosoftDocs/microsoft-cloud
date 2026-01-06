---
title: Are Microsoft Graph Beta Endpoints Used in Production?
description: This article explains the trade-offs that you should consider when you use Microsoft Graph beta endpoints in production.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/26/2024
---

<!-- INTENT: Understand risks of using Microsoft Graph beta endpoints in production -->

# Can I use Microsoft Graph beta endpoints in production?

Microsoft Graph beta endpoint is a preview version of the API that you can use to test and experiment with new features before they're released to the general public. Using the Microsoft Graph beta endpoint is a great way to take advantage of the latest innovations. *The beta endpoint isn't intended for use in production environments.*

One of the main risks of using beta Microsoft Graph endpoints in production apps is that the APIs and functionalities available in the beta endpoint are subject to change. As a result, features and APIs that you rely on in your app might be modified or removed without notice. Their removal might cause disruptions or breaking changes to your app.

The beta endpoint also might not have the same level of support, reliability, or performance as the v1.0 endpoint. It can unexpectedly change or be unavailable, have slower response times, or cause other issues that can affect the user experience of your app.

For these reasons, we strongly recommend that developers use the v1.0 endpoint when they build production apps. The v1.0 endpoint provides a stable and reliable API that Microsoft supports. By using the v1.0 endpoint, you can ensure that your app is built on a solid foundation and that your users have the best possible experience.

Use Dev Proxy to see if your application uses the Microsoft Graph beta endpoint.

## Next step

> [!div class="nextstepaction"]
> [Check if my app uses Microsoft Graph beta endpoints](../technical-reference/graphbetasupportguidanceplugin.md)
