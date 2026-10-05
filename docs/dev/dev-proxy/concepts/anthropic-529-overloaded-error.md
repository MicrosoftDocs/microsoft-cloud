---
title: "Anthropic 529 overloaded_error: what it means and how to handle it"
description: What the Claude API 529 overloaded_error means, how it differs from a 429 rate_limit_error and a spend-cap 429, how to handle each one, and how to test your handling.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Understand and handle the Anthropic 529 overloaded_error -->
<!-- AUDIENCE: Developers whose app calls the Claude API and got a 529 -->

# Anthropic 529 overloaded_error: what it means and how to handle it

The Claude API returns `529` with the error type `overloaded_error` when the API is temporarily overloaded. According to Anthropic, it can happen when the API experiences high traffic across all users. Your request is fine. The API is busy, so it turned the request down. When your organization goes over its own rate limits, you get a 429 instead. The response body has the same shape as every other Claude API error: a top-level `type` of `error`, an `error` object with `type` and `message`, and a `request_id` you can give to Anthropic support. For more information, see [Claude API errors](https://platform.claude.com/docs/en/api/errors).

## 529, 429, or spend cap: how to tell them apart

The Claude API uses similar-looking errors for very different problems. Some go away if you wait. One doesn't go away until next month.

| Response | `error.type` | `retry-after` | What it means | What to do |
|---|---|---|---|---|
| `529` | `overloaded_error` | Use it if it's there | The API is overloaded across all users | Back off and retry a few times |
| `429` | `rate_limit_error` | Yes | Your organization went over its requests, input tokens, or output tokens per minute, or ramped up too fast and hit an acceleration limit | Wait as long as `retry-after` says |
| `429` | `rate_limit_error`, with `error.details.error_code` set to `enforced_spend_limit_reached` | No | Your organization reached its usage tier's monthly spend cap | Don't retry. Usage pauses until 00:00 UTC on the first day of the next month, or until you move to a higher tier. |
| `400` | `invalid_request_error` | No | Usage reached a spend limit that you set on your organization or workspace | Raise or remove the limit |

A spend-cap 429 has the same error type as a rate limit, so code that retries every `rate_limit_error` keeps failing. Anthropic notes that retries fail until access resumes, including the SDK's automatic retries. For details, see [Reaching your spend cap](https://platform.claude.com/docs/en/api/rate-limits#reaching-your-spend-cap).

## How to handle a 529

1. **Check the status code before you retry.** A `529` and a `429` need different waits, and a `429` without `retry-after` needs no retry at all.
1. **Back off on a 529.** Retry with exponential backoff and random jitter, and stop after a few attempts. If the response has a `retry-after` header, wait that long instead.
1. **Let the SDK do the first retries.** The [official Anthropic SDKs](https://github.com/anthropics/anthropic-sdk-python#retries) retry connection errors, rate limits, and 5xx errors twice by default, with exponential backoff, and honor `retry-after` when it's present. You can change the count with `max_retries` (`maxRetries` in TypeScript). When the SDK runs out of retries, your code gets the error.
1. **Stop retrying on a spend cap.** If a 429 has no `retry-after` header, tell the user and alert yourself.
1. **Keep the user informed.** Queue the work and try again later, or show a clear "busy, try again in a minute" message instead of a generic error.

In the Python SDK, a 429 raises `anthropic.RateLimitError` and any status of 500 or above, including 529, raises `anthropic.InternalServerError`:

```python
import anthropic

client = anthropic.Anthropic(max_retries=4)


def summarize(text: str) -> str | None:
    try:
        message = client.messages.create(
            model="claude-sonnet-5",
            max_tokens=1024,
            messages=[{"role": "user", "content": f"Summarize:\n\n{text}"}],
        )
    except anthropic.RateLimitError as e:
        if "retry-after" not in e.response.headers:
            # Spend cap: every retry fails until access resumes
            alert_admin(e)
            return None
        raise
    except anthropic.InternalServerError as e:
        if e.status_code == 529:
            # Overloaded after all SDK retries: queue the job for later
            queue_for_later(text)
            return None
        raise
    return next(block.text for block in message.content if block.type == "text")
```

## How to test that your app handles a 529

You rarely see a 529 while you develop. It depends on traffic from every Claude API user, so you can't trigger it. The way you test decides whether you find the bugs before your users do.

| Approach | What you find | What you miss |
|---|---|---|
| Wait for production | Real overloads | Everything, until a user hits it |
| Mock the API in your tests, or let your coding agent write the mock | Whether your error branch runs | Anthropic's real status codes and error bodies, and your SDK's retry policy. Your app also needs a test-only switch to reach the mock. |
| Call the real API until it fails | Real behavior | You can't trigger a 529 on demand, and you can't safely trigger a spend cap at all |
| Intercept your app's real traffic and return 529s and 429s on demand | Real URLs, your real SDK and retry policy, and Anthropic's own error format | Nothing in your app changes, so it doesn't test your code in isolation. Keep your unit tests for that. |

## Try it on your app

[Dev Proxy](../overview.md?WT.mc_id=devproxy-learn-anthropic-529-overloaded-error) intercepts your app's requests to `https://api.anthropic.com` and returns errors in Anthropic's error format, while your app keeps calling the real URL. Download a preset, and start Dev Proxy with it:

```console
devproxy config get anthropic-throttling
devproxy --config-file "~dataFolder/configs/anthropic-throttling/.devproxy/devproxyrc.json"
```

| Preset | What it returns |
|---|---|
| `anthropic-throttling` | At random, 1 of 4 429 `rate_limit_error` responses (requests, input tokens, output tokens, and acceleration limit) or a 529 `overloaded_error`. On the 429s, Dev Proxy sets `retry-after` and tells you when your app calls the API again too early. |
| `anthropic-random-errors` | At random, one of the errors from the Claude API error list, including 400, 401, 402, 403, 404, 409, 413, 429, 500, 504, and 529, for 50% of requests |

Neither preset includes a spend-cap 429. To test that path, add a response without a `retry-after` header to the preset's `anthropic-errors.json` file:

```json
{
  "statusCode": 429,
  "headers": [
    { "name": "content-type", "value": "application/json" }
  ],
  "body": {
    "type": "error",
    "error": {
      "type": "rate_limit_error",
      "message": "You have reached your API usage limits.",
      "details": { "error_code": "enforced_spend_limit_reached" }
    }
  }
}
```

To make every request fail, so that you see what happens when the SDK runs out of retries, start Dev Proxy with `--failure-rate 100`. For more information, see [Change request failure rate](../how-to/change-request-failure-rate.md?WT.mc_id=devproxy-learn-anthropic-529-overloaded-error). To install Dev Proxy, see [Set up Dev Proxy](../get-started/set-up.md?WT.mc_id=devproxy-learn-anthropic-529-overloaded-error).

## Next steps

> [!div class="nextstepaction"]
> [Test my app with random errors](../how-to/test-my-app-with-random-errors.md?WT.mc_id=devproxy-learn-anthropic-529-overloaded-error)

## See also

- [429 Too Many Requests: what it means and how to handle it](http-429-too-many-requests.md)
- [500, 502, 503, and 504 errors from APIs: what they mean and how to handle them](http-5xx-api-errors.md)
- [What is throttling?](what-is-throttling.md)
- [Test that my application handles throttling properly](../how-to/test-that-my-application-handles-throttling-properly.md?WT.mc_id=devproxy-learn-anthropic-529-overloaded-error)
