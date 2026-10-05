---
title: "Mocks, stubs, fakes, and emulators: testing API calls with coding agents"
description: What stubs, mocks, fake servers, emulators, and intercepting proxies test when your code calls an API, what coding agents build when you ask them to test API failures, and how to close the gap.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Understand the options for testing code that calls APIs and choose the right one when a coding agent writes the tests -->
<!-- AUDIENCE: Developers who use coding agents to write code that calls third-party APIs and want to know what the agent's tests prove -->

# Mocks, stubs, fakes, and emulators: testing API calls with coding agents

When your code calls an API, you need a way to test it without waiting for the real API to fail. You have 5 kinds of stand-ins to choose from. People use the names loosely, so here's how this article uses them:

- **Stub**: an in-process replacement for your HTTP client or SDK call that returns a canned answer. It's fast and repeatable, and it tests your logic for the response you wrote.
- **Mock**: a stub that also records how your code called it, so your test can check the calls. It reaches as far as a stub.
- **Fake server**: a small working server, often in-memory, that your app calls over HTTP instead of the real API. It exercises your HTTP client, but you have to point your app at its URL.
- **Emulator**: a local version of a service, usually published by the service's owner, that behaves like the real one for the operations it supports. Check which limits and failures it covers before you rely on it for error handling.
- **Intercepting proxy**: sits on the network between your app and the API. Your app calls the real URL, and the proxy passes requests through or answers some of them with the response you define. Your app has to send its traffic through the proxy and, for HTTPS, trust the proxy's certificate.

## How to test code that calls an API

| Approach | What it tests | What it needs | Right when |
|---|---|---|---|
| Stub or mock | Your logic for a specific response | Your test framework | You test business logic, parsing, and error branches in unit tests |
| Fake server | Your HTTP client and serialization | A base URL setting or config switch in your app | The API doesn't exist yet, or you need a stable backend for UI work |
| Emulator | Behavior close to the real service for supported operations | A different endpoint or connection string | The service's owner ships one and you develop offline |
| Real API with a test account | The real thing | Credentials, quota, and money | You verify the main path end to end |
| Intercepting proxy | Your running app on real URLs, including SDK retries and response headers | Proxy settings and certificate trust | You test failures, limits, and latency without changing your app |

You need more than 1 of these. Stubs keep your unit tests fast. A proxy shows you what the whole app does when the real API misbehaves. For more on how the 2 fit together, see [Dev Proxy vs unit tests](dev-proxy-vs-unit-tests.md).

## What coding agents build

When you ask a coding agent to make your app handle API failures and show that it works, it picks a stand-in for you. We wanted to know which one, so we ran a test with 3 coding agents (210 runs). The tasks used phrasings like "handle rate limiting properly and show me that it works", "verify it without spending money on real API calls", and "run this without an OpenAI key or an internet connection".

- In 76% of the 140 runs that asked for working code, the agent built the failure by hand: fetch stubs, `httpx.MockTransport`, or a throwaway HTTP server.
- In 61 of the 105 runs on apps that call GitHub, OpenAI, or a weather API, the agent added a base URL or config switch to the app so it could reach its fake.
- In the 75 runs where the real API was available and the prompt didn't rule it out, 0 tested the app on its real API URL.

Stubs are a reasonable choice for unit tests. The gap is what they leave out. The agent's stub returns the error the agent expected, which might not match what the API sends. And the switch it added to reach the fake ships with your app.

## How to work with your agent's tests

- **Keep the stubs for your logic.** They're fast, and they test the branches the agent wrote.
- **Ask for the real error format.** Ask the agent to base each simulated error on the provider's documented status codes, headers, and body fields. A bare 429 doesn't test that your app reads `retry-after` or tells a billing error from a rate limit.
- **Review switches added for tests.** If the agent adds a base URL setting only so tests can reach a fake, decide whether you want that setting in your production code.
- **Run the app once on real URLs with simulated failures.** Before you ship, check what the running app, its SDK, and its retry policy do with the API's own errors. To review the agent's error handling step by step, see [How to verify error handling your coding agent wrote](verify-agent-written-error-handling.md).

## Try it on your app

[Dev Proxy](../overview.md?WT.mc_id=devproxy-learn-mocks-stubs-fakes-coding-agents) is an intercepting proxy for development. It returns the responses you define for the URLs your app already calls, with no changes to your app's code. Enable the MockResponsePlugin in your config, `devproxyrc.json`:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/rc.schema.json",
  "plugins": [
    {
      "name": "MockResponsePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "mocksPlugin"
    }
  ],
  "urlsToWatch": [
    "https://api.contoso.com/*"
  ],
  "mocksPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/mockresponseplugin.schema.json",
    "mocksFile": "mocks.json"
  }
}
```

Then define the response in `mocks.json`. This one returns `503` with a `Retry-After` header for the forecast endpoint:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/mockresponseplugin.mocksfile.schema.json",
  "mocks": [
    {
      "request": {
        "url": "https://api.contoso.com/v1/forecast*",
        "method": "GET"
      },
      "response": {
        "statusCode": 503,
        "headers": [
          {
            "name": "Retry-After",
            "value": "10"
          }
        ],
        "body": {
          "error": "Service unavailable"
        }
      }
    }
  ]
}
```

Start Dev Proxy and run your app as usual:

```console
devproxy --config-file devproxyrc.json
```

Requests that don't match a mock go to the real API. When you need a backend that doesn't exist yet, the CrudApiPlugin [simulates a CRUD API](../how-to/simulate-crud-api.md?WT.mc_id=devproxy-learn-mocks-stubs-fakes-coding-agents) with in-memory data. To fail a share of requests at random instead of every time, see [Test my app with random errors](../how-to/test-my-app-with-random-errors.md?WT.mc_id=devproxy-learn-mocks-stubs-fakes-coding-agents). To install Dev Proxy, see [Set up Dev Proxy](../get-started/set-up.md?WT.mc_id=devproxy-learn-mocks-stubs-fakes-coding-agents).

## Next steps

> [!div class="nextstepaction"]
> [Mock responses](../how-to/mock-responses.md?WT.mc_id=devproxy-learn-mocks-stubs-fakes-coding-agents)

## See also

- [Dev Proxy vs unit tests](dev-proxy-vs-unit-tests.md)
- [What is a proxy?](what-is-proxy.md)
- [Simulate a CRUD API](../how-to/simulate-crud-api.md?WT.mc_id=devproxy-learn-mocks-stubs-fakes-coding-agents)
- [How to verify error handling your coding agent wrote](verify-agent-written-error-handling.md)
