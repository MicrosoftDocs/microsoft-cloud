---
title: Handle rate limiting
description: This article explains how developers can handle rate limiting in their applications.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/26/2024
---

# Handle rate limiting

[Rate limiting](what-is-rate-limiting.md) is a common technique used by API providers to manage the number of requests that can be made to their service in a specific time period. API providers use rate limiting to ensure that their service remains available and responsive to all users, and to prevent abuse or overuse of the service.

When you use cloud APIs in your application, you should understand their rate limits. The following techniques can help you handle rate limiting in your applications:

- **Understand rate limits.** Check the documentation of the API that you use to understand its rate limits. Rate limits might depend on the API provider or the service plan that you use. For example, some APIs might have different rate limits for free and paid plans.
- **Use rate limiting information.** APIs that use rate limits typically communicate the current rate limits in the response headers. For example, the `RateLimit-Remaining` header indicates the number of requests that remain in the current window. If you receive a response with this header set to 0, you know that you reached the rate limit and should wait for the next window before you send another request. The `RateLimit-Reset` header indicates the time when the rate limit resets. Some APIs send the `RateLimit-...` headers only after you reach a threshold. An example is when you have 10% of the requests remaining.
- **Optimize API usage.** Some services assign different costs to different requests based on their complexity. For example, some APIs might charge more for requests that return more data. To reduce the cost of your application, optimize your API usage by fetching only the data that you need. Use batch requests if the API supports them. They help you reduce the number of resources required to process the response and stay within the rate limits.
- **Implement a local rate limiter.** Implement a rate limiter within the application itself to limit the number of requests that can be made to the API in a specific time period. You can do it by using techniques such as token bucket or leaky bucket algorithms, which allow the application to make many requests per time period. Any more requests are queued or discarded.
- **Avoid exceeding rate limits.** When you exceed rate limits, the API [throttles](what-is-throttling.md) all subsequent requests that typically return an HTTP `429 Too Many Requests` status code. Typically, throttling affects the throughput of your application more than rate limiting. Use the information exposed in rate limit response headers to stay within the rate limits and avoid throttling.

By using these techniques, you can build applications that are resilient to rate limiting and can continue to function even when the API is under heavy load.

## Next step

> [!div class="nextstepaction"]
> [Test that my application handles rate limiting properly](../how-to/simulate-rate-limit-api-responses.md)
