---
title: Mock STDIO responses for MCP servers
description: How to mock STDIO responses for Model Context Protocol (MCP) servers using Dev Proxy
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/07/2026
ms.topic: how-to
---

# Mock STDIO responses for MCP servers

Modern AI development increasingly relies on local tools that communicate via STDIO, particularly Model Context Protocol (MCP) servers. These servers receive JSON-RPC requests via stdin and send JSON-RPC responses via stdout. Using Dev Proxy, you can intercept and mock STDIO communication to test your AI client applications without running actual server logic.

## Prerequisites

- [Dev Proxy installed and running](../get-started/set-up.md)

## Mock MCP server responses

To mock MCP server responses, you use the `stdio` command with the `MockStdioResponsePlugin`. The plugin intercepts stdin and returns mock responses via stdout or stderr.

### 1. Create a Dev Proxy configuration file

Create a `devproxyrc.json` file with the following content:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
  "plugins": [
    {
      "name": "MockStdioResponsePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "mockSTDIOResponsePlugin"
    },
    {
      "name": "DevToolsPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "devTools"
    }
  ],
  "devTools": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/devtoolsplugin.schema.json",
    "preferredBrowser": "Edge"
  },
  "mockStdioResponsePlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/mockstdioresponseplugin.schema.json",
    "mocksFile": "stdio-mocks.json"
  }
}
```

### 2. Create a mocks file

Create a `stdio-mocks.json` file with mock responses for the MCP server:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/mockstdioresponseplugin.mocksfile.schema.json",
  "mocks": [
    {
      "request": {
        "bodyFragment": "initialize"
      },
      "response": {
        "stdout": "{\"jsonrpc\":\"2.0\",\"id\":@stdin.body.id,\"result\":{\"protocolVersion\":\"2024-11-05\",\"capabilities\":{\"tools\":{}},\"serverInfo\":{\"name\":\"Mock MCP Server\",\"version\":\"1.0.0\"}}}\n"
      }
    },
    {
      "request": {
        "bodyFragment": "tools/list"
      },
      "response": {
        "stdout": "{\"jsonrpc\":\"2.0\",\"id\":@stdin.body.id,\"result\":{\"tools\":[{\"name\":\"get_weather\",\"description\":\"Get current weather for a location\",\"inputSchema\":{\"type\":\"object\",\"properties\":{\"location\":{\"type\":\"string\",\"description\":\"City name\"}},\"required\":[\"location\"]}}]}}\n"
      }
    },
    {
      "request": {
        "bodyFragment": "tools/call"
      },
      "response": {
        "stdout": "{\"jsonrpc\":\"2.0\",\"id\":@stdin.body.id,\"result\":{\"content\":[{\"type\":\"text\",\"text\":\"Mock response from the tool\"}]}}\n"
      }
    }
  ]
}
```

### 3. Start Dev Proxy

Run Dev Proxy with the `STDIO` command, specifying the command to run:

```console
devproxy stdio npx -y @modelcontextprotocol/server-filesystem
```

Dev Proxy starts the MCP server as a child process and intercepts all stdin/stdout communication. When stdin contains text matching a mock's `bodyFragment`, Dev Proxy returns the mock response instead of forwarding the request to the actual server.

## Use placeholders for dynamic responses

To create dynamic responses that include values from the request, use `@stdin.body.*` placeholders:

```json
{
  "mocks": [
    {
      "request": {
        "bodyFragment": "echo"
      },
      "response": {
        "stdout": "{\"jsonrpc\":\"2.0\",\"id\":@stdin.body.id,\"result\":{\"message\":\"You said: @stdin.body.params.text\"}}\n"
      }
    }
  ]
}
```

Available placeholders:

| Placeholder | Description |
| ----------- | ----------- |
| `@stdin.body.id` | JSON-RPC request ID |
| `@stdin.body.method` | JSON-RPC method name |
| `@stdin.body.params.*` | Access to request parameters |

## Block unmocked requests

To prevent unmocked requests from reaching the child process, set `blockUnmockedRequests` to `true`:

```json
{
  "mockStdioResponsePlugin": {
    "mocksFile": "stdio-mocks.json",
    "blockUnmockedRequests": true
  }
}
```

Blocking unmocked requests is useful when you want to fully mock the MCP server without running its actual logic.

## Load mock responses from files

For complex responses, load the content from external files:

```json
{
  "mocks": [
    {
      "request": {
        "bodyFragment": "initialize"
      },
      "response": {
        "stdout": "@initialize-response.json"
      }
    }
  ]
}
```

Create the `initialize-response.json` file:

```json
{"jsonrpc":"2.0","id":1,"result":{"protocolVersion":"2024-11-05","capabilities":{"tools":{}},"serverInfo":{"name":"Mock MCP Server","version":"1.0.0"}}}
```

## Inspect STDIO traffic in DevTools

When you enable the `DevToolsPlugin`, Dev Proxy opens Chrome DevTools where you can inspect all STDIO communication:

- **Network tab**: View all stdin/stdout/stderr messages
- **URLs**: Messages appear as `stdio://command-name`
- **Methods**: Requests show as `stdin`
- **Status codes**: `stdout` appears as 200, `stderr` as 500
- **Timing**: See how long each request/response took

Using the `DevToolsPlugin` is invaluable for debugging communication issues between your AI client and MCP servers.

## Simulate latency

To test how your application handles slow MCP server responses, add the `LatencyPlugin`:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
  "plugins": [
    {
      "name": "LatencyPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "latencyPlugin"
    },
    {
      "name": "MockStdioResponsePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "mockStdioResponsePlugin"
    }
  ],
  "latencyPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/latencyplugin.schema.json",
    "minMs": 100,
    "maxMs": 500
  },
  "mockStdioResponsePlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/mockstdioresponseplugin.schema.json",
    "mocksFile": "stdio-mocks.json"
  }
}
```

## Next step

Learn more about the STDIO proxy feature:

> [!div class="nextstepaction"]
> [`stdio` command reference](../technical-reference/stdio.md)
