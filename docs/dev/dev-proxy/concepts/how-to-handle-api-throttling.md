---
title: How to Handle API Throttling
description: How to handle API throttling in your app with Retry-After, exponential backoff, caching, and queues, and how to test that your handling works before production.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Best practices for handling API throttling (retries, backoff, caching) -->

# How to handle API throttling

[API throttling](./what-is-throttling.md) is a common challenge when you build applications that rely on cloud APIs. Here are techniques that you can use to handle it:

- **Use rate limiting.** If the API that you use supports [rate limiting](./what-is-rate-limiting.md), use the rate limiting information that the API sends to ensure that your application doesn't exceed the API's limits.
- **Handle Retry-After headers.** Some APIs send a [`Retry-After`](./retry-after-header.md) header in their response when a request is throttled. If you get throttled, and the response has a `Retry-After` header, wait for the specified time before you send another request.
- **Implement exponential backoff.** If the API that you use doesn't send a `Retry-After` header, implement an exponential backoff algorithm with random jitter. After each failed request, wait twice as long before you try again, and stop after a few attempts. Waiting longer helps you reduce the load on the API and increases the chances of your next requests being successful.
- **Know when not to retry.** Some throttling-like errors mean that your quota or credits are used up, for example OpenAI's [`credit_balance_exhausted`](./openai-insufficient-quota.md). Retrying those doesn't help, so stop and tell the user.
- **Cache previously received data.** Cache responses from the API, especially for requests that are likely to return the same data. [Caching](./what-is-caching.md) helps you reduce the number of calls made to the API and stay within the rate limits.
- **Queue requests.** Implement a queue for outgoing API requests to manage the request rate and ensure that the API's rate limits aren't exceeded.
- **Optimize API calls.** Fetch only the data that you need and use batch requests if the API supports them. Optimizing helps you reduce the number of requests and stay within the rate limits.
- **Show the user what's happening.** "Busy, retrying in 5 seconds" beats a spinner that never ends.

After you implement these techniques in your application, test that it handles throttling properly. Retry logic that you never saw run is retry logic that you don't know works. For a comparison of ways to test it, see [How to test that your app handles throttling](./what-is-throttling.md#how-to-test-that-your-app-handles-throttling).

## Try it on your app

[Dev Proxy](../overview.md?WT.mc_id=devproxy-learn-how-to-handle-api-throttling) returns throttling responses for the APIs you choose, while your app keeps calling the real URLs, and tells you when your app retries before the `Retry-After` time is up. Presets are available for GitHub (`github-rate-limiting`), OpenAI (`openai-throttling`), Anthropic (`anthropic-throttling`), and Microsoft Graph OneDrive and SharePoint endpoints (`microsoft-graph-rate-limiting`):

```console
devproxy config get openai-throttling
devproxy --config-file "~dataFolder/configs/openai-throttling/.devproxy/devproxyrc.json"
```

To install Dev Proxy, see [Set up Dev Proxy](../get-started/set-up.md?WT.mc_id=devproxy-learn-how-to-handle-api-throttling).

## Next step

> [!div class="nextstepaction"]
> [Test that my application handles throttling properly](../how-to/test-that-my-application-handles-throttling-properly.md?WT.mc_id=devproxy-learn-how-to-handle-api-throttling)
