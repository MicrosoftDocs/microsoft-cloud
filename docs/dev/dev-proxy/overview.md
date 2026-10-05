---
title: What is Dev Proxy?
description: Free, open-source API simulator. Test how your app handles errors, rate limits, and slow responses from APIs like GitHub and OpenAI without changing your code.
author: garrytrinder
ms.author: garrytrinder
ms.date: 10/03/2026
ms.topic: overview
zone_pivot_groups: client-operating-system
---

<!-- INTENT: Help users understand what Dev Proxy is and whether it's right for them -->
<!-- AUDIENCE: Developers evaluating API testing tools -->

# What is Dev Proxy?

Dev Proxy is an API simulator that helps you effortlessly test your app beyond the happy path.

> [!VIDEO https://www.youtube.com/embed/HVTJlGSxhcw]

You test your app to make sure it works as intended. But what if the APIs you use fail? Will your app lose your customer's data? How do you test for this? Simulating API failures is hard. You end up writing code that you won't be shipping or worse: not testing at all. That's why we built Dev Proxy, to simulate API errors so that you can easily test your app without changing your code.

With Dev Proxy you:

- [See how your app responds to API errors](./how-to/test-my-app-with-random-errors.md), without changing your app's code, so that you can **build more robust apps and don't lose customers' data**.
- [Verify how your app handles API rate limits](./how-to/simulate-rate-limit-api-responses.md), so that you can avoid getting throttled and **improve the user experience for your customers**.
- [See how your app handles slow APIs](./how-to/simulate-slow-api-responses.md), so that you can implement the necessary affordances, and **make your app more user-friendly**.
- [Quickly stand-up mock APIs](./how-to/simulate-crud-api.md) without writing a line of code, so that you can **focus on building your app instead of writing code you won't be shipping**.
- Improve your app with contextual guidance on how you use APIs, to **make your app even better**.

Dev Proxy is a command-line tool that works on any platform. Because it intercepts network requests, it works with any type of app and tech stack. Dev Proxy is open source and free to use.

## Who is Dev Proxy for?

Dev Proxy helps developers who:

- **Build apps that call APIs** - Test how your app handles errors and rate limits from APIs like GitHub, OpenAI, and Anthropic, without changing your code
- **Build apps with Microsoft Graph** - Get guidance on permissions and best practices
- **Design APIs** - Prototype and mock APIs before implementation
- **Automate testing** - Integrate chaos testing into CI/CD pipelines

## When to use Dev Proxy

**Use Dev Proxy when you need to:**

- Test API resilience without modifying your application code
- Work with any tech stack (browser, Node.js, .NET, Python, etc.)
- Simulate failures for APIs you don't control
- Get guidance on Microsoft Graph best practices
- Automate chaos testing in CI/CD pipelines

**Consider other approaches when:**

- You only need in-browser mocking for frontend unit tests
- You're building the API and need contract testing
- You need to modify request/response bodies programmatically (Dev Proxy can do this, but dedicated tools may be simpler)

## Quick start by scenario

Choose your path based on what you want to accomplish:

| What do you want to do? | Time | Guide |
|-------------------------|------|-------|
| Test my app handles API errors | 5 min | [Test with random errors](./how-to/test-my-app-with-random-errors.md) |
| Test my app handles GitHub API rate limits | 15 min | [Test GitHub API rate limits](./how-to/test-github-api-rate-limit-handling.md) |
| Test my app handles OpenAI rate limits | 10 min | [Test OpenAI rate limits](./how-to/test-openai-rate-limit-handling.md) |
| Mock an API that doesn't exist yet | 10 min | [Simulate a CRUD API](./how-to/simulate-crud-api.md) |
| Check my Microsoft Graph permissions | 10 min | [Detect minimal permissions](./how-to/detect-minimal-microsoft-graph-api-permissions.md) |
| Understand what APIs my app calls | 5 min | [Discover URLs to watch](./how-to/discover-urls-watch.md) |
| Automate API testing in CI/CD | 15 min | [Use Dev Proxy in CI/CD](./how-to/use-dev-proxy-in-ci-cd-overview.md) |

## Fix an API problem

Got an error from an API, or building an AI app or agent? Start from the problem:

- **Errors and limits:** [429 Too Many Requests](./concepts/http-429-too-many-requests.md), [Retry-After](./concepts/retry-after-header.md), [500, 502, 503, and 504 errors](./concepts/http-5xx-api-errors.md), [GitHub API rate limit exceeded](./concepts/github-api-rate-limit-exceeded.md), [OpenAI rate limit reached](./concepts/openai-rate-limit-reached.md), [Anthropic 529 overloaded_error](./concepts/anthropic-529-overloaded-error.md), [.NET HttpClient timeouts](./concepts/dotnet-httpclient-timeouts.md)
- **AI apps and agents:** [LLM rate limits](./concepts/llm-rate-limits.md), [Verify error handling your coding agent wrote](./concepts/verify-agent-written-error-handling.md), [Test what your AI agent does when tools fail](./concepts/test-ai-agent-tool-failures.md), [Test your AI agent for prompt injection in tool responses](./concepts/test-ai-agent-prompt-injection-tool-responses.md)

## Try it with a preset

Presets are ready-made Dev Proxy configurations for specific APIs. To see how your app handles the rate limits and errors of the API it calls, start by installing Dev Proxy.

::: zone pivot="client-operating-system-windows"

```console
winget install DevProxy.DevProxy --silent
```

After the installation finishes, open a new command prompt so that it picks up the updated PATH.

::: zone-end

::: zone pivot="client-operating-system-macos"

```console
brew tap dotnet/dev-proxy
brew install dev-proxy
```

::: zone-end

::: zone pivot="client-operating-system-linux"

```console
bash -c "$(curl -sL https://aka.ms/devproxy/setup.sh)"
```

::: zone-end

Next, download the preset for your API.

| API | Command |
|-----|---------|
| GitHub | `devproxy config get github-rate-limiting` |
| OpenAI | `devproxy config get openai-throttling` |
| Anthropic | `devproxy config get anthropic-throttling` |
| Microsoft Graph | `devproxy config get microsoft-graph-rate-limiting` |

Start Dev Proxy with the preset. Replace `github-rate-limiting` with the preset that you downloaded. The first time you start Dev Proxy, you need to [trust its certificate](./get-started/set-up.md#start-dev-proxy-for-the-first-time).

```console
devproxy --config-file "~dataFolder/configs/github-rate-limiting/.devproxy/devproxyrc.json"
```

Run your app as usual. It keeps calling the real API URLs, and Dev Proxy simulates the API's rate limits and errors.

For presets for other APIs, see the [Dev Proxy samples gallery](https://aka.ms/devproxy/samples).

How does your app handle API errors?

> [!div class="nextstepaction"]
> [Get started](./get-started/set-up.md)

## See also

- [Set up Dev Proxy](./get-started/set-up.md) - Installation and first run
- [Configure Dev Proxy](./get-started/configure.md) - Customize to your needs
- [How-to guides](./how-to/overview.md) - Task-oriented guides
- [Technical reference](./technical-reference/overview.md) - Plugin documentation
