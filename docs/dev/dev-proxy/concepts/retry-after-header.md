---
title: "The Retry-After header: how long to wait before you retry"
description: What the HTTP Retry-After header means, its seconds and HTTP-date formats, which status codes use it, what to do when it's missing, and how to test that your app waits long enough.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Understand and honor the HTTP Retry-After response header -->
<!-- AUDIENCE: Developers who saw a Retry-After header in a failed response and searched for it -->

# The Retry-After header: how long to wait before you retry

`Retry-After` is an HTTP response header that tells your app how long to wait before it sends the next request. The value is either a number of seconds or an HTTP date. When an API sends it, it's the most reliable answer to "when can I try again?" because it comes from the server that turned your request down. For more information, see [RFC 9110, section 10.2.3](https://www.rfc-editor.org/rfc/rfc9110#field.retry-after).

## What Retry-After looks like

The header has 2 formats. Your app needs to handle both.

| Format | Example | What it means |
|---|---|---|
| Seconds | `Retry-After: 120` | Wait 120 seconds (2 minutes) from when you got the response. The value is a non-negative whole number. |
| HTTP date | `Retry-After: Fri, 31 Dec 1999 23:59:59 GMT` | Don't send the request again before this time. The date is always in GMT. |

Servers send `Retry-After` with these status codes:

| Status | What `Retry-After` means | Source |
|---|---|---|
| `429 Too Many Requests` | How long to wait before you send a new request. The server may include it. | [RFC 6585, section 4](https://www.rfc-editor.org/rfc/rfc6585#section-4) |
| `503 Service Unavailable` | How long the service is expected to be unavailable. The server may include it. | [RFC 9110, section 15.6.4](https://www.rfc-editor.org/rfc/rfc9110#status.503) |
| `413 Content Too Large` | If the condition is temporary, the server should say after how long it's over. | [RFC 9110, section 15.5.14](https://www.rfc-editor.org/rfc/rfc9110#status.413) |
| Any `3xx` redirect | The minimum time to wait before you follow the redirect. | [RFC 9110, section 10.2.3](https://www.rfc-editor.org/rfc/rfc9110#field.retry-after) |

The header is optional. Some APIs use their own headers instead. For example, GitHub tells you when your limit resets with `x-ratelimit-reset`. For more information, see [GitHub API rate limit exceeded](github-api-rate-limit-exceeded.md).

## How to handle Retry-After

1. **Read both formats.** If the value is a number, it's seconds. Otherwise, parse it as a date and subtract the current time. If the date is already in the past, you can retry right away.
1. **Wait at least as long as the header says.** Retrying earlier usually gets you another `429` or `503`. Some APIs keep counting your requests while they throttle you, so early retries can make the wait longer. For example, see [Microsoft Graph throttling guidance](/graph/throttling).
1. **Fall back to backoff with jitter when the header is missing.** Double the wait after each failed attempt, add a random amount so that many clients don't retry at the same moment, and cap the wait.
1. **Limit your retries.** After a few attempts, return the error to the caller.
1. **Check that a retry can help.** Some APIs return `429` when your credits or spend limit are used up. Waiting won't fix those. For an example, see [OpenAI insufficient_quota and credit_balance_exhausted](openai-insufficient-quota.md).

```javascript
function retryDelayMs(response, attempt) {
  const value = response.headers.get('retry-after');
  if (value) {
    const seconds = Number(value);
    const ms = Number.isNaN(seconds) ? Date.parse(value) - Date.now() : seconds * 1000;
    if (!Number.isNaN(ms)) {
      return Math.max(ms, 0);
    }
  }
  // No usable header: exponential backoff with jitter, capped at 30 seconds
  return Math.random() * Math.min(30_000, 1_000 * 2 ** attempt);
}
```

### What popular SDKs do

Many SDKs handle `Retry-After` for you, but only until they run out of retries. Then your code gets the error.

| SDK | What it does by default |
|---|---|
| .NET [standard resilience handler](/dotnet/core/resilience/http-resilience#standard-resilience-handler-defaults) | Retries `408`, `429`, and `5xx` responses up to 3 times with exponential backoff and jitter. It uses `Retry-After` for the delay because [`ShouldRetryAfterHeader`](/dotnet/api/microsoft.extensions.http.resilience.httpretrystrategyoptions.shouldretryafterheader) defaults to `true`. |
| [Microsoft Graph SDKs](/graph/throttling) | Use `Retry-After` when it's present, and fall back to exponential backoff when it isn't. Requests inside a JSON batch aren't retried automatically. |
| [OpenAI Python SDK](https://github.com/openai/openai-python#retries) | Retries connection errors and `408`, `409`, `429`, and `5xx` responses 2 times with a short exponential backoff. Set `max_retries` to change it. |

Check your SDK's documentation for the exact policy, and test what happens after the last retry fails.

## How to test that your app handles Retry-After

You rarely get a `Retry-After` while you develop, and when you do, you can't control its value. So the way you test this decides whether you find the bugs before your users do.

| Approach | What you find | What you miss |
|---|---|---|
| Wait for production | Real failures | Everything, until a user hits it |
| Mock the API in your tests, or let your coding agent write the mock | Whether your code parses the header | Whether your real HTTP client or SDK waits long enough, and what the API really sends. Your app also needs a test-only switch to reach the mock. |
| Call the real API until it throttles you | Real behavior | You can't trigger a throttled response on demand, and you use up your real quota |
| Intercept your app's real traffic and return throttled responses on demand | Whether your real SDK and retry policy wait as long as the header says | Nothing in your app changes, so it doesn't test your code in isolation. Keep your unit tests for that. |

## Try it on your app

[Dev Proxy](../overview.md?WT.mc_id=devproxy-learn-retry-after-header) intercepts your app's requests to the APIs you choose and returns `429` responses with a `Retry-After` header, while your app keeps calling the real URLs. The [RetryAfterPlugin](../technical-reference/retryafterplugin.md?WT.mc_id=devproxy-learn-retry-after-header) remembers when each throttled request may be retried. If your app calls the same URL before that time, Dev Proxy reports it and throttles the request again. The plugin tracks `429` responses only.

In your errors file for the [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md?WT.mc_id=devproxy-learn-retry-after-header), set the `Retry-After` value of a `429` response to `@dynamic`, and Dev Proxy fills in the number of seconds and tracks it for you.

To try it, download a preset that uses both plugins, and start Dev Proxy with it:

```console
devproxy config get openai-throttling
devproxy --config-file "~dataFolder/configs/openai-throttling/.devproxy/devproxyrc.json"
```

Then run your app as usual and watch what it does. To install Dev Proxy, see [Set up Dev Proxy](../get-started/set-up.md?WT.mc_id=devproxy-learn-retry-after-header).

## Next steps

> [!div class="nextstepaction"]
> [Test that my application handles throttling properly](../how-to/test-that-my-application-handles-throttling-properly.md?WT.mc_id=devproxy-learn-retry-after-header)

## See also

- [429 Too Many Requests: what it means and how to handle it](http-429-too-many-requests.md)
- [How to handle API throttling](how-to-handle-api-throttling.md)
- [RetryAfterPlugin](../technical-reference/retryafterplugin.md?WT.mc_id=devproxy-learn-retry-after-header)
- [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md?WT.mc_id=devproxy-learn-retry-after-header)
- [Test retries and timeouts in .NET apps](../how-to/test-dotnet-http-resilience.md?WT.mc_id=devproxy-learn-retry-after-header)
