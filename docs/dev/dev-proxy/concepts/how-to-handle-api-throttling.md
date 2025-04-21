---
title: Handle API throttling
description: This article explains how developers can handle throttling in their applications.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/26/2024
---

# Handle API throttling

[API throttling](./what-is-throttling.md) is a common challenge that developers face when they build applications that rely on cloud APIs. Here are some common techniques that you can use to handle API throttling in your applications:

- **Use rate limiting.** If the API that you use supports rate limiting, use rate limiting information sent by the API in your application to ensure that your application doesn't exceed the API's rate limits.
- **Handle Retry-After headers.** Some APIs send a `Retry-After` header in their response when a request is throttled. If you get throttled, and the API sends a response with a `Retry-After` header, wait for the specified time before you send another request.
- **Implement exponential backoff.** If the API that you use doesn't send a `Retry-After` header, implement an exponential backoff algorithm. After each failed request, wait twice as long before you try again. Waiting longer helps you reduce the load on the API and increases the chances of your subsequent requests being successful.
- **Cache previously received data.** Cache responses from the API, especially for requests that are likely to return the same data. [Caching](./what-is-caching.md) helps you reduce the number of calls made to the API and stay within the rate limits.
- **Use queue requests.** Implement a queue for outgoing API requests to manage the request rate and ensure that the API's rate limits aren't exceeded.
- **Optimize API calls.** Optimize your API calls by fetching only the data that you need and using batch requests if supported by the API. Optimizing helps you reduce the number of resources required to process the response and stay within the rate limits.

By implementing these techniques, you can make your application more resilient to API throttling and ensure a smoother interaction with external services.

After you implement these techniques in your application, test if it handles throttling properly.

## Next step

> [!div class="nextstepaction"]
> [Test that my application handles throttling properly](../how-to/test-that-my-application-handles-throttling-properly.md)
