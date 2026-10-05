---
title: ".NET HttpClient timeouts: TaskCanceledException and TimeoutRejectedException"
description: Why a slow API makes .NET HttpClient throw TaskCanceledException or Polly's TimeoutRejectedException, which timeout fired, how to catch each one, and how to test your timeout handling.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Understand and handle .NET HttpClient timeout exceptions -->
<!-- AUDIENCE: .NET developers whose app calls a slow API and got TaskCanceledException or TimeoutRejectedException -->

# .NET HttpClient timeouts: TaskCanceledException and TimeoutRejectedException

When an API is too slow, your .NET app gets 1 of 2 exceptions, depending on which timeout fired. `HttpClient.Timeout` throws a `TaskCanceledException`. The standard resilience handler from `Microsoft.Extensions.Http.Resilience` throws a `TimeoutRejectedException` from Polly. They come from different places, have different defaults, and need separate `catch` blocks.

## Which timeout fired

| Timeout | Default | What your code gets | Where you set it |
|---|---|---|---|
| `HttpClient.Timeout` | 100 seconds | `TaskCanceledException`, with a `TimeoutException` as its `InnerException` (.NET 5 and later) | `HttpClient.Timeout` |
| Standard handler attempt timeout | 10 seconds per attempt | Nothing at first: the handler retries the attempt | `AddStandardResilienceHandler(options => ...)` |
| Standard handler total timeout | 30 seconds, including all retries | `Polly.Timeout.TimeoutRejectedException` | `AddStandardResilienceHandler(options => ...)` |

### HttpClient.Timeout

`HttpClient.Timeout` applies to every request that the `HttpClient` instance sends. To use a different timeout for one request, pass a `CancellationToken` from a `CancellationTokenSource` with its own timeout. The shorter of the two applies. Set `Timeout.InfiniteTimeSpan` to turn it off.

On .NET 5 and later, a timeout throws a `TaskCanceledException` with a `TimeoutException` inside. On earlier versions of .NET Core, the inner exception isn't there. On .NET Framework, you get an `HttpRequestException` instead. For details, see [HttpClient.Timeout](/dotnet/api/system.net.http.httpclient.timeout) and [Make HTTP requests with the HttpClient class](/dotnet/fundamentals/networking/http/httpclient).

A `TaskCanceledException` also means that someone canceled the request, for example, a user who closed the page. To tell a timeout from a cancellation, check `ex.InnerException is TimeoutException`, or check whether your own token is canceled.

### The standard resilience handler

`AddStandardResilienceHandler()` chains a rate limiter, a total timeout, a retry, a circuit breaker, and an attempt timeout. When one attempt takes longer than 10 seconds, the attempt timeout cancels it and the retry strategy tries again: up to 3 retries, with exponential backoff and jitter, starting at 2 seconds. When the whole request, including retries, takes longer than 30 seconds, the total timeout cancels it and your code gets a `TimeoutRejectedException`.

`TimeoutRejectedException` derives from `Exception`. It's neither a `TimeoutException` nor an `HttpRequestException`, so a `catch (HttpRequestException)` block doesn't catch it. For the full list of defaults, see [Standard resilience handler defaults](/dotnet/core/resilience/http-resilience#standard-resilience-handler-defaults).

For example, when a .NET 10 app that uses the standard handler's defaults calls an API that takes 11 to 15 seconds per response, the app gets a `TimeoutRejectedException` after 30 seconds, and its `catch (HttpRequestException)` block doesn't run.

## How to handle HttpClient timeouts

1. **Catch both exceptions where you call the API.** If you use the standard handler, catch `TimeoutRejectedException`. Catch `TaskCanceledException` for `HttpClient.Timeout`.
1. **Tell a timeout from a cancellation.** Only treat a `TaskCanceledException` as a timeout when its inner exception is a `TimeoutException`. When the caller canceled, stop quietly.
1. **Pick timeouts that fit the API.** If the API often takes longer than 10 seconds, change the attempt and total timeouts in `AddStandardResilienceHandler(options => ...)`.
1. **Don't retry `POST` or `PATCH` unless the API makes it safe.** The standard handler retries all methods by default, including `POST`. Call `options.Retry.DisableForUnsafeHttpMethods()` to exclude `POST`, `PATCH`, `PUT`, `DELETE`, and `CONNECT`, or `options.Retry.DisableFor(HttpMethod.Post, HttpMethod.Patch)` to keep retrying idempotent `PUT` and `DELETE`.
1. **Tell the user what happened.** Show "the service is slow, try again" instead of a generic error.

```csharp
using Polly.Timeout;

public async Task<string?> GetForecastAsync(HttpClient client, CancellationToken cancellationToken)
{
    try
    {
        return await client.GetStringAsync("https://api.contoso.com/forecast", cancellationToken);
    }
    catch (TimeoutRejectedException)
    {
        // Standard resilience handler: total timeout expired after all retries
        return null;
    }
    catch (TaskCanceledException ex) when (ex.InnerException is TimeoutException)
    {
        // HttpClient.Timeout expired
        return null;
    }
    catch (HttpRequestException)
    {
        // Network error, or an error status code after all retries
        return null;
    }
}
```

## How to test that your app handles timeouts

You rarely see a timeout while you develop, so your `catch` blocks rarely run. The way you test decides whether you find the bugs before your users do.

| Approach | What you find | What you miss |
|---|---|---|
| Wait for production | Real timeouts | Everything, until a user hits it |
| Mock the API in your tests, or let your coding agent write the mock | Whether your `catch` block runs, if the mock throws the right exception | Your real `HttpClient` setup, the resilience handler's retries, and the exception that it actually throws. Your app also needs a test-only switch to reach the mock. |
| Call the real API and hope it's slow | Real behavior | You can't make the API slow on demand |
| Intercept your app's real traffic and delay the responses | Your real `HttpClient`, resilience handler, and exceptions | Nothing in your app changes, so it doesn't test your code in isolation. Keep your unit tests for that. |

## Try it on your app

[Dev Proxy](../overview.md?WT.mc_id=devproxy-learn-dotnet-httpclient-timeouts) intercepts your app's requests to the API and delays the responses with the [LatencyPlugin](../technical-reference/latencyplugin.md?WT.mc_id=devproxy-learn-dotnet-httpclient-timeouts). .NET uses the system proxy, so you don't need to change your code. This example delays each response by 11 to 15 seconds, longer than the standard handler's 10-second attempt timeout. Replace `https://api.contoso.com` with the URL of the API that your app calls.

**File:** devproxyrc.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/rc.schema.json",
  "plugins": [
    {
      "name": "LatencyPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "slowApi"
    }
  ],
  "urlsToWatch": [
    "https://api.contoso.com/*"
  ],
  "slowApi": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/latencyplugin.schema.json",
    "minMs": 11000,
    "maxMs": 15000
  }
}
```

Start Dev Proxy with `devproxy --config-file devproxyrc.json` and run your app. Each attempt times out, the handler retries, and after 30 seconds your code gets a `TimeoutRejectedException`. To test `HttpClient.Timeout` instead, set `minMs` higher than the timeout you configured. For setup details, see [Use Dev Proxy with .NET applications](../how-to/use-dev-proxy-with-dotnet.md?WT.mc_id=devproxy-learn-dotnet-httpclient-timeouts). To install Dev Proxy, see [Set up Dev Proxy](../get-started/set-up.md?WT.mc_id=devproxy-learn-dotnet-httpclient-timeouts).

## Next steps

> [!div class="nextstepaction"]
> [Test retries and timeouts in .NET apps that use Microsoft.Extensions.Http.Resilience](../how-to/test-dotnet-http-resilience.md?WT.mc_id=devproxy-learn-dotnet-httpclient-timeouts)

## See also

- [Simulate slow API responses](../how-to/simulate-slow-api-responses.md?WT.mc_id=devproxy-learn-dotnet-httpclient-timeouts)
- [500, 502, 503, and 504 errors from APIs: what they mean and how to handle them](http-5xx-api-errors.md)
- [What is chaos testing?](what-is-chaos-testing.md)
