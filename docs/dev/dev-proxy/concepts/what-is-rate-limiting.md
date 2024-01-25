---
title: What is rate limiting?
description: The concept of rate limiting in cloud APIs
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

# What is rate limiting?

Rate limiting is a control mechanism that cloud APIs use to regulate the number of requests a user can make in a given time frame. Cloud API producers use rate limiting to ensure that the flow of requests doesn't overwhelm the service. Rate limiting sets a cap on the speed and volume of API calls, typically defined in terms of requests per period of time.

## Why cloud APIs use rate limiting

- **Prevent overload.** Rate limiting ensures that the API server remains stable and responsive by preventing any single user or service from flooding it with too many requests.
- **Ensure fair usage.** Rate limiting enforces fair usage policies, ensuring that no single user monopolizes the API resources, allowing equitable access to all users.
- **Security.** It helps in mitigating DDoS (Distributed Denial of Service) attacks and other abusive behaviors by restricting the number of requests from potentially malicious sources.
- **Cost Management.** For cloud service providers, rate limiting helps in managing operational costs by preventing unpredictable or excessive use of resources.
- **Quality of Service.** By preventing traffic spikes, rate limiting ensures a consistent quality of service for all users.

## How you experience rate limiting in your apps

When you build apps that integrate cloud APIs, check their documentation to verify if they support rate limiting. If they do, you receive `RateLimit-...` or `X-RateLimit-...` response headers with information about the rate limits. You can use this information in your application to ensure that you don't exceed the API's rate limits. For example, the `RateLimit-Remaining` header indicates the number of requests remaining in the current window. If you receive a response with this header set to 0, you know that you've reached the rate limit and should wait for the next window before sending another request. The `RateLimit-Reset` header indicates the time when the rate limit resets. Remember, that some APIs only send the `RateLimit-...` headers after you reached a threshold, for example, when you have 10% of the requests remaining.

When you exceed the rate limit, the API throttles your requests returning an HTTP 429 (Too Many Requests) status code. Some APIs might also send a `Retry-After` header indicating how long you should wait before sending another request.

To avoid throttling and ensure that your application remains responsive, you should implement rate limiting in your application. Depending on your technology stack, there are different libraries that help you handle rate limiting in your application. After you implement rate limiting in your application, test if it handles rate limiting properly.

## Next steps

> [!div class="nextstepaction"]
> [Test that my application handles rate limiting properly](../how-to/simulate-rate-limit-api-responses.md)
