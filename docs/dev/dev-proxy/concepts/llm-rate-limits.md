---
title: "LLM rate limits: tokens per minute, requests per minute, and what happens when you hit them"
description: How OpenAI, Azure OpenAI, and Anthropic limit tokens and requests per minute, which 429 and overload errors you get when you hit the limits, how to handle them, and how to test your handling.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Understand LLM rate limits (TPM, RPM) and how to handle and test them -->
<!-- AUDIENCE: Developers building apps on OpenAI, Azure OpenAI, or Anthropic who got a 429 or want to avoid one -->

# LLM rate limits: tokens per minute, requests per minute, and what happens when you hit them

Language model APIs limit your traffic in 2 ways at once: how many requests you send per minute (RPM) and how many tokens you use per minute (TPM). You can stay well under your request limit and still get throttled because a few long prompts used up your token budget. When you hit either limit, the API answers with [`429 Too Many Requests`](http-429-too-many-requests.md) and your app has to wait.

## How OpenAI, Azure OpenAI, and Anthropic count

| Provider | What's limited | What to know |
|---|---|---|
| [OpenAI](https://developers.openai.com/api/docs/guides/rate-limits) | RPM, requests per day, TPM, tokens per day, and more, per organization and project, per model | You hit whichever limit runs out first. For the token limit, a request counts as the higher of `max_tokens` and an estimate based on its characters. Failed requests count too. |
| [Azure OpenAI](/azure/foundry/openai/how-to/quota) | TPM that you assign to each deployment, plus an RPM limit set in proportion to it | RPM is checked over windows of 1 or 10 seconds, so a burst gets a 429 even when the per-minute total is fine. The token estimate includes `max_tokens`. |
| [Anthropic](https://platform.claude.com/docs/en/api/rate-limits) | RPM, input tokens per minute (ITPM), and output tokens per minute (OTPM), per model | Capacity refills continuously, and 60 RPM might be enforced as 1 request per second. For most models, cached input tokens don't count toward ITPM, and `max_tokens` doesn't count toward OTPM. |

If you set `max_tokens` to 4,000 and get 200 tokens back, OpenAI and Azure OpenAI still count the 4,000 against your limit. That's how you can get 429s while your usage metrics look well below your quota.

## What a 429 means for each provider

Not every 429 goes away if you wait.

| Provider | Wait and retry | Stop and tell someone |
|---|---|---|
| [OpenAI](https://developers.openai.com/api/docs/guides/error-codes) | 429 for requests or tokens, 429 `slow_down` (your traffic grew too fast, even within your limits), and 503 `server_is_overloaded`. Wait for `Retry-After` when it's present. | 429 with `credit_balance_exhausted`, `organization_spend_limit_exceeded`, `project_spend_limit_exceeded`, or `organization_usage_limit_exceeded` in `error.code`. Retrying won't restore access. |
| [Azure OpenAI](/azure/foundry/openai/how-to/quota) | 429 for the deployment's TPM or RPM, for system capacity, or for a temporary reduction of your rate limit. Wait for `retry-after-ms`. | Sustained 429s in production while you're below your approved quota. Check the deployment's TPM allocation, then open a support request. |
| [Anthropic](https://platform.claude.com/docs/en/api/errors) | 429 `rate_limit_error` with a `retry-after` header, including acceleration limits after a sharp increase in usage, and 529 `overloaded_error`. | 429 for the monthly spend cap. It has no `retry-after` header, `error.details.error_code` is `enforced_spend_limit_reached`, and it keeps failing until access resumes. |

## How to handle LLM rate limits

1. **Check which 429 you got.** Billing, spend, and quota errors need a person, not a retry.
1. **Wait as long as the API asks.** OpenAI and Anthropic send `retry-after` in seconds. Azure OpenAI sends `retry-after-ms` in milliseconds. Without a hint, back off exponentially with random jitter, and cap both the number of attempts and the total time.
1. **Know what your SDK already does.** The [OpenAI](https://github.com/openai/openai-python#retries) and [Anthropic](https://github.com/anthropics/anthropic-sdk-python#retries) Python SDKs retry connection errors and `408`, `409`, `429`, and 5xx responses 2 times by default. If you add your own retry loop, turn off the SDK's retries, for example with `max_retries=0` in Python, as [Azure OpenAI recommends](/azure/foundry/openai/how-to/quota), or the attempts multiply.
1. **Shrink what counts.** On OpenAI and Azure OpenAI, set `max_tokens` close to the response size you expect. On Anthropic, cache repeated content like system instructions.
1. **Ramp up gradually.** A sharp jump in traffic triggers OpenAI's `slow_down` and Anthropic's acceleration limits, even within your limits. OpenAI suggests that once you reach 1 million input tokens per minute, you grow by no more than 50% every 15 minutes.
1. **Don't replay a stream you already consumed.** An error after a stream starts can arrive as a stream event, and [OpenAI advises](https://developers.openai.com/api/docs/guides/rate-limits) against automatically replaying a request after you consumed output.
1. **Tell the user** when a request stalls on a 429, instead of showing a spinner.

## How to test that your app handles LLM rate limits

| Approach | What you find | What you miss |
|---|---|---|
| Mock the SDK client in your tests, or let your coding agent write the mock | Whether your error branch runs | The provider's real status codes, error codes, and headers, and your SDK's own retries. Your app also needs a test-only switch to reach the mock. |
| Call the real API until it throttles you | Real behavior | You pay for every token, and you can't trigger `slow_down`, an overload, or a spend cap on demand |
| Intercept your app's real traffic and return the provider's errors, or throttle on the tokens your app uses | Real URLs, your SDK's retry policy, and the provider's error bodies, with a limit you choose | Your code in isolation. Keep your unit tests for that. |

## Try it on your app

[Dev Proxy](../overview.md?WT.mc_id=devproxy-learn-llm-rate-limits) intercepts your app's requests to the language model API and returns the provider's own errors, while your app keeps calling the real URL. For OpenAI, download the preset and start Dev Proxy with it:

```console
devproxy config get openai-throttling
devproxy --config-file "~dataFolder/configs/openai-throttling/.devproxy/devproxyrc.json"
```

The `openai-throttling` preset fails 90% of requests to `https://api.openai.com/*` with a random error: `rate_limit_exceeded` for TPM or RPM, `slow_down`, `credit_balance_exhausted`, or 503 `server_is_overloaded`. The `anthropic-throttling` preset does the same for `https://api.anthropic.com/*` with RPM, input token, output token, and acceleration 429s, and 529 `overloaded_error`. Both presets include the [RetryAfterPlugin](../technical-reference/retryafterplugin.md?WT.mc_id=devproxy-learn-llm-rate-limits), which tells you when your app calls the API again before the `retry-after` time on a 429 is up. It doesn't check the 503 and 529 responses.

To throttle on the tokens your app actually uses, use the [LanguageModelRateLimitingPlugin](../technical-reference/languagemodelratelimitingplugin.md?WT.mc_id=devproxy-learn-llm-rate-limits). It counts the prompt and completion tokens that each response reports and returns 429 with `retry-after` when your app goes over the limits you set. It works with OpenAI-compatible APIs, including Azure OpenAI and local models:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/rc.schema.json",
  "plugins": [
    {
      "name": "LanguageModelRateLimitingPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "languageModelRateLimitingPlugin"
    }
  ],
  "urlsToWatch": [
    "https://api.openai.com/*",
    "https://*.openai.azure.com/openai/deployments/*/chat/completions*"
  ],
  "languageModelRateLimitingPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/languagemodelratelimitingplugin.schema.json",
    "promptTokenLimit": 1000,
    "completionTokenLimit": 500,
    "resetTimeWindowSeconds": 60
  }
}
```

The plugin doesn't reproduce the `max_tokens` estimate. Its default 429 body uses the `insufficient_quota` code. To return the error body your app expects for a rate limit, set `whenLimitExceeded` to `Custom` and point `customResponseFile` to your own response. To install Dev Proxy, see [Set up Dev Proxy](../get-started/set-up.md?WT.mc_id=devproxy-learn-llm-rate-limits).

## Next steps

> [!div class="nextstepaction"]
> [Test how your app handles OpenAI rate limits](../how-to/test-openai-rate-limit-handling.md?WT.mc_id=devproxy-learn-llm-rate-limits)

## See also

- [429 Too Many Requests: what it means and how to handle it](http-429-too-many-requests.md)
- [What is rate limiting?](what-is-rate-limiting.md)
- [Test language model token limits](../how-to/test-language-model-token-limits.md?WT.mc_id=devproxy-learn-llm-rate-limits)
- [LanguageModelRateLimitingPlugin](../technical-reference/languagemodelratelimitingplugin.md?WT.mc_id=devproxy-learn-llm-rate-limits)
- [How to test what your AI agent does when its tools fail](test-ai-agent-tool-failures.md)
