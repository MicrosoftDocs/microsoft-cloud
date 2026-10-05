---
title: Test how your app handles GitHub API rate limits
description: How to test that your app handles GitHub REST API primary and secondary rate limits, including 403 and 429 responses and the x-ratelimit-remaining, x-ratelimit-reset, and retry-after headers
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
ms.topic: how-to
---

<!-- INTENT: Test that an app handles GitHub REST API rate limits correctly -->
<!-- SOLUTION: Simulate primary limits with RateLimitingPlugin and secondary limits with GenericRandomErrorPlugin, plus RetryAfterPlugin -->
<!-- RESULT: App waits for x-ratelimit-reset or retry-after instead of hammering the API -->
<!-- PLUGINS: RateLimitingPlugin, GenericRandomErrorPlugin, RetryAfterPlugin -->
<!-- JOB: test-error-handling -->
<!-- TIME: 15 minutes -->

# Test how your app handles GitHub API rate limits

> [!TIP]
> New to throttling? Learn [what throttling is](../concepts/what-is-throttling.md) and [how to handle it](../concepts/how-to-handle-api-throttling.md).

> **At a glance**  
> **Goal:** Test how your app handles GitHub REST API rate limits  
> **Time:** 15 minutes  
> **Plugins:** [RateLimitingPlugin](../technical-reference/ratelimitingplugin.md), [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md), [RetryAfterPlugin](../technical-reference/retryafterplugin.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

Your app calls the GitHub API. It works on your machine, then a CI job, a large organization, or a busy day pushes it over the rate limit, and it starts failing. To test it against the real API, you'd need to use up your rate limit, and then wait up to an hour before you can try again. Dev Proxy simulates GitHub rate limits locally, with a limit and a time window that you choose.

## Know what GitHub returns

GitHub has 2 kinds of rate limits for the REST API.

**Primary rate limits** cap how many requests you make per hour. For example, 60 for unauthenticated requests and 5,000 for requests with a personal access token. Every response includes headers that show where you are:

| Header | Meaning |
| ------ | ------- |
| `x-ratelimit-limit` | The maximum number of requests per hour |
| `x-ratelimit-remaining` | The number of requests left in the current window |
| `x-ratelimit-reset` | The time when the window resets, in UTC epoch seconds |

When you exceed the primary limit, GitHub returns `403` or `429` with `x-ratelimit-remaining` set to `0`. Don't retry until the time in `x-ratelimit-reset`.

**Secondary rate limits** protect GitHub from bursts, like too many concurrent requests or creating too much content too fast. When you exceed one, GitHub returns `403` or `429` with a message about a secondary rate limit. If the response has a `retry-after` header, wait that many seconds. Otherwise, wait at least a minute, and increase the wait time if the request keeps failing.

GitHub can ban integrations that keep sending requests while they're rate limited. For more information, see [Rate limits for the REST API](https://docs.github.com/rest/using-the-rest-api/rate-limits-for-the-rest-api) in the GitHub documentation.

## Simulate the primary rate limit

Use the [RateLimitingPlugin](../technical-reference/ratelimitingplugin.md) to count requests and return GitHub's rate limit headers. To test without waiting an hour, use a small limit and a short window.

**File:** devproxyrc.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/rc.schema.json",
  "plugins": [
    {
      "name": "RetryAfterPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    },
    {
      "name": "RateLimitingPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "githubRateLimit"
    }
  ],
  "urlsToWatch": [
    "https://api.github.com/*"
  ],
  "githubRateLimit": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/ratelimitingplugin.schema.json",
    "headerLimit": "x-ratelimit-limit",
    "headerRemaining": "x-ratelimit-remaining",
    "headerReset": "x-ratelimit-reset",
    "resetFormat": "UtcEpochSeconds",
    "costPerRequest": 1,
    "rateLimit": 5,
    "resetTimeWindowSeconds": 60,
    "warningThresholdPercent": 0,
    "whenLimitExceeded": "Custom",
    "customResponseFile": "github-rate-limit-exceeded.json"
  }
}
```

> [!CAUTION]
> Add the `RetryAfterPlugin` before the `RateLimitingPlugin` in your configuration file. If you add it after, the `RateLimitingPlugin` handles the request before the `RetryAfterPlugin` can check it.

In the custom response file, define the response that GitHub returns when you exceed the primary rate limit.

**File:** github-rate-limit-exceeded.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/ratelimitingplugin.customresponsefile.schema.json",
  "statusCode": 429,
  "headers": [
    {
      "name": "content-type",
      "value": "application/json; charset=utf-8"
    }
  ],
  "body": {
    "message": "API rate limit exceeded for user ID 1.",
    "documentation_url": "https://docs.github.com/rest/overview/rate-limits-for-the-rest-api"
  }
}
```

Start Dev Proxy and run your app.

```console
devproxy --config-file devproxyrc.json
```

Dev Proxy forwards the first 5 requests in each minute to GitHub and sets the `x-ratelimit-*` headers on the responses. From the 6th request on, Dev Proxy returns the rate limit response with `x-ratelimit-remaining` set to `0` and `x-ratelimit-reset` set to the end of the window. If your app calls the API again before the window resets, the `RetryAfterPlugin` reports it and throttles the request.

Check that your app:

- Reads `x-ratelimit-remaining` and slows down before it reaches `0`.
- Stops calling the API after a rate limit response and waits until `x-ratelimit-reset`.
- Tells the user what's happening, for example "GitHub rate limit reached, retrying at 14:05", instead of failing silently.

> [!NOTE]
> GitHub returns either `403` or `429` when you exceed a rate limit. To test that your app handles `403` too, change `statusCode` to `403`. The `RetryAfterPlugin` only tracks `429` responses, so it doesn't report early retries after a `403`.

> [!TIP]
> Dev Proxy forwards requests to GitHub until the simulated limit is reached. These requests count against your real GitHub rate limit as well.

## Simulate secondary rate limits

Secondary rate limits come in bursts and include a `retry-after` header. Use the [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md) to return them at random.

**File:** devproxyrc.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/rc.schema.json",
  "plugins": [
    {
      "name": "RetryAfterPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    },
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "githubSecondaryRateLimit"
    }
  ],
  "urlsToWatch": [
    "https://api.github.com/*"
  ],
  "githubSecondaryRateLimit": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/genericrandomerrorplugin.schema.json",
    "errorsFile": "github-secondary-rate-limit.json",
    "rate": 50,
    "retryAfterInSeconds": 60
  }
}
```

**File:** github-secondary-rate-limit.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/genericrandomerrorplugin.errorsfile.schema.json",
  "errors": [
    {
      "request": {
        "url": "https://api.github.com/*"
      },
      "responses": [
        {
          "statusCode": 429,
          "headers": [
            {
              "name": "content-type",
              "value": "application/json; charset=utf-8"
            },
            {
              "name": "retry-after",
              "value": "@dynamic"
            }
          ],
          "body": {
            "message": "You have exceeded a secondary rate limit. Please wait a few minutes before you try again.",
            "documentation_url": "https://docs.github.com/rest/overview/rate-limits-for-the-rest-api#about-secondary-rate-limits"
          }
        }
      ]
    }
  ]
}
```

Start Dev Proxy and run your app. Check that your app waits for the number of seconds in the `retry-after` header before it calls the API again. If it doesn't, the `RetryAfterPlugin` reports it.

If you use [Octokit](https://github.com/octokit) with the throttling plugin, check that your `onRateLimit` and `onSecondaryRateLimit` handlers run and that they return the result you expect.

## Next step

Learn more about the `RateLimitingPlugin`.

> [!div class="nextstepaction"]
> [RateLimitingPlugin](../technical-reference/ratelimitingplugin.md)

## See also

- [Simulate Rate-Limit API responses](./simulate-rate-limit-api-responses.md) - Rate limits on any API
- [Test that my application handles throttling properly](./test-that-my-application-handles-throttling-properly.md) - Throttling on any API
- [RetryAfterPlugin](../technical-reference/retryafterplugin.md) - Verify retry behavior
- [Use Dev Proxy in CI/CD](./use-dev-proxy-in-ci-cd-overview.md) - Automate resilience testing in your pipeline
