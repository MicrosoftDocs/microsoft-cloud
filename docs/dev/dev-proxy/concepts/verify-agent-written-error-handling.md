---
title: How to verify error handling your coding agent wrote
description: A checklist for reviewing the retries, rate limit handling, and error messages a coding agent added to your app, and how to check them against the API's real failures before you ship.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Verify that error handling written by a coding agent works against the API's real failures -->
<!-- AUDIENCE: Developers who asked a coding agent to add retries or rate limit handling and need to decide whether to trust it -->

# How to verify error handling your coding agent wrote

Your coding agent says it added retries and rate limit handling, and its tests pass. Before you trust that, check what the tests ran against.

In a test we ran with 3 coding agents (210 runs), we asked them to make apps handle API failures and show that it works. In 76% of the 140 runs that asked for working code, the agent built the failure by hand with fetch stubs, `httpx.MockTransport`, or a throwaway HTTP server. In the 75 runs where the real API was available and the prompt didn't rule it out, 0 tested the app on its real API URL. Passing tests like these prove that the agent's code handles the failure the agent imagined. That's a different failure from the one the API sends.

## Checklist

1. **Ask what it tested against.** Ask the agent: "Which URL did the app call during your test, and what returned the error?" If the answer is a stub or a local server it wrote, treat the tests as unit tests of the branches it added.
1. **Look for switches added for tests.** Search the diff for new environment variables, base URL settings, or flags. In our test, 61 of the 105 runs on apps that call GitHub, OpenAI, or a weather API added one. Decide whether you want it in production.
1. **Check the code against the API's documented behavior.** Each API fails in its own way:
    - **GitHub**: when you exceed the primary rate limit, you get [403 or 429](https://docs.github.com/en/rest/using-the-rest-api/rate-limits-for-the-rest-api) with `x-ratelimit-remaining` set to `0`, and you wait until the time in `x-ratelimit-reset`, in UTC epoch seconds. For secondary rate limits, wait for `retry-after` if it's there, otherwise until `x-ratelimit-reset` if `x-ratelimit-remaining` is `0`, otherwise at least 1 minute. Code that only checks for 429 misses the 403s.
    - **OpenAI**: some 429s are about [billing and spend limits](https://developers.openai.com/api/docs/guides/error-codes), like `credit_balance_exhausted`. Retrying them won't help. Code that retries every 429 hides the problem from you.
    - **Anthropic**: the [spend cap 429](https://platform.claude.com/docs/en/api/rate-limits) has no `retry-after` header, and overloads return [529](https://platform.claude.com/docs/en/api/errors), which code that only knows standard status codes might not expect.
1. **Check how it reads `Retry-After`.** The header holds either a number of seconds or an HTTP date ([RFC 9110](https://www.rfc-editor.org/rfc/rfc9110#field.retry-after)). Check that the code handles the format your API sends, caps the number of attempts and the total wait, and doesn't retry on top of an SDK that already retries.
1. **Check what the user sees when retries run out.** Look for a clear message instead of a stack trace or an endless spinner, and check that the user doesn't lose their work.
1. **Run the app on its real URLs with simulated failures.** This is the only step that shows how the running app, its SDK, and its retry policy behave together.

## How to verify agent-written error handling

| Approach | What you find | What you miss |
|---|---|---|
| Read the diff | Whether the code looks right | Whether it behaves right against the API's real responses |
| Run the agent's tests | That the branches it wrote run | Everything the stub didn't model: real status codes, headers, error bodies, and SDK retries |
| Call the real API until it fails | Real behavior | You can't trigger failures on demand, and you spend real quota |
| Run the app on real URLs with simulated failures | How the running app, its SDK, and its retry policy handle the API's own errors | Your code in isolation. Keep the agent's unit tests for that. |

## Try it on your app

[Dev Proxy](../overview.md?WT.mc_id=devproxy-learn-verify-agent-written-error-handling) intercepts the requests your app sends to the real API and returns the errors you choose, with no changes to your app's code. For popular APIs, start with a [preset](../how-to/use-preset-configurations.md?WT.mc_id=devproxy-learn-verify-agent-written-error-handling). To check the GitHub rate limit handling your agent wrote:

```console
devproxy config get github-rate-limiting
devproxy --config-file "~dataFolder/configs/github-rate-limiting/.devproxy/devproxyrc.json"
```

Run your app and watch the Dev Proxy output. The preset returns a 429 with GitHub's rate limit headers once your app goes over the limit. The [RetryAfterPlugin](../technical-reference/retryafterplugin.md?WT.mc_id=devproxy-learn-verify-agent-written-error-handling) in the preset tells you if your app calls the API again before the wait time is up. The `openai-throttling` and `anthropic-throttling` presets do the same for the provider's own rate limit errors, and `openai-throttling` also returns a `credit_balance_exhausted` 429 that shouldn't be retried.

For other APIs:

- The GenericRandomErrorPlugin returns errors you define at a rate you choose. Set the rate with `--failure-rate`. See [Test my app with random errors](../how-to/test-my-app-with-random-errors.md?WT.mc_id=devproxy-learn-verify-agent-written-error-handling).
- The LatencyPlugin delays responses so you can check the timeouts the agent set. See [Simulate slow API responses](../how-to/simulate-slow-api-responses.md?WT.mc_id=devproxy-learn-verify-agent-written-error-handling).

You can also ask your coding agent to write the Dev Proxy config. Add the [Dev Proxy MCP server](../how-to/use-mcp-server.md?WT.mc_id=devproxy-learn-verify-agent-written-error-handling) to your agent so it can look up the Dev Proxy docs and best practices and check which version you have installed. Review that config like any other code the agent writes.

To install Dev Proxy, see [Set up Dev Proxy](../get-started/set-up.md?WT.mc_id=devproxy-learn-verify-agent-written-error-handling).

## Next steps

> [!div class="nextstepaction"]
> [Test my app with random errors](../how-to/test-my-app-with-random-errors.md?WT.mc_id=devproxy-learn-verify-agent-written-error-handling)

## See also

- [Mocks, stubs, fakes, and emulators: testing API calls with coding agents](mocks-stubs-fakes-coding-agents.md)
- [429 Too Many Requests: what it means and how to handle it](http-429-too-many-requests.md)
- [Test how your app handles GitHub API rate limits](../how-to/test-github-api-rate-limit-handling.md?WT.mc_id=devproxy-learn-verify-agent-written-error-handling)
- [RetryAfterPlugin](../technical-reference/retryafterplugin.md?WT.mc_id=devproxy-learn-verify-agent-written-error-handling)
- [Simulate slow API responses](../how-to/simulate-slow-api-responses.md?WT.mc_id=devproxy-learn-verify-agent-written-error-handling)
