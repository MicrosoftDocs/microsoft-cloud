---
title: Test how your app handles OpenAI rate limits
description: How to test that your app handles OpenAI API 429 rate limit, slow_down, credit_balance_exhausted, and 503 server_is_overloaded errors, with or without the OpenAI SDK retries
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
ms.topic: how-to
---

<!-- INTENT: Test that an app handles OpenAI rate limits and overload errors correctly -->
<!-- SOLUTION: Use the openai-throttling preset (GenericRandomErrorPlugin + RetryAfterPlugin) -->
<!-- RESULT: App waits on Retry-After, stops retrying billing errors, and degrades gracefully -->
<!-- PLUGINS: GenericRandomErrorPlugin, RetryAfterPlugin -->
<!-- JOB: test-error-handling -->
<!-- TIME: 10 minutes -->

# Test how your app handles OpenAI rate limits

> **At a glance**  
> **Goal:** Test how your app handles OpenAI rate limit and overload errors  
> **Time:** 10 minutes  
> **Plugins:** [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md), [RetryAfterPlugin](../technical-reference/retryafterplugin.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

Your app works in development, then fails in production with `429 Too Many Requests`. OpenAI rate limits depend on your organization's usage tier, your model, and everyone else using the same organization, so you can't reliably hit them on purpose. Dev Proxy returns the same errors that the OpenAI API returns, so you can see what your app does before your users do.

## Know what OpenAI returns

Not every OpenAI error means "try again". Your app needs to tell them apart.

| Status | `error.code` | What it means | What your app should do |
| ------ | ------------ | ------------- | ----------------------- |
| 429 | `rate_limit_exceeded` | You hit your requests per minute (RPM) or tokens per minute (TPM) limit. | Wait for the time in the `Retry-After` header, then retry. |
| 429 | `slow_down` | Your request rate increased too quickly, even if you're within your limits. | Wait for `Retry-After`, reduce your request rate, and increase it gradually. |
| 429 | `credit_balance_exhausted` | Your organization has no prepaid credits left. | Don't retry. Retrying won't restore access. Tell the user or alert an admin. |
| 503 | `server_is_overloaded` | The model doesn't have enough capacity right now. | Wait for `Retry-After` if present, then retry with increasing delays. |

For the full list, see [Error codes](https://developers.openai.com/api/docs/guides/error-codes) in the OpenAI documentation.

> [!IMPORTANT]
> All 3 errors in the 429 rows use the same status code. If your app retries every 429, it keeps retrying billing errors that never recover. Check `error.code` to decide what to do.

## Know what the OpenAI SDK does for you

The official OpenAI SDKs for Python and JavaScript retry 408, 409, 429, and 5xx responses, and connection errors, 2 times by default with exponential backoff. You can change this with the `max_retries` option in Python and `maxRetries` in JavaScript.

SDK retries cover short bursts. Your app still needs to decide what happens when the retries run out, and how to handle errors that retrying doesn't fix. In Python, a 429 raises `RateLimitError` and a 503 raises `InternalServerError`, so handle both.

> [!TIP]
> To see exactly what your error handling code receives, set `max_retries=0` (Python) or `maxRetries: 0` (JavaScript) while you test. Turn the SDK retries back on afterward.

## Simulate OpenAI rate limits

Dev Proxy has a preset with the OpenAI errors in the table. Download it:

```console
devproxy config get openai-throttling
```

Start Dev Proxy with the preset:

```console
devproxy --config-file "~dataFolder/configs/openai-throttling/.devproxy/devproxyrc.json"
```

The preset fails 90% of the requests to `https://api.openai.com/*` with a random error from the table. For throttling responses, it sets the `Retry-After` header and uses the [RetryAfterPlugin](../technical-reference/retryafterplugin.md) to check that your app waits that long before it calls the API again.

Make sure that your app sends its requests through Dev Proxy and trusts the Dev Proxy certificate. For Node.js, see [Use Dev Proxy with Node.js applications](./use-dev-proxy-with-nodejs.md). For other runtimes, see [Troubleshoot Dev Proxy](./troubleshooting.md#are-you-using-a-different-runtime-or-framework).

Run your app and check that:

- After a `rate_limit_exceeded` or `slow_down` error, your app waits for the `Retry-After` time. If it calls the API too early, Dev Proxy reports it and throttles the request.
- After a `credit_balance_exhausted` error, your app stops calling the API and shows a clear message.
- After a `server_is_overloaded` error, your app retries with a delay and, when retries run out, falls back or shows a clear message instead of a stack trace.
- Your app doesn't lose work. For example, a long chat conversation or a batch job continues after the error.

To change how often requests fail, use the `--failure-rate` option. For example, to fail every request:

```console
devproxy --config-file "~dataFolder/configs/openai-throttling/.devproxy/devproxyrc.json" --failure-rate 100
```

## Simulate token limits for Azure OpenAI and other providers

The preset returns errors at random, regardless of how many tokens your app uses. To throttle requests based on actual token use, for example to see what happens when a long conversation goes over your TPM limit, use the [LanguageModelRateLimitingPlugin](../technical-reference/languagemodelratelimitingplugin.md). It works with any OpenAI-compatible API, including Azure OpenAI and local models. For more information, see [Test language model token limits](./test-language-model-token-limits.md).

## Next step

Learn how to simulate token-based limits.

> [!div class="nextstepaction"]
> [Test language model token limits](./test-language-model-token-limits.md)

## See also

- [Test my app with language model failures](./test-my-app-with-language-model-failures.md) - Simulate unexpected language model responses
- [Simulate errors from OpenAI APIs](./simulate-errors-openai-apis.md) - Build your own OpenAI errors file
- [Test that my application handles throttling properly](./test-that-my-application-handles-throttling-properly.md) - Throttling on any API
- [Use preset configurations](./use-preset-configurations.md) - Work with presets
- [RetryAfterPlugin](../technical-reference/retryafterplugin.md) - Verify retry behavior
