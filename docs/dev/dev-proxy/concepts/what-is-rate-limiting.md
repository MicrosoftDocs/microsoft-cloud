---
title: What is rate limiting?
description: This article explains the concept of rate limiting in cloud APIs.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/26/2024
---

# What is rate limiting?

Rate limiting is a control mechanism that cloud APIs use to regulate the number of requests that a user can make in a specific time. Cloud API producers use rate limiting to ensure that the flow of requests doesn't overwhelm the service. Rate limiting sets a cap on the speed and volume of API calls. Rate limits are typically defined in terms of requests per period of time.

## Why cloud APIs use rate limiting

- **Prevent overload.** Rate limiting ensures that the API server remains stable and responsive by preventing any single user or service from flooding it with too many requests.
- **Ensure fair usage.** Rate limiting enforces fair usage policies by ensuring that no single user monopolizes the API resources. Rate limiting allows equitable access to all users.
- **Increase security.** Rate limiting helps in mitigating Distributed Denial of Service attacks and other abusive behaviors by restricting the number of requests from potentially malicious sources.
- **Manage costs.** For cloud service providers, rate limiting helps in managing operational costs by preventing unpredictable or excessive use of resources.
- **Maintain quality of service.** Rate limiting ensures a consistent quality of service for all users by preventing traffic spikes.

## How you experience rate limiting in your apps

When you build apps that integrate cloud APIs, check their documentation to verify if they support rate limiting. If they do, you receive `RateLimit-...` or `X-RateLimit-...` response headers with information about the rate limits. You can use this information in your application to ensure that you don't exceed the API's rate limits. For example, the `RateLimit-Remaining` header indicates the number of requests remaining in the current window. If you receive a response with this header set to 0, you know that you reached the rate limit and should wait for the next window before you send another request. The `RateLimit-Reset` header indicates the time when the rate limit resets. Some APIs send the `RateLimit-...` headers only after you reach a threshold. An example is when you have 10% of the requests remaining.

When you exceed the rate limit, the API throttles your requests and returns an HTTP `429 Too Many Requests` status code. Some APIs might also send a `Retry-After` header to indicate how long you should wait before you send another request.

To avoid throttling and ensure that your application remains responsive, implement rate limiting in your application. Depending on your technology stack, different libraries can help you handle rate limiting in your application. After you implement rate limiting in your application, test to see if it handles rate limiting properly.

## Next step

> [!div class="nextstepaction"]
> [Test that my application handles rate limiting properly](../how-to/simulate-rate-limit-api-responses.md)
