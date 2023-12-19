---
title: How to handle rate limiting
description: This article explains how developers can handle rate limiting in their applications.
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

# How to handle rate limiting

[Rate limiting](what-is-rate-limiting.md) is a common technique used by API providers to manage the number of requests that can be made to their service in a given time period. API providers use rate limiting to ensure that their service remains available and responsive to all users, and to prevent abuse or overuse of the service.

When you use cloud APIs in your application, you should understand their rate limits. Following techniques can help you handle rate limiting in your applications:

- **Understand rate limits.** Check the documentation of the API that you use to understand its rate limits. Rate limits might depend on the API provider or the service plan that you use. For example, some APIs might have different rate limits for free and paid plans.
- **Use rate limiting information.** APIs that use rate limits, typically communicate the current rate limits in the response headers. For example, the `RateLimit-Remaining` header indicates the number of requests remaining in the current window. If you receive a response with this header set to 0, you know that you reached the rate limit and should wait for the next window before sending another request. The `RateLimit-Reset` header indicates the time when the rate limit resets. Remember, that some APIs only send the `RateLimit-...` headers after you reached a threshold, for example, when you have 10% of the requests remaining.
- **Optimize API usage.** Some services assign different cost to different requests based on their complexity. For example, some APIs might charge more for requests that return more data. To reduce the cost of your application, optimize your API usage by fetching only the data that you need, and using batch requests if supported by the API. It helps you reduce the number of resources required to process the response and stay within the rate limits.
- **Implement a local rate limiter.** Implement a rate limiter within the application itself, to limit the number of requests that can be made to the API in a given time period. You can do it using techniques such as token bucket or leaky bucket algorithms, which allow the application to make a number of requests per time period. Any more requests are queued or discarded.
- **Avoid exceeding rate limits.** When you exceed rate limits, the API [throttles](what-is-throttling.md) all subsequent requests typically returning an HTTP 429 (Too Many Requests) status code. Typically, throttling affects throughput of your application more than rate limiting. Use the information exposed in rate limit response headers, to stay within the rate limits and avoid throttling.

You can build applications that are resilient to rate limiting and can continue to function even when the API is under heavy load using these techniques.

> [!div class="nextstepaction"]
> [Test that my application handles rate limiting properly](../how-to/simulate-rate-limit-api-responses.md)
