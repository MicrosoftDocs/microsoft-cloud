---
title: Simulate Rate-Limit API responses
description: How to simulate Rate-Limit API responses
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/24/2024
---

# Simulate Rate-Limit API responses

Rate-Limit headers are used in HTTP responses to limit the number of requests that a client can make within a given time period.

The server sends these headers in response to a client's request to indicate how many requests are allowed and how many requests remain before the limit is reached.

The `RateLimit-Limit` response header field indicates the request-quota associated with the client in the current time-window. If the client exceeds that limit, it might not be served.

## Custom rate limit support

When you exceed rate limit, some APIs use custom behaviors, such as returning a `403 Forbidden` status code with a custom error message. Dev Proxy allows you to simulate these custom behaviors by using the `Custom` value for the `whenLimitExceeded` property.

The following example shows how you can configure how you can configure [RateLimitingPlugin](../technical-reference/RateLimitingPlugin.md) in the [devproxyrc](../technical-reference/devproxyrc.md) file to simulate rate limits for the [GitHub API](https://docs.github.com/en/rest/guides/best-practices-for-using-the-rest-api?apiVersion=2022-11-28#dealing-with-rate-limits).

```json
{
  "rateLimiting": {
    "headerLimit": "X-RateLimit-Limit",
    "headerRemaining": "X-RateLimit-Remaining",
    "headerReset": "X-RateLimit-Reset",
    "costPerRequest": 1,
    "resetTimeWindowSeconds": 3600,
    "warningThresholdPercent": 0,
    "rateLimit": 60,
    "resetFormat": "UtcEpochSeconds",
    "whenLimitExceeded": "Custom",
    "customResponseFile": "github-rate-limit-exceeded.json"
  }
}
```

The `customResponseFile` contains the response that the proxy returns when your app reached the rate limit.

```json
{
  "statusCode": 403,
  "headers": [
    {
      "name": "Content-Type",
      "value": "application/json; charset=utf-8"
    }
  ],
  "body": {
    "message": "You have exceeded a secondary rate limit and have been temporarily blocked from content creation. Please retry your request again later.",
    "documentation_url": "https://docs.github.com/rest/overview/resources-in-the-rest-api#secondary-rate-limits"
  }
}
```

## Next steps

Learn more about the `RateLimitingPlugin`.

> [!div class="nextstepaction"]
> [RateLimitingPlugin](../technical-reference/ratelimitingplugin.md)