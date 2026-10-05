---
title: "Azure OpenAI 429: token and request rate limits explained"
description: Why Azure OpenAI returns 429 Too Many Requests, how tokens per minute (TPM) and requests per minute (RPM) limits work per deployment, why you can get a 429 below your quota, and how to handle and test it.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Understand and handle Azure OpenAI 429 token and request rate limits -->
<!-- AUDIENCE: Developers whose app calls Azure OpenAI and got a 429 -->

# Azure OpenAI 429: token and request rate limits explained

Azure OpenAI returns `429 Too Many Requests` when a request goes over a rate limit on your deployment, or when the service can't process your request at this time. Each deployment has 2 limits: tokens per minute (TPM) and requests per minute (RPM). Going over either one gets you a 429. For more information, see [Manage Azure OpenAI quota](/azure/foundry/openai/how-to/quota).

## How quota becomes a rate limit

Quota is assigned to your subscription per region, per model, and per deployment type, in TPM. When you create a deployment, you assign it part of that quota, and the TPM you assign becomes the deployment's tokens-per-minute limit. The RPM limit is set in proportion to the TPM. The ratio varies by model. For older chat models, 1 unit of capacity is 1,000 TPM and 6 RPM.

For example, with 240,000 TPM of quota for a model in a region, you can create 1 deployment with 240,000 TPM or 2 with 120,000 TPM each. So you can have approved quota at the subscription level and still get 429s, because the quota isn't assigned to the deployment that gets the traffic. Quota management is also moving to the subscription level for some deployment types. For the current limits, see [Azure OpenAI quotas and limits](/azure/foundry/openai/quotas-limits).

## Why you get a 429 below your quota

Your token usage metrics can look well below quota while your app gets 429s. Azure OpenAI rate limits requests when it receives them, using estimates, and the metrics show billed tokens from successful requests.

- **`max_tokens` counts in full.** The TPM check uses an estimate that includes the prompt, the `max_tokens` setting, and `best_of`. A request with `max_tokens` set to 4,000 uses up 4,000 tokens of your limit, even if the answer is 200 tokens long.
- **RPM is checked over 1 or 10 seconds.** A 600-RPM deployment can throttle you when you send more than 10 requests in 1 second, even if you stay under 600 in that minute.
- **Rejected requests count.** A request that fails with a 400 can still count toward your rate limit.
- **The service can lower your limit for a while.** When Standard deployments are under heavy demand, the service can temporarily lower a deployment's effective limit. You see it when `x-ratelimit-limit-tokens` is lower than the TPM you assigned.

## What a 429 tells you

The error message tells you which kind of 429 you have. Messages like "Requests to … have been limited" or "Rate limit is exceeded" mean you went over your deployment's TPM or RPM. Messages like "The service is temporarily unable to process your request" or "System is experiencing high demand" mean the service is short on capacity, which is often temporary. Requesting more quota won't fix a capacity 429.

Azure OpenAI also sends rate limit headers:

| Header | What it tells you |
|---|---|
| `x-ratelimit-limit-tokens`, `x-ratelimit-limit-requests` | The deployment's current limits |
| `x-ratelimit-remaining-tokens`, `x-ratelimit-remaining-requests` | What's left before you get a 429 |
| `x-ratelimit-reset-tokens`, `x-ratelimit-reset-requests` | Time until each limit resets |
| `retry-after-ms` | Sent with a 429. How many milliseconds to wait before you retry. |

## How to handle an Azure OpenAI 429

1. **Read the message.** Tell a quota 429 from a capacity 429 before you decide what to change.
1. **Wait as long as the service asks.** Use `retry-after-ms` if it's there. Otherwise, retry with exponential backoff and random jitter, and stop after a few attempts. Failed requests count toward your limit too, so retrying without waiting keeps you throttled.
1. **Set `max_tokens` to what you need.** The lower it is, the more requests fit in your TPM.
1. **Spread your requests out.** Avoid bursts, ramp up new workloads gradually, and slow down when the `remaining` headers get low.
1. **Retry in one place.** The OpenAI Python SDK retries twice by default. If you write your own retry loop, set `max_retries=0` on the client so that each of your attempts doesn't turn into 3 requests.

```python
import os
import random
import time

from openai import AzureOpenAI, RateLimitError

client = AzureOpenAI(
    azure_endpoint="https://<your-resource>.openai.azure.com/",
    api_key=os.environ["AZURE_OPENAI_API_KEY"],
    api_version="2024-10-21",
    max_retries=0,
)


def wait_seconds(error: RateLimitError, attempt: int) -> float:
    try:
        return float(error.response.headers["retry-after-ms"]) / 1000
    except (KeyError, ValueError):
        return 2 ** attempt + random.random()


def chat(messages, attempts=4):
    for attempt in range(1, attempts + 1):
        try:
            return client.chat.completions.create(
                model="gpt-4o",  # your deployment name
                messages=messages,
                max_tokens=300,
            )
        except RateLimitError as error:
            if attempt == attempts:
                raise
            time.sleep(wait_seconds(error, attempt))
```

## How to test that your app handles a 429

You rarely see a 429 while you develop. You're the only user, and your prompts are short. So the way you test decides whether you find the bugs before your users do.

| Approach | What you find | What you miss |
|---|---|---|
| Wait for production | Real 429s | Everything, until a user hits it |
| Mock the API in your tests, or let your coding agent write the mock | Whether your retry branch runs | Real token counts, the service's headers and error bodies, and your SDK's retry policy. Your app also needs a test-only switch to reach the mock. |
| Lower the TPM on a test deployment and call it until it throttles you | Real behavior | You pay for every token, and you can't control when the limit hits |
| Intercept your app's real traffic and enforce a token limit you choose | Your real prompts and token usage, and your real SDK and retry policy | Nothing in your app changes, so it doesn't test your code in isolation. Keep your unit tests for that. |

## Try it on your app

[Dev Proxy](../overview.md?WT.mc_id=devproxy-learn-azure-openai-429-token-rate-limit) can enforce a token limit on your app's real Azure OpenAI calls with the [LanguageModelRateLimitingPlugin](../technical-reference/languagemodelratelimitingplugin.md?WT.mc_id=devproxy-learn-azure-openai-429-token-rate-limit). The plugin works with OpenAI-compatible APIs. It reads `prompt_tokens` and `completion_tokens` from each response, and when your app goes over either limit, it returns a 429 with a `retry-after` header in seconds until the time window resets.

**File:** devproxyrc.json

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
    "https://*.openai.azure.com/openai/deployments/*/completions*"
  ],
  "languageModelRateLimitingPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/languagemodelratelimitingplugin.schema.json",
    "promptTokenLimit": 1000,
    "completionTokenLimit": 500,
    "resetTimeWindowSeconds": 60,
    "whenLimitExceeded": "Throttle"
  }
}
```

Start Dev Proxy with this file and use your app as usual. Keep the differences in mind: the plugin counts the tokens that each response reports, so it doesn't reproduce the `max_tokens` estimate, and its default 429 body uses the `insufficient_quota` code, which your app might treat as a billing error instead of a rate limit. To return your own body, set `whenLimitExceeded` to `Custom` and use a custom response file. To simulate a requests-per-minute limit instead, see [Simulate rate limit API responses](../how-to/simulate-rate-limit-api-responses.md?WT.mc_id=devproxy-learn-azure-openai-429-token-rate-limit). To install Dev Proxy, see [Set up Dev Proxy](../get-started/set-up.md?WT.mc_id=devproxy-learn-azure-openai-429-token-rate-limit).

## Next steps

> [!div class="nextstepaction"]
> [Test language model token limits](../how-to/test-language-model-token-limits.md?WT.mc_id=devproxy-learn-azure-openai-429-token-rate-limit)

## See also

- [429 Too Many Requests: what it means and how to handle it](http-429-too-many-requests.md)
- [What is rate limiting?](what-is-rate-limiting.md)
- [Simulate Azure OpenAI API](../how-to/simulate-azure-openai.md?WT.mc_id=devproxy-learn-azure-openai-429-token-rate-limit)
- [Test how your app handles OpenAI rate limits](../how-to/test-openai-rate-limit-handling.md?WT.mc_id=devproxy-learn-azure-openai-429-token-rate-limit)
