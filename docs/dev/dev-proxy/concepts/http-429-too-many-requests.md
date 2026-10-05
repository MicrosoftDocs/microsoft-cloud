---
title: "429 Too Many Requests: what it means and how to handle it"
description: What the HTTP 429 Too Many Requests status means, what it looks like in GitHub, OpenAI, Anthropic, and Microsoft Graph, how to handle it, and how to test your handling.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Understand and handle HTTP 429 Too Many Requests -->
<!-- AUDIENCE: Developers who got a 429 from an API and searched for it -->

# 429 Too Many Requests: what it means and how to handle it

An API returns `429 Too Many Requests` when your app sent more requests than the API allows in a period of time. The request itself is fine. You sent it too often, so the API turned it down. If you wait and send it again, it usually succeeds. The response often includes a `Retry-After` header that tells you how long to wait. For more information, see [RFC 6585, section 4](https://www.rfc-editor.org/rfc/rfc6585#section-4).

## What a 429 looks like in popular APIs

Every API implements rate limits differently. The status code, the headers, and the error body all vary, so code that handles one API correctly can mishandle the next.

| API | Status | How to tell how long to wait | Watch out for |
|---|---|---|---|
| [GitHub](https://docs.github.com/rest/using-the-rest-api/rate-limits-for-the-rest-api) | `403` or `429` | `retry-after` if present, otherwise `x-ratelimit-reset` (UTC epoch seconds) when `x-ratelimit-remaining` is `0`, otherwise at least 1 minute | A `403` can be a rate limit or a missing permission. Read the headers to tell them apart. |
| [OpenAI](https://developers.openai.com/api/docs/guides/error-codes) | `429` | `retry-after` | Some 429s, like `credit_balance_exhausted`, mean that retrying won't help. Check `error.code`. |
| [Anthropic](https://platform.claude.com/docs/en/api/errors) | `429` | `retry-after` | A spend-cap 429 has no `retry-after` and keeps failing until access resumes. An overloaded API returns `529`, not `429`. |
| [Microsoft Graph](/graph/throttling) | `429` | `Retry-After` (seconds) | Limits differ per service, for example SharePoint and Outlook. |

## How to handle a 429

1. **Decide whether to retry at all.** If the error says that your quota, credits, or spend limit is used up, retrying won't help. Tell the user, and alert yourself.
1. **Wait as long as the API asks.** If the response has `Retry-After`, wait that long. It's either a number of seconds or an HTTP date. If the API uses rate limit headers instead, like GitHub's `x-ratelimit-reset`, wait until the reset time.
1. **Otherwise, back off.** Without a hint from the API, retry with exponential backoff and random jitter, and stop after a few attempts.
1. **Tell the user what's happening.** "Busy, retrying in 5 seconds" beats a spinner that never ends.
1. **Slow down before the next 429.** If the API sends rate limit headers, use the remaining count to pace your requests.

```javascript
async function fetchWithRetry(url, options, attempts = 3) {
  for (let attempt = 1; ; attempt++) {
    const response = await fetch(url, options);
    if (response.status !== 429 || attempt === attempts) {
      return response;
    }
    const retryAfter = response.headers.get('retry-after');
    const waitMs = retryAfter
      ? (isNaN(retryAfter) ? new Date(retryAfter) - Date.now() : retryAfter * 1000)
      : 2 ** attempt * 1000 + Math.random() * 1000;
    await new Promise(resolve => setTimeout(resolve, Math.max(waitMs, 0)));
  }
}
```

Many SDKs retry 429s for you. For example, the [OpenAI Python SDK](https://github.com/openai/openai-python#retries) retries 2 times by default, and the .NET [standard resilience handler](/dotnet/core/resilience/http-resilience#standard-resilience-handler-defaults) retries 3 times and honors `Retry-After`. When the SDK runs out of retries, your code gets the error, so it still needs a plan.

## How to test that your app handles 429

You rarely see a 429 while you develop. The API is fast, you're the only user, and your test data is small. So the way you test 429 handling decides whether you find the bugs before your users do.

| Approach | What you find | What you miss |
|---|---|---|
| Wait for production | Real failures | Everything, until a user hits it |
| Mock the API in your tests, or let your coding agent write the mock | Whether your retry branch runs | The API's real status codes, headers, and error bodies, and your SDK's retry policy. Your app also needs a test-only switch to reach the mock. |
| Call the real API until it throttles you | Real behavior | You can't trigger a 429 on demand, and you use up your real quota |
| Intercept your app's real traffic and return 429s on demand | Real URLs, your real SDK and retry policy, and the API's own 429 format | Nothing in your app changes, so it doesn't test your code in isolation. Keep your unit tests for that. |

## Try it on your app

[Dev Proxy](../overview.md?WT.mc_id=devproxy-learn-http-429) intercepts your app's requests to the APIs you choose and returns 429s, with the API's own headers and error format, while your app keeps calling the real URLs. It also tells you when your app retries before the `Retry-After` time is up.

Download the preset for the API your app calls, and start Dev Proxy with it:

```console
devproxy config get github-rate-limiting
devproxy --config-file "~dataFolder/configs/github-rate-limiting/.devproxy/devproxyrc.json"
```

| API | Preset |
|---|---|
| GitHub | `github-rate-limiting` |
| OpenAI | `openai-throttling` |
| Anthropic | `anthropic-throttling` |
| Microsoft Graph (OneDrive and SharePoint: `/drive`, `/shares`, `/sites`) | `microsoft-graph-rate-limiting` |

Then run your app as usual and watch what it does. To install Dev Proxy, see [Set up Dev Proxy](../get-started/set-up.md?WT.mc_id=devproxy-learn-http-429).

## Next steps

> [!div class="nextstepaction"]
> [Test that my application handles throttling properly](../how-to/test-that-my-application-handles-throttling-properly.md?WT.mc_id=devproxy-learn-http-429)

## See also

- [What is rate limiting?](what-is-rate-limiting.md)
- [What is throttling?](what-is-throttling.md)
- [Test how your app handles GitHub API rate limits](../how-to/test-github-api-rate-limit-handling.md?WT.mc_id=devproxy-learn-http-429)
- [Test how your app handles OpenAI rate limits](../how-to/test-openai-rate-limit-handling.md?WT.mc_id=devproxy-learn-http-429)
- [Test retries and timeouts in .NET apps](../how-to/test-dotnet-http-resilience.md?WT.mc_id=devproxy-learn-http-429)
