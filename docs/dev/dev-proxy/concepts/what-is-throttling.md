---
title: What is throttling?
description: The concept of throttling in cloud APIs
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

# What is throttling?

Throttling is a technique that cloud APIs use to limit the number of requests that can be made in a given period of time. They use throttling to ensure that the API remains available and responsive to all users, and to prevent any single user from consuming too many resources.

You can experience throttling in several ways. One common way is by using HTTP status codes. For example, when a user exceeds the allowed number of requests, the API might return a `429 Too Many Requests` status code. This response indicates that the user issued too many requests in a given period of time and should slow down.

In addition to status codes, some APIs might also provide additional information in the response headers or body. For example, they might use the `Retry-After` header to indicate how long the user should wait before making another request.

It's important that as a developer you're aware of the throttling limits of the APIs you use and handle throttling errors appropriately in your apps. It helps you ensure that your apps remain responsive and reliable, even when the API is under heavy load.

> [!div class="nextstepaction"]
> [Test that my application handles throttling properly](../how-to/test-that-my-application-handles-throttling-properly.md)
