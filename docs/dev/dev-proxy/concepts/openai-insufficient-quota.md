---
title: "OpenAI insufficient_quota and credit_balance_exhausted: why retrying won't help"
description: What OpenAI's credit_balance_exhausted, spend limit, and usage limit errors mean, why they come back as 429 with type insufficient_quota, why retries don't fix them, and how to test that your app stops and tells the user.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Understand and handle OpenAI billing and quota errors that retries can't fix -->
<!-- AUDIENCE: Developers who got insufficient_quota or credit_balance_exhausted from the OpenAI API and searched for it -->

# OpenAI insufficient_quota and credit_balance_exhausted: why retrying won't help

The OpenAI API returns `429` with the error type `insufficient_quota` when your account ran out of credits or went over a spend or usage limit. It's the same status code as a rate limit, but waiting a few seconds won't fix it. Access comes back only after someone adds credits, raises a limit, or the monthly period resets. OpenAI says that retrying billing, spend, or quota errors won't restore API access, and that you should inspect `error.code` to find the specific cause. For more information, see [Error codes](https://developers.openai.com/api/docs/guides/error-codes).

## What OpenAI's quota errors look like

Each of these errors returns `429`. The `error.type` can still be `insufficient_quota`, so check `error.code` to know which one you got.

| `error.code` | What it means | How access comes back |
|---|---|---|
| `credit_balance_exhausted` | Your organization has no prepaid credits left. | Add credits. |
| `organization_spend_limit_exceeded` | Your organization reached its monthly spend limit across all projects. | Raise or remove the limit, or wait for the monthly reset. |
| `project_spend_limit_exceeded` | The project reached its monthly spend limit. Other projects keep working. | Raise or remove the project's limit, or wait for the monthly reset. |
| `organization_usage_limit_exceeded` | Your organization reached the monthly usage limit that OpenAI assigned to it. It's separate from the spend limits you set. | Request a higher approved limit, or contact OpenAI support. |

Compare these with "Rate limit reached" errors, like `rate_limit_exceeded` and `slow_down`. Those are temporary, and a retry after a short wait usually works. For more information, see [OpenAI 'Rate limit reached' errors](openai-rate-limit-reached.md).

## How to handle OpenAI quota errors

1. **Read `error.code`, not only the status.** A `429` alone doesn't tell you whether to retry. Treat the 4 codes in the table as "stop", and the rate limit codes as "wait and retry".
1. **Stop retrying.** Don't send the request again, and don't let a retry loop keep calling the API. Until someone fixes the billing problem, every request fails the same way.
1. **Pause the calls that would fail.** One quota error means the next requests from the same organization or project fail too. Skip them instead of sending each one and waiting for the error.
1. **Tell the user.** Explain that the AI feature is unavailable for now, and keep the rest of your app working.
1. **Alert yourself.** Log the code at a high severity, or page whoever owns billing. The fix is outside your code, so someone needs to know.

The [OpenAI Python SDK](https://github.com/openai/openai-python#retries) retries `429` responses 2 times by default. Whatever it retries, your code eventually gets `RateLimitError` with the billing code, and that's where you stop:

```python
import logging

import openai
from openai import OpenAI

client = OpenAI()
logger = logging.getLogger(__name__)

BILLING_CODES = {
    "credit_balance_exhausted",
    "organization_spend_limit_exceeded",
    "project_spend_limit_exceeded",
    "organization_usage_limit_exceeded",
}
billing_error: str | None = None


def ask(prompt: str) -> str:
    global billing_error
    if billing_error:
        raise RuntimeError("AI features are paused until billing is fixed.")
    try:
        response = client.responses.create(model="gpt-4.1", input=prompt)
        return response.output_text
    except openai.RateLimitError as error:
        if error.code in BILLING_CODES:
            billing_error = error.code
            logger.critical("OpenAI billing error: %s", error.code)
        raise
```

The flag stays set until your app restarts. Clear it another way if your app runs for a long time, for example with an admin action after someone fixes billing.

## How to test that your app handles OpenAI quota errors

You rarely see a quota error while you develop. Your test account has credits, and your usage is low. So the way you test quota handling decides whether you find the bugs before your users do.

| Approach | What you find | What you miss |
|---|---|---|
| Wait for production | Real failures | Everything, until a user hits it, and the AI feature stays down until someone notices |
| Mock the API in your tests, or let your coding agent write the mock | Whether your stop branch runs | OpenAI's real error body and codes, and your SDK's retry policy. Your app also needs a test-only switch to reach the mock. |
| Use up your real credits or set a tiny spend limit | Real behavior | It costs money, and it blocks every other app that shares the organization or project |
| Intercept your app's real traffic and return quota errors on demand | Real URLs, your real SDK and retry policy, and OpenAI's own error format | Nothing in your app changes, so it doesn't test your code in isolation. Keep your unit tests for that. |

## Try it on your app

[Dev Proxy](../overview.md?WT.mc_id=devproxy-learn-openai-insufficient-quota) intercepts your app's requests to `api.openai.com` and returns OpenAI errors, while your app keeps calling the real URLs. The `openai-throttling` preset mixes a `credit_balance_exhausted` error in with the rate limit errors. It returns `429` with type `insufficient_quota` and no `Retry-After` header, so you can check that your app stops instead of retrying.

Download the preset, and start Dev Proxy with it:

```console
devproxy config get openai-throttling
devproxy --config-file "~dataFolder/configs/openai-throttling/.devproxy/devproxyrc.json"
```

To test only quota errors, edit the preset's `openai-errors.json` file and keep only the `credit_balance_exhausted` response. To test the spend and usage limit codes, add responses with the same shape and a different `code`.

Then run your app as usual and watch what it does. To install Dev Proxy, see [Set up Dev Proxy](../get-started/set-up.md?WT.mc_id=devproxy-learn-openai-insufficient-quota).

## Next steps

> [!div class="nextstepaction"]
> [Test how your app handles OpenAI rate limits](../how-to/test-openai-rate-limit-handling.md?WT.mc_id=devproxy-learn-openai-insufficient-quota)

## See also

- [OpenAI 'Rate limit reached' errors: what they mean and how to handle them](openai-rate-limit-reached.md)
- [429 Too Many Requests: what it means and how to handle it](http-429-too-many-requests.md)
- [Simulate errors from OpenAI APIs](../how-to/simulate-errors-openai-apis.md?WT.mc_id=devproxy-learn-openai-insufficient-quota)
- [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md?WT.mc_id=devproxy-learn-openai-insufficient-quota)
