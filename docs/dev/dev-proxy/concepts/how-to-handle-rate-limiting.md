---
title: How to Handle Rate Limiting
description: How to handle API rate limits in your app with rate limit headers, local rate limiters, and batching, and how to test that your app stays within the limits.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Best practices for handling rate limiting (retries, backoff, queuing) -->

# How to handle rate limiting

[Rate limiting](what-is-rate-limiting.md) is a common technique used by API providers to manage the number of requests that can be made to their service in a specific time period. API providers use rate limiting to ensure that their service remains available and responsive to all users, and to prevent abuse or overuse of the service.

When you use cloud APIs in your application, you should understand their rate limits. The following techniques can help you handle rate limiting in your applications:

- **Understand rate limits.** Check the documentation of the API that you use to understand its rate limits. Rate limits might depend on the API provider or the service plan that you use. For example, some APIs might have different rate limits for free and paid plans.
- **Use rate limiting information.** APIs that use rate limits typically communicate the current limits in the response headers. For example, the `RateLimit-Remaining` header indicates the number of requests that remain in the current window. If you receive a response with this header set to 0, you know that you reached the rate limit and should wait for the next window before you send another request. The `RateLimit-Reset` header indicates the time when the rate limit resets. Header names and formats differ per API: GitHub, for example, uses `x-ratelimit-remaining` and `x-ratelimit-reset` in UTC epoch seconds. Some APIs send the headers only after you reach a threshold. An example is when you have 10% of the requests remaining.
- **Optimize API usage.** Some services assign different costs to different requests based on their complexity. For example, some APIs might charge more for requests that return more data. To reduce the cost of your application, optimize your API usage by fetching only the data that you need. Use batch requests if the API supports them. They help you reduce the number of resources required to process the response and stay within the rate limits.
- **Implement a local rate limiter.** Implement a rate limiter within the application itself to limit the number of requests that can be made to the API in a specific time period. You can do it by using techniques such as token bucket or leaky bucket algorithms, which allow the application to make many requests per time period. Any more requests are queued or discarded.
- **Avoid exceeding rate limits.** When you exceed rate limits, the API [throttles](what-is-throttling.md) your requests, typically with an HTTP [`429 Too Many Requests`](http-429-too-many-requests.md) status code. Throttling affects the throughput of your application more than rate limiting does. Use the information in rate limit response headers to stay within the limits and avoid throttling.

By using these techniques, you can build applications that are resilient to rate limiting and can continue to function even when the API is under heavy load. Then test that they do, because you rarely hit a rate limit while you develop.

## How to test your rate limit handling

| Approach | What you find | What you miss |
|---|---|---|
| Wait for production | Real rate limits | Everything, until a user hits it |
| Mock the API in your tests, or let your coding agent write the mock | Whether your code reads the headers you mocked | The API's real header names, formats, and reset behavior, and your SDK's retry policy. Your app also needs a test-only switch to reach the mock. |
| Call the real API until you hit the limit | Real behavior | You can't hit the limit on demand, you use up your real quota, and some limits reset only after an hour |
| Intercept your app's real traffic and simulate the limit | Real URLs, a limit and time window you choose, and the API's own headers | Your code in isolation. Keep your unit tests for that. |

## Try it on your app

[Dev Proxy](../overview.md?WT.mc_id=devproxy-learn-how-to-handle-rate-limiting) counts your app's requests to the APIs you choose, adds rate limit headers to the responses, and answers with `429` when your app goes over the limit, while your app keeps calling the real URLs. You choose the limit and the time window, so you can hit it in a minute instead of an hour.

For GitHub, download the preset and start Dev Proxy with it:

```console
devproxy config get github-rate-limiting
devproxy --config-file "~dataFolder/configs/github-rate-limiting/.devproxy/devproxyrc.json"
```

For Microsoft Graph calls to OneDrive and SharePoint (`/drive`, `/shares`, `/sites`), use the `microsoft-graph-rate-limiting` preset. To install Dev Proxy, see [Set up Dev Proxy](../get-started/set-up.md?WT.mc_id=devproxy-learn-how-to-handle-rate-limiting).

## Next step

> [!div class="nextstepaction"]
> [Test that my application handles rate limiting properly](../how-to/simulate-rate-limit-api-responses.md?WT.mc_id=devproxy-learn-how-to-handle-rate-limiting)

- [Test how your app handles GitHub API rate limits](../how-to/test-github-api-rate-limit-handling.md?WT.mc_id=devproxy-learn-how-to-handle-rate-limiting)
- [GitHub API rate limit exceeded: what it means and how to handle it](github-api-rate-limit-exceeded.md)
