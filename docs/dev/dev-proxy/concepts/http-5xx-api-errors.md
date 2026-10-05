---
title: "500, 502, 503, and 504 errors from APIs: what they mean and how to handle them"
description: What HTTP 500, 502, 503, and 504 responses from an API mean, which ones are safe to retry, how to use Retry-After and circuit breakers, and how to test that your app handles them.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Understand and handle HTTP 500, 502, 503, and 504 errors from APIs -->
<!-- AUDIENCE: Developers whose app calls an API and got a 5xx error -->

# 500, 502, 503, and 504 errors from APIs: what they mean and how to handle them

A status code from 500 to 599 means that the server failed to fulfill a request that looked valid. Your request isn't the problem, so sending it again can work. Whether you should send it again depends on the status code and on what the request does. For the definitions, see [RFC 9110, section 15.6](https://www.rfc-editor.org/rfc/rfc9110#section-15.6).

## What each status code means

| Status code | What it means | Retry? |
|---|---|---|
| `500 Internal Server Error` | The server hit a condition it didn't expect | It depends on the API. Some APIs, like the [Claude API](https://platform.claude.com/docs/en/api/errors), tell you to retry a 500 with exponential backoff. Check the API's docs. |
| `502 Bad Gateway` | A gateway or proxy got an invalid response from the server behind it | Yes, if the request is safe to repeat |
| `503 Service Unavailable` | The server is temporarily overloaded or down for maintenance, and it should recover after some time. The server can send a `Retry-After` header. | Yes, after the `Retry-After` time if the server sent one |
| `504 Gateway Timeout` | A gateway or proxy didn't get a response in time from the server behind it | Yes, if the request is safe to repeat. The gateway gave up waiting, so you don't know whether the server did the work. |

## Which requests are safe to retry

RFC 9110 calls a method idempotent when sending the same request several times has the same effect as sending it once. `GET`, `HEAD`, `OPTIONS`, `TRACE`, `PUT`, and `DELETE` are idempotent. `POST` and `PATCH` aren't. According to the RFC, a client shouldn't automatically retry a request with a non-idempotent method unless it knows that the request is idempotent anyway, or it can tell that the server never applied the original request. For details, see [Idempotent methods](https://www.rfc-editor.org/rfc/rfc9110#section-9.2.2).

A retried `POST` after a 502 or 504 can create a second order or send a second email. Some retry libraries retry every method by default. For example, the .NET standard resilience handler retries `POST` unless you call `options.Retry.DisableForUnsafeHttpMethods()`. For details, see [Build resilient HTTP apps](/dotnet/core/resilience/http-resilience).

## Retry-After on a 503

A 503 can include a `Retry-After` header. Its value is either a number of seconds, like `120`, or an HTTP date, like `Fri, 31 Dec 1999 23:59:59 GMT`. Your code needs to handle both. For details, see [Retry-After](https://www.rfc-editor.org/rfc/rfc9110#section-10.2.3).

## Stop calling an API that keeps failing

Retries help with short failures. When an API is down for minutes, retrying every request adds load to a server that's already struggling, and your users wait for each retry to fail. A circuit breaker tracks failures, and when there are too many, it stops calling the API for a while and fails fast. After that time, it lets a few requests through to check whether the API recovered. For more information, see [Circuit Breaker pattern](/azure/architecture/patterns/circuit-breaker). The .NET [standard resilience handler](/dotnet/core/resilience/http-resilience#standard-resilience-handler-defaults) includes a circuit breaker that opens for 5 seconds when at least 10% of requests fail in a 30-second window with at least 100 requests.

## How to handle 5xx errors

1. **Retry 502, 503, and 504 for idempotent requests only.** For `POST` and `PATCH`, retry only if the API documents a way to make them safe to repeat.
1. **Wait before you retry.** Use `Retry-After` when the server sends it. Otherwise, use exponential backoff with random jitter, and stop after a few attempts.
1. **Read the API's docs for 500.** Retry it only if the API says that it's safe.
1. **Stop calling an API that keeps failing.** Use a circuit breaker so that your app fails fast while the API recovers.
1. **Tell the user what happened.** Show "the service is having problems, try again later" instead of a generic error or a stack trace.

```javascript
const RETRYABLE_STATUS = new Set([502, 503, 504]);
const IDEMPOTENT_METHODS = new Set(["GET", "HEAD", "OPTIONS", "TRACE", "PUT", "DELETE"]);

function retryAfterMs(response) {
  const value = response.headers.get("retry-after");
  if (!value) return null;
  const seconds = Number(value);
  if (Number.isFinite(seconds)) return seconds * 1000;
  const date = Date.parse(value);
  return Number.isNaN(date) ? null : Math.max(0, date - Date.now());
}

export async function fetchWithRetry(url, options = {}, maxRetries = 3) {
  const method = (options.method ?? "GET").toUpperCase();
  const canRetry = IDEMPOTENT_METHODS.has(method);

  for (let attempt = 0; ; attempt++) {
    const response = await fetch(url, options);
    if (!RETRYABLE_STATUS.has(response.status) || !canRetry || attempt === maxRetries) {
      return response;
    }
    await response.body?.cancel();
    const backoff = 2 ** attempt * 1000 + Math.random() * 1000;
    const wait = retryAfterMs(response) ?? backoff;
    await new Promise((resolve) => setTimeout(resolve, wait));
  }
}
```

## How to test that your app handles 5xx errors

You rarely see a 5xx while you develop, and you can't make an API fail on demand. The way you test decides whether you find the bugs before your users do.

| Approach | What you find | What you miss |
|---|---|---|
| Wait for production | Real outages | Everything, until a user hits it |
| Mock the API in your tests, or let your coding agent write the mock | Whether your error branch runs | Your real HTTP client and retry library, and how many times it actually retries. Your app also needs a test-only switch to reach the mock. |
| Call the real API and wait for it to fail | Real behavior | You can't make the API fail on demand |
| Intercept your app's real traffic and return 5xx errors at a rate you choose | Your real HTTP client, retry library, and circuit breaker | Nothing in your app changes, so it doesn't test your code in isolation. Keep your unit tests for that. |

## Try it on your app

[Dev Proxy](../overview.md?WT.mc_id=devproxy-learn-http-5xx-api-errors) intercepts your app's requests and fails a share of them with the errors you define, using the [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md?WT.mc_id=devproxy-learn-http-5xx-api-errors). Your app keeps calling the real URL. Add the plugin to your configuration file and point its `errorsFile` at a file with the 5xx errors. This example uses `https://api.contoso.com`. Replace it with the URL of the API that your app calls.

**File:** server-errors.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/genericrandomerrorplugin.errorsfile.schema.json",
  "errors": [
    {
      "request": {
        "url": "https://api.contoso.com/*"
      },
      "responses": [
        { "statusCode": 500 },
        { "statusCode": 502 },
        {
          "statusCode": 503,
          "headers": [
            { "name": "Retry-After", "value": "10" }
          ]
        },
        { "statusCode": 504 }
      ]
    }
  ]
}
```

By default, the plugin fails 50% of requests. Check in the Dev Proxy output that your app retries `GET` requests and sends each `POST` only once. Dev Proxy doesn't check whether your app waits for `Retry-After` on a 503, so compare the request times yourself. Then start Dev Proxy with `--failure-rate 100` to see what your app does when the API keeps failing. For more information, see [Change request failure rate](../how-to/change-request-failure-rate.md?WT.mc_id=devproxy-learn-http-5xx-api-errors). To install Dev Proxy, see [Set up Dev Proxy](../get-started/set-up.md?WT.mc_id=devproxy-learn-http-5xx-api-errors).

## Next steps

> [!div class="nextstepaction"]
> [Test my app with random errors](../how-to/test-my-app-with-random-errors.md?WT.mc_id=devproxy-learn-http-5xx-api-errors)

## See also

- [What is chaos testing?](what-is-chaos-testing.md)
- [.NET HttpClient timeouts: TaskCanceledException and TimeoutRejectedException](dotnet-httpclient-timeouts.md)
- [429 Too Many Requests: what it means and how to handle it](http-429-too-many-requests.md)
- [Test retries and timeouts in .NET apps that use Microsoft.Extensions.Http.Resilience](../how-to/test-dotnet-http-resilience.md?WT.mc_id=devproxy-learn-http-5xx-api-errors)
