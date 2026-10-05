---
title: "OpenAI 'Rate limit reached' errors: what they mean and how to handle them"
description: What OpenAI's rate_limit_exceeded, slow_down, and server_is_overloaded errors mean, how the OpenAI SDKs retry them, how to handle them in your app, and how to test your handling.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Understand and handle OpenAI rate limit and overload errors -->
<!-- AUDIENCE: Developers who got "Rate limit reached" from the OpenAI API and searched for it -->

# OpenAI 'Rate limit reached' errors: what they mean and how to handle them

The OpenAI API returns `429` with "Rate limit reached" when your organization sent more requests or more tokens per minute than its limits allow. Limits apply to your organization, not to each user. These errors are temporary. If you wait and send the request again, it usually succeeds. Some other `429` errors from OpenAI are about billing, and those don't go away when you wait. For more information, see [Error codes](https://developers.openai.com/api/docs/guides/error-codes).

## What OpenAI's rate limit errors look like

| Status | Error | What it means | Retry? |
|---|---|---|---|
| `429` | `rate_limit_exceeded`, requests per minute (RPM) | You sent too many requests in a minute. | Yes, after `Retry-After` |
| `429` | `rate_limit_exceeded`, tokens per minute (TPM) | Your requests used too many tokens in a minute. The message shows your limit, how many tokens you used, and how many the request asked for. | Yes, after `Retry-After`. Smaller requests help. |
| `429` | `slow_down` (type `rate_limit_error`) | Your traffic grew too quickly, even though you're within your RPM and TPM limits. | Yes, at a lower rate |
| `503` | `server_is_overloaded` (type `service_unavailable_error`) | OpenAI's servers are busy. | Yes, with longer delays each time |
| `429` | `credit_balance_exhausted`, spend limit, or usage limit errors (type `insufficient_quota`) | You're out of credits or over a limit. | No. See [OpenAI insufficient_quota and credit_balance_exhausted](openai-insufficient-quota.md). |

Most of these errors share the `429` status, so you can't tell them apart by status alone. Read `error.code` in the response body.

## How to handle OpenAI rate limit errors

1. **Check `error.code` first.** If it's a billing code like `credit_balance_exhausted`, stop retrying and tell the user. Retrying a billing error won't restore access.
1. **Follow `Retry-After` when it's present.** If it's missing, use exponential backoff with jitter, and limit the number of retries.
1. **Slow down after `slow_down`.** Reduce your request rate, then ramp it up gradually. OpenAI's rule of thumb above 1M input TPM is to increase traffic by no more than 50% every 15 minutes.
1. **Send fewer tokens after a TPM error.** Shorter prompts and responses let more requests fit in each minute.
1. **Back off further after a `503`.** Increase the delay between retries, and check the OpenAI status page.
1. **Tell the user what's happening.** "Busy, retrying in 5 seconds" beats a spinner that never ends.

The OpenAI Python SDK retries connection errors and `408`, `409`, `429`, and `5xx` responses 2 times by default, with a short exponential backoff. You can change it with `max_retries`. When the retries run out, the SDK raises `RateLimitError` for a `429` and `InternalServerError` for a `503`, so your code still needs a plan:

```python
import openai
from openai import OpenAI

client = OpenAI(max_retries=3)

BILLING_CODES = {
    "credit_balance_exhausted",
    "organization_spend_limit_exceeded",
    "project_spend_limit_exceeded",
    "organization_usage_limit_exceeded",
}


def summarize(text: str) -> str | None:
    try:
        response = client.responses.create(model="gpt-4.1", input=text)
        return response.output_text
    except openai.RateLimitError as error:
        if error.code in BILLING_CODES:
            raise  # Retrying won't help: alert and tell the user
        return None  # Still throttled after retries: show "busy, try again"
    except openai.InternalServerError:
        return None
```

## How to test that your app handles OpenAI rate limits

You rarely hit an OpenAI rate limit while you develop. You're the only user, and your prompts are short. So the way you test rate limit handling decides whether you find the bugs before your users do.

| Approach | What you find | What you miss |
|---|---|---|
| Wait for production | Real failures | Everything, until a user hits it |
| Mock the API in your tests, or let your coding agent write the mock | Whether your retry branch runs | OpenAI's real error bodies and codes, and your SDK's retry policy. Your app also needs a test-only switch to reach the mock. |
| Call the real API until it throttles you | Real behavior | You can't trigger a specific error on demand, and every request costs tokens |
| Intercept your app's real traffic and return OpenAI errors on demand | Real URLs, your real SDK and retry policy, and OpenAI's own error format | Nothing in your app changes, so it doesn't test your code in isolation. Keep your unit tests for that. |

## Try it on your app

[Dev Proxy](../overview.md?WT.mc_id=devproxy-learn-openai-rate-limit-reached) intercepts your app's requests to `api.openai.com` and returns OpenAI errors, while your app keeps calling the real URLs. The `openai-throttling` preset fails most requests with a random pick from TPM and RPM `rate_limit_exceeded`, `slow_down`, `credit_balance_exhausted`, and `503` `server_is_overloaded` errors, in OpenAI's own format. The `429` rate limit responses include a `Retry-After` header, and if your app retries before that time is up, Dev Proxy reports it.

Download the preset, and start Dev Proxy with it:

```console
devproxy config get openai-throttling
devproxy --config-file "~dataFolder/configs/openai-throttling/.devproxy/devproxyrc.json"
```

Then run your app as usual and watch what it does. To install Dev Proxy, see [Set up Dev Proxy](../get-started/set-up.md?WT.mc_id=devproxy-learn-openai-rate-limit-reached).

To test how your app behaves when it runs out of tokens per minute, based on the prompt and completion tokens your requests use, see [Test language model token limits](../how-to/test-language-model-token-limits.md?WT.mc_id=devproxy-learn-openai-rate-limit-reached).

## Next steps

> [!div class="nextstepaction"]
> [Test how your app handles OpenAI rate limits](../how-to/test-openai-rate-limit-handling.md?WT.mc_id=devproxy-learn-openai-rate-limit-reached)

## See also

- [OpenAI insufficient_quota and credit_balance_exhausted: why retrying won't help](openai-insufficient-quota.md)
- [429 Too Many Requests: what it means and how to handle it](http-429-too-many-requests.md)
- [The Retry-After header: how long to wait before you retry](retry-after-header.md)
- [Simulate errors from OpenAI APIs](../how-to/simulate-errors-openai-apis.md?WT.mc_id=devproxy-learn-openai-rate-limit-reached)
- [LanguageModelRateLimitingPlugin](../technical-reference/languagemodelratelimitingplugin.md?WT.mc_id=devproxy-learn-openai-rate-limit-reached)
