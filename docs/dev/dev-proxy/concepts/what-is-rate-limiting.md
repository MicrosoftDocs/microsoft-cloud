---
title: What Is Rate Limiting?
description: What API rate limiting is, how APIs like GitHub and OpenAI tell you about their limits, what happens when you exceed them, and how to test that your app handles it.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Understand rate limiting concepts (requests per time period) -->

# What is rate limiting?

Rate limiting is a control mechanism that cloud APIs use to regulate the number of requests that a user can make in a specific time. Cloud API producers use rate limiting to ensure that the flow of requests doesn't overwhelm the service. Rate limiting sets a cap on the speed and volume of API calls. Rate limits are typically defined in terms of requests per period of time.

## Why cloud APIs use rate limiting

- **Prevent overload.** Rate limiting ensures that the API server remains stable and responsive by preventing any single user or service from flooding it with too many requests.
- **Ensure fair usage.** Rate limiting enforces fair usage policies by ensuring that no single user monopolizes the API resources. Rate limiting allows equitable access to all users.
- **Increase security.** Rate limiting helps in mitigating Distributed Denial of Service attacks and other abusive behaviors by restricting the number of requests from potentially malicious sources.
- **Manage costs.** For cloud service providers, rate limiting helps in managing operational costs by preventing unpredictable or excessive use of resources.
- **Maintain quality of service.** Rate limiting ensures a consistent quality of service for all users by preventing traffic spikes.

## How you experience rate limiting in your apps

When you build apps that integrate cloud APIs, check their documentation to verify if they support rate limiting. If they do, you receive `RateLimit-...` or `X-RateLimit-...` response headers with information about the rate limits. You can use this information in your application to ensure that you don't exceed the API's rate limits. For example, the `RateLimit-Remaining` header indicates the number of requests remaining in the current window. If you receive a response with this header set to 0, you know that you reached the rate limit and should wait for the next window before you send another request. The `RateLimit-Reset` header indicates the time when the rate limit resets. Some APIs send the `RateLimit-...` headers only after you reach a threshold. An example is when you have 10% of the requests remaining.

When you exceed the rate limit, the API throttles your requests and returns an HTTP [`429 Too Many Requests`](http-429-too-many-requests.md) status code. Some APIs might also send a `Retry-After` header to indicate how long you should wait before you send another request. Not every API follows this pattern. GitHub, for example, can answer with `403` instead of `429`.

To avoid throttling and ensure that your application remains responsive, implement rate limiting in your application. Depending on your technology stack, different libraries can help you handle rate limiting in your application. After you implement rate limiting in your application, test to see if it handles rate limiting properly.

## How does rate limiting affect your app?

When an API starts returning `429` responses, an app that doesn't handle them crashes, shows a generic error, retries so fast that it stays throttled, or silently drops data. You rarely see any of this while you develop, because the API is fast, you're the only user, and your test data is small.

## How to test that your app handles rate limits

| Approach | What you find | What you miss |
|---|---|---|
| Wait for production | Real rate limits | Everything, until a user hits it |
| Mock the API in your tests, or let your coding agent write the mock | Whether your retry branch runs | The API's real status codes, rate limit headers, and error bodies, and your SDK's retry policy. Your app also needs a test-only switch to reach the mock. |
| Call the real API until it throttles you | Real behavior | You can't hit the limit on demand, you use up your real quota, and some limits reset only after an hour |
| Intercept your app's real traffic and simulate the limit | Real URLs, your real SDK and retry policy, and the API's own headers, with a limit you choose | Your code in isolation. Keep your unit tests for that. |

## Try it on your app

[Dev Proxy](../overview.md?WT.mc_id=devproxy-learn-what-is-rate-limiting) counts your app's requests to the APIs you choose, adds rate limit headers to the responses, and returns `429` when your app goes over the limit, while your app keeps calling the real URLs. It also tells you when your app calls the API again before the limit resets.

Download the preset for the API your app calls, and start Dev Proxy with it:

```console
devproxy config get github-rate-limiting
devproxy --config-file "~dataFolder/configs/github-rate-limiting/.devproxy/devproxyrc.json"
```

For Microsoft Graph calls to OneDrive and SharePoint (`/drive`, `/shares`, `/sites`), use the `microsoft-graph-rate-limiting` preset. To set your own limit for any API, see [Simulate rate limit API responses](../how-to/simulate-rate-limit-api-responses.md?WT.mc_id=devproxy-learn-what-is-rate-limiting). To install Dev Proxy, see [Set up Dev Proxy](../get-started/set-up.md?WT.mc_id=devproxy-learn-what-is-rate-limiting).

## Next steps

> [!div class="nextstepaction"]
> [How to handle rate limiting](how-to-handle-rate-limiting.md)

- [429 Too Many Requests: what it means and how to handle it](http-429-too-many-requests.md)
- [Test how your app handles GitHub API rate limits](../how-to/test-github-api-rate-limit-handling.md?WT.mc_id=devproxy-learn-what-is-rate-limiting)
- [Simulate rate limiting on any API](../how-to/simulate-rate-limit-api-responses.md?WT.mc_id=devproxy-learn-what-is-rate-limiting)
- [What is throttling?](what-is-throttling.md)
