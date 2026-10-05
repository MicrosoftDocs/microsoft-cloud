---
title: How to test what your AI agent does when its tools fail
description: How AI agents go wrong when an API or MCP server they call fails, how to handle tool errors, timeouts, and rate limits, and how to test your agent against tool failures.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Understand how to handle and test tool failures in an AI agent -->
<!-- AUDIENCE: Developers building AI agents that call HTTP APIs or MCP servers as tools -->

# How to test what your AI agent does when its tools fail

Your AI agent calls tools: HTTP APIs, MCP servers, and other services. Those tools time out, hit rate limits, return errors, and send data in shapes you didn't expect. The model decides what to do next based on what your code passes back to it. If your code passes back an exception, an empty string, or nothing at all after a long wait, the agent behaves differently than if it gets a clear error.

## How tool failures go wrong

- **The agent hangs.** A tool call without a timeout keeps the user waiting.
- **The agent loops.** The model calls the failing tool again and again, using up tokens and the tool's rate limit.
- **The agent covers it up.** Your code swallows the error, and the model answers as if the tool succeeded.
- **The agent crashes.** An unhandled exception ends the whole conversation.

## How to handle tool failures

1. **Set a timeout on every tool call and a budget for the whole turn.** The [MCP specification](https://modelcontextprotocol.io/specification/2025-06-18/server/tools) says clients should implement timeouts for tool calls.
1. **Retry temporary errors in code.** Handle 429 and 503 responses in your tool code, honor `Retry-After`, and cap the attempts, so the model doesn't have to decide when to retry.
1. **Return failures to the model as clear results.** MCP separates protocol errors, like an unknown tool or invalid arguments, from tool execution errors, like an API failure. It reports execution errors in the tool result with `isError: true`, so the model can see what went wrong. Say what failed and whether trying again makes sense.
1. **Cap the number of tool calls per turn.** After a set number of failures, stop and tell the user.
1. **Validate tool results before you pass them to the model.** The MCP specification says clients should do this, and should validate structured results against the tool's output schema when it has one.
1. **Tell the user what didn't work.** An answer built on a failed tool call should say so.

## How to test tool failure handling in your agent

| Approach | What you find | What you miss |
|---|---|---|
| Unit test your tool wrapper with a stubbed client | How your code maps the failure you wrote | What the model does with it, and how the real tool fails |
| Break the real tool, for example stop the server or revoke a key | A real failure of that kind | Rate limits, slow responses, and malformed data, which you can't cause on demand |
| Write a fake API or MCP server | Any response you script | You have to point your agent at the fake, and it drifts from the real tool |
| Intercept the agent's real tool traffic and inject failures | What the running agent and the model do with errors, latency, and bad data from the real tool | Your code in isolation. Keep your unit tests for that. |

Model output can vary between runs, so run each failure scenario more than once.

## Try it on your app

[Dev Proxy](../overview.md?WT.mc_id=devproxy-learn-test-ai-agent-tool-failures) sits between your agent and its tools and injects failures, with no changes to your agent's code.

For tools that call HTTP APIs, combine random errors, latency, and a check that your agent waits as long as the API asks:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/rc.schema.json",
  "plugins": [
    {
      "name": "RetryAfterPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    },
    {
      "name": "LatencyPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "latencyPlugin"
    },
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "errorsContosoApi"
    }
  ],
  "urlsToWatch": [
    "https://api.contoso.com/*"
  ],
  "latencyPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/latencyplugin.schema.json",
    "minMs": 2000,
    "maxMs": 10000
  },
  "errorsContosoApi": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/genericrandomerrorplugin.schema.json",
    "errorsFile": "errors-contoso-api.json",
    "rate": 50
  }
}
```

Define the errors in `errors-contoso-api.json`, as described in [Test my app with random errors](../how-to/test-my-app-with-random-errors.md?WT.mc_id=devproxy-learn-test-ai-agent-tool-failures). Set `Retry-After` to `@dynamic` on your `429` responses. The RetryAfterPlugin checks only those.

For MCP servers that use STDIO, start the server through [`devproxy stdio`](../technical-reference/stdio.md?WT.mc_id=devproxy-learn-test-ai-agent-tool-failures) with a config that enables the MockStdioResponsePlugin, as shown in the [`stdio` configuration example](../technical-reference/stdio.md?WT.mc_id=devproxy-learn-test-ai-agent-tool-failures#configuration-example). Save it as `devproxyrc-stdio.json`. Then put this in `stdio-mocks.json` to return a tool execution error for every `tools/call` request:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.3.1/mockstdioresponseplugin.mocksfile.schema.json",
  "mocks": [
    {
      "request": {
        "bodyFragment": "tools/call"
      },
      "response": {
        "stdout": "{\"jsonrpc\":\"2.0\",\"id\":@stdin.body.id,\"result\":{\"content\":[{\"type\":\"text\",\"text\":\"Failed to fetch weather data: API rate limit exceeded\"}],\"isError\":true}}\n"
      }
    }
  ]
}
```

```console
devproxy stdio --config-file devproxyrc-stdio.json npx -y @modelcontextprotocol/server-filesystem
```

To have your agent use it, change the command in your agent's MCP server configuration so it starts the server through `devproxy stdio`. Use the `nth` property on a mock to fail only a specific call, and add the LatencyPlugin to slow down the server's responses.

To test what your agent does when the model itself fails, the LanguageModelFailurePlugin makes the model hallucinate, ignore instructions, or answer in the wrong format. See [Test my app with language model failures](../how-to/test-my-app-with-language-model-failures.md?WT.mc_id=devproxy-learn-test-ai-agent-tool-failures).

To install Dev Proxy, see [Set up Dev Proxy](../get-started/set-up.md?WT.mc_id=devproxy-learn-test-ai-agent-tool-failures).

## Next steps

> [!div class="nextstepaction"]
> [Mock STDIO responses for MCP servers](../how-to/mock-stdio-responses-mcp-servers.md?WT.mc_id=devproxy-learn-test-ai-agent-tool-failures)

## See also

- [`stdio` command](../technical-reference/stdio.md?WT.mc_id=devproxy-learn-test-ai-agent-tool-failures)
- [Simulate slow API responses](../how-to/simulate-slow-api-responses.md?WT.mc_id=devproxy-learn-test-ai-agent-tool-failures)
- [LLM rate limits: tokens per minute, requests per minute, and what happens when you hit them](llm-rate-limits.md)
- [How to test whether your AI agent follows instructions hidden in tool responses](test-ai-agent-prompt-injection-tool-responses.md)
