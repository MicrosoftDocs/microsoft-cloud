---
title: How to implement rate limiting in Azure API Management
description: Learn how to implement rate limiting in Azure API Management
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/26/2024
---

# How to implement rate limiting in Azure API Management

Using [rate limiting](./what-is-rate-limiting.md) you can limit the number of API calls a user or service can make in a given time frame. Rate limiting helps you ensure fair usage and prevents any single user or service from monopolizing the API resources. Azure API Management (APIM) provides a convenient way to implement rate limiting for your APIs.

## Why Azure API Management?

[Azure API Management](/azure/api-management/api-management-key-concepts) is a powerful and versatile cloud service that helps organizations publish APIs to external, partner, and internal developers. It provides tools for securing, managing, and scaling API calls. One of its features is controlling rate limiting which is useful for maintaining the health and reliability of your APIs.

## Configure rate limiting in Azure API Management

Azure API Management uses policies to enforce rate limiting. You can define these policies at different scopes: global, product, or API-specific. This flexibility allows you to tailor rate limiting according to your API's requirements and usage patterns.

Before you start implementing rate limiting, decide on the rate limits. The limits you set depend on your API's capacity and the traffic you expect. Common limits are set as a number of calls per second, minute, or hour. For instance, you might allow 1000 calls per minute per user.

To define rate limits on your API in Azure API Management, use the [`rate-limit`](/azure/api-management/rate-limit-policy) or [`rate-limit-by-key`](/azure/api-management/rate-limit-by-key-policy) policies. The former sets a limit across all users, while the latter allows limits per identified key (like a subscription or a user ID).

Here's an example of a policy that limits the calls to 1000 per minute.

```xml
<policies>
  <inbound>
    <base />
    <rate-limit calls="1000" renewal-period="60" />
  </inbound>
  <backend>
    <base />
  </backend>
  <outbound>
    <base />
  </outbound>
  <on-error>
    <base />
  </on-error>
</policies>
```

When you exceed the specified number of calls, Azure API Management sends a 429 Too Many Requests status code, along with the `retry-after` response header and a message indicating when you can try again.

```text
HTTP/1.1 429 Too Many Requests
content-type: application/json
retry-after: 60
    
{
  "statusCode": 429,
  "message": "Rate limit is exceeded. Try again in 60 seconds."
}
```

## Expose rate limit information on response headers

By default, Azure API Management doesn't expose rate limit information on response headers. Not communicating rate limits makes it difficult for apps to avoid exceeding the limit and getting throttled. To expose rate limit information, extend the `rate-limit` policy with the `remaining-calls-header-name` and `total-calls-header-name` properties.

```xml
<policies>
  <inbound>
    <base />
    <rate-limit calls="1000" renewal-period="60" remaining-calls-header-name="ratelimit-remaining" total-calls-header-name="ratelimit-limit" />
  </inbound>
  <backend>
    <base />
  </backend>
  <outbound>
    <base />
  </outbound>
  <on-error>
    <base />
  </on-error>
</policies>
```

When you call your API now, each response includes the `ratelimit-remaining` and `ratelimit-limit` headers, which communicate how many more calls the API can handle before exceeding the limit.

## Summary

Implementing rate limiting in Azure API Management helps you create robust and scalable APIs. By using rate limiting, you can ensure that your API serves your users reliably and efficiently. Remember, the key is to find the right balance – too strict, and you might hinder usability; too lenient, and you risk overwhelming your API. With careful planning and continuous monitoring, you can achieve this balance and maintain a healthy API environment.

## Next steps

> [!div class="nextstepaction"]
> [Test that my application handles rate limiting properly](../how-to/simulate-rate-limit-api-responses.md)
