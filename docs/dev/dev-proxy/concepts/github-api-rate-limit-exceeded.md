---
title: "GitHub API rate limit exceeded: what it means and how to handle it"
description: What GitHub's primary and secondary rate limits are, how to read the x-ratelimit headers and retry-after, how long to wait after a 403 or 429, and how to test your handling.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Understand and handle GitHub REST API rate limit errors -->
<!-- AUDIENCE: Developers who got "API rate limit exceeded" from GitHub and searched for it -->

# GitHub API rate limit exceeded: what it means and how to handle it

GitHub limits how many REST API requests your app can send. When you go over a limit, GitHub turns down your requests with a `403` or `429` status until the limit resets or the wait time passes. GitHub has 2 kinds of limits: a primary limit on requests per hour, and secondary limits that protect against bursts. If your app keeps sending requests while it's rate limited, GitHub may ban your integration. For more information, see [Rate limits for the REST API](https://docs.github.com/rest/using-the-rest-api/rate-limits-for-the-rest-api).

## What GitHub's rate limits look like

| Limit | Value | When you go over it |
|---|---|---|
| Primary, unauthenticated | 60 requests per hour, per IP address | `403` or `429`, and `x-ratelimit-remaining` is `0` |
| Primary, personal access token | 5,000 requests per hour | Same as above |
| Primary, `GITHUB_TOKEN` in GitHub Actions | 1,000 requests per hour, per repository | Same as above |
| Secondary | For example, no more than 100 concurrent requests, 900 points per minute for REST endpoints, and about 80 content-generating requests per minute | `403` or `429` with an error message. `retry-after` might be present. |

GitHub can change secondary limits without notice, and there's no way to check how close you are to them.

Every response includes headers that tell you where you stand on the primary limit:

| Header | What it tells you |
|---|---|
| `x-ratelimit-limit` | The most requests you can send per hour |
| `x-ratelimit-remaining` | How many requests you have left in the current window |
| `x-ratelimit-used` | How many requests you sent in the current window |
| `x-ratelimit-reset` | When the window resets, in UTC epoch seconds |
| `x-ratelimit-resource` | Which limit the request counted against |

## How to handle a GitHub rate limit

1. **Tell a rate limit apart from a permission error.** GitHub also returns `403` when your token lacks access. If the response has no `retry-after`, `x-ratelimit-remaining` isn't `0`, and the message doesn't mention a rate limit, it's a permission problem. Don't retry it.
1. **Follow `retry-after` first.** If the header is present, wait that many seconds.
1. **Otherwise, wait for the reset.** If `x-ratelimit-remaining` is `0`, don't retry until the time in `x-ratelimit-reset`.
1. **Otherwise, wait at least 1 minute.** For a secondary limit without either header, GitHub asks you to wait at least 1 minute and to wait longer after each failed retry. Stop after a set number of retries and raise an error.
1. **Slow down before you run out.** Use `x-ratelimit-remaining` and `x-ratelimit-reset` to pace your requests. Don't build logic around an exact remaining count, because GitHub can change limits. The `x-ratelimit-*` headers are the source of truth, not the `GET /rate_limit` endpoint.

```javascript
async function githubWaitMs(response, attempt) {
  if (response.status !== 403 && response.status !== 429) {
    return null;
  }
  const retryAfter = response.headers.get('retry-after');
  if (retryAfter) {
    return Number(retryAfter) * 1000;
  }
  if (response.headers.get('x-ratelimit-remaining') === '0') {
    const resetMs = Number(response.headers.get('x-ratelimit-reset')) * 1000;
    return Math.max(resetMs - Date.now(), 0);
  }
  const { message = '' } = await response.clone().json().catch(() => ({}));
  if (response.status === 429 || /rate limit/i.test(message)) {
    return 60_000 * 2 ** attempt;
  }
  // A 403 without rate limit signals is a permission problem: don't retry
  return null;
}
```

Your caller retries when the function returns a number, and stops after a few attempts.

## How to test that your app handles GitHub rate limits

You rarely hit a GitHub rate limit while you develop. You send a few requests, and 5,000 per hour feels endless. So the way you test rate limit handling decides whether you find the bugs before your users do.

| Approach | What you find | What you miss |
|---|---|---|
| Wait for production | Real failures | Everything, until a user hits it |
| Mock the API in your tests, or let your coding agent write the mock | Whether your retry branch runs | GitHub's real headers and error bodies, and your SDK's retry policy. Your app also needs a test-only switch to reach the mock. |
| Call the real API until it limits you | Real behavior | It takes up to 5,000 requests, you can't trigger a secondary limit on demand, and you risk a ban on your integration |
| Intercept your app's real traffic and return rate limit responses on demand | Real URLs, your real SDK and retry policy, and GitHub's own headers and error format | Nothing in your app changes, so it doesn't test your code in isolation. Keep your unit tests for that. |

## Try it on your app

[Dev Proxy](../overview.md?WT.mc_id=devproxy-learn-github-api-rate-limit-exceeded) intercepts your app's requests to `api.github.com` and returns GitHub-style rate limit responses, while your app keeps calling the real URLs. The `github-rate-limiting` preset counts your requests against a limit of 60 per hour, sends the `x-ratelimit-*` headers, and returns a `429` with `API rate limit exceeded` when you run out. Until then, your requests go to GitHub and count against your real limit too.

Download the preset, and start Dev Proxy with it:

```console
devproxy config get github-rate-limiting
devproxy --config-file "~dataFolder/configs/github-rate-limiting/.devproxy/devproxyrc.json"
```

To test secondary limits, start Dev Proxy with `devproxyrc-secondary.json` from the same folder instead. It randomly returns a secondary rate limit `429` with a `retry-after` header.

Then run your app as usual and watch what it does. To install Dev Proxy, see [Set up Dev Proxy](../get-started/set-up.md?WT.mc_id=devproxy-learn-github-api-rate-limit-exceeded).

## Next steps

> [!div class="nextstepaction"]
> [Test how your app handles GitHub API rate limits](../how-to/test-github-api-rate-limit-handling.md?WT.mc_id=devproxy-learn-github-api-rate-limit-exceeded)

## See also

- [429 Too Many Requests: what it means and how to handle it](http-429-too-many-requests.md)
- [The Retry-After header: how long to wait before you retry](retry-after-header.md)
- [What is rate limiting?](what-is-rate-limiting.md)
- [How to handle rate limiting](how-to-handle-rate-limiting.md)
- [Simulate rate limit API responses](../how-to/simulate-rate-limit-api-responses.md?WT.mc_id=devproxy-learn-github-api-rate-limit-exceeded)
- [RateLimitingPlugin](../technical-reference/ratelimitingplugin.md?WT.mc_id=devproxy-learn-github-api-rate-limit-exceeded)
