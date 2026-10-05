---
title: What Is Throttling?
description: What API throttling is, how APIs like Microsoft Graph, GitHub, and OpenAI throttle your app, and how to test that your app handles 429 responses and Retry-After.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Understand API throttling concepts (429 errors, service protection) -->

# What is throttling?

Throttling is a technique that cloud APIs use to limit the number of requests that can be made in a specific period of time. Throttling ensures that the API remains available and responsive to all users. It also prevents any single user from consuming too many resources.

You can experience throttling in several ways. One common way is by using HTTP status codes. For example, when you exceed the allowed number of requests, the API might return a [`429 Too Many Requests`](http-429-too-many-requests.md) status code. This response indicates that you issued too many requests in a specific period of time and should slow down. Not every API uses `429`: GitHub can also answer with `403`, and Anthropic uses `529` when its whole API is overloaded.

In addition to status codes, some APIs provide more information in the response headers or body. For example, they might use the [`Retry-After`](retry-after-header.md) header to indicate how long you should wait before you make another request.

You need to be aware of the throttling limits of the APIs that you use, and know how to [handle throttling](how-to-handle-api-throttling.md) in your apps, so that they stay responsive and reliable when the API is under heavy load.

## How throttling affects your app

When an API throttles your app and the app doesn't handle it, the app crashes, shows a generic error, retries so fast that it stays throttled, or silently drops data. You rarely see any of this while you develop, because the API is fast, you're the only user, and your test data is small.

## How to test that your app handles throttling

| Approach | What you find | What you miss |
|---|---|---|
| Wait for production | Real throttling | Everything, until a user hits it |
| Mock the API in your tests, or let your coding agent write the mock | Whether your retry branch runs | The API's real status codes, `Retry-After` headers, and error bodies, and your SDK's retry policy. Your app also needs a test-only switch to reach the mock. |
| Call the real API until it throttles you | Real behavior | You can't trigger throttling on demand, and you use up your real quota |
| Intercept your app's real traffic and simulate throttling | Real URLs, your real SDK and retry policy, and whether your app waits as long as `Retry-After` says | Your code in isolation. Keep your unit tests for that. |

## Try it on your app

[Dev Proxy](../overview.md?WT.mc_id=devproxy-learn-what-is-throttling) returns throttling responses for the APIs you choose, while your app keeps calling the real URLs. It also tells you when your app calls the API again before the `Retry-After` time is up.

Download the preset for the API your app calls, and start Dev Proxy with it:

```console
devproxy config get microsoft-graph-rate-limiting
devproxy --config-file "~dataFolder/configs/microsoft-graph-rate-limiting/.devproxy/devproxyrc.json"
```

| API | Preset |
|---|---|
| Microsoft Graph (OneDrive and SharePoint: `/drive`, `/shares`, `/sites`) | `microsoft-graph-rate-limiting` |
| GitHub | `github-rate-limiting` |
| OpenAI | `openai-throttling` |
| Anthropic | `anthropic-throttling` |

For any other API, see [Test that my application handles throttling properly](../how-to/test-that-my-application-handles-throttling-properly.md?WT.mc_id=devproxy-learn-what-is-throttling). To install Dev Proxy, see [Set up Dev Proxy](../get-started/set-up.md?WT.mc_id=devproxy-learn-what-is-throttling).

## Next steps

> [!div class="nextstepaction"]
> [Test that my application handles throttling properly](../how-to/test-that-my-application-handles-throttling-properly.md?WT.mc_id=devproxy-learn-what-is-throttling)

- [How to handle API throttling](how-to-handle-api-throttling.md)
- [What is rate limiting?](what-is-rate-limiting.md)
- [429 Too Many Requests: what it means and how to handle it](http-429-too-many-requests.md)
