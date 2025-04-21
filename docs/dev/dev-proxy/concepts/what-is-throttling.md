---
title: What is throttling?
description: This article explains the concept of throttling in cloud APIs.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 12/18/2023
---

# What is throttling?

Throttling is a technique that cloud APIs use to limit the number of requests that can be made in a specific period of time. Throttling ensures that the API remains available and responsive to all users. It also prevents any single user from consuming too many resources.

You can experience throttling in several ways. One common way is by using HTTP status codes. For example, when a user exceeds the allowed number of requests, the API might return a `429 Too Many Requests` status code. This response indicates that the user issued too many requests in a specific period of time and should slow down.

In addition to status codes, some APIs might also provide more information in the response headers or body. For example, they might use the `Retry-After` header to indicate how long the user should wait before making another request.

You need to be aware of the throttling limits of the APIs that you use, and know how to handle throttling errors appropriately in your apps. Throttling helps you ensure that your apps remain responsive and reliable, even when the API is under heavy load.

## Next step

> [!div class="nextstepaction"]
> [Test that my application handles throttling properly](../how-to/test-that-my-application-handles-throttling-properly.md)
