---
title: MockStdioResponsePlugin
description: MockStdioResponsePlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/13/2026
---

# MockStdioResponsePlugin

Simulates responses for STDIO-based applications, such as Model Context Protocol (MCP) servers.

## Plugin instance definition

```json
{
  "name": "MockStdioResponsePlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
  "configSection": "mockStdioResponsePlugin"
}
```

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
  "plugins": [
    {
      "name": "MockStdioResponsePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "mockStdioResponsePlugin"
    }
  ],
  "mockStdioResponsePlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/mockstdioresponseplugin.schema.json",
    "mocksFile": "stdio-mocks.json"
  }
}
```

## Configuration properties

| Property | Description | Default |
| -------- | ----------- | :-----: |
| `mocksFile` | Path to the file containing STDIO mock responses | `stdio-mocks.json` |
| `blockUnmockedRequests` | When `true`, prevents unmatched stdin from reaching the child process | `false` |

## Command line options

| Name | Description | Default |
| ---- | ----------- | :-----: |
| `--no-stdio-mocks` | Disable loading STDIO mock responses | `false` |
| `--stdio-mocks-file` | Path to the file containing STDIO mock responses | - |

## Mocks file examples

Following are examples of STDIO mock objects.

### Respond with stdout

Respond to stdin containing a specific text with a stdout response.

**File:** stdio-mocks.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/mockstdioresponseplugin.schema.json",
  "mocks": [
    {
      "request": {
        "bodyFragment": "tools/list"
      },
      "response": {
        "stdout": "{\"jsonrpc\":\"2.0\",\"id\":1,\"result\":{\"tools\":[]}}\n"
      }
    }
  ]
}
```

### Respond with stderr

Respond to stdin with an error message on stderr.

**File:** stdio-mocks.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/mockstdioresponseplugin.schema.json",
  "mocks": [
    {
      "request": {
        "bodyFragment": "invalid_method"
      },
      "response": {
        "stderr": "Error: Unknown method\n"
      }
    }
  ]
}
```

### Use placeholders from stdin

Use `@stdin.body.*` placeholders to dynamically include values from stdin in the response.

**File:** stdio-mocks.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/mockstdioresponseplugin.schema.json",
  "mocks": [
    {
      "request": {
        "bodyFragment": "tools/list"
      },
      "response": {
        "stdout": "{\"jsonrpc\":\"2.0\",\"id\":@stdin.body.id,\"result\":{\"tools\":[]}}\n"
      }
    }
  ]
}
```

The following placeholders are available:

| Placeholder | Description |
| ----------- | ----------- |
| `@stdin.body.id` | JSON-RPC request ID |
| `@stdin.body.method` | JSON-RPC method name |
| `@stdin.body.params.name` | Nested property access |

### Load response from file

Load the mock response content from an external file using `@filename` syntax.

**File:** stdio-mocks.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/mockstdioresponseplugin.schema.json",
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

**File:** initialize-response.json

```json
{"jsonrpc":"2.0","id":1,"result":{"protocolVersion":"2024-11-05","capabilities":{"tools":{}},"serverInfo":{"name":"Mock MCP Server","version":"1.0.0"}}}
```

### Respond on nth occurrence

Respond only after intercepting the matching stdin for the nth time.

**File:** stdio-mocks.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/mockstdioresponseplugin.schema.json",
  "mocks": [
    {
      "request": {
        "bodyFragment": "tools/call",
        "nth": 2
      },
      "response": {
        "stdout": "{\"jsonrpc\":\"2.0\",\"id\":@stdin.body.id,\"result\":{\"content\":[{\"type\":\"text\",\"text\":\"Operation completed\"}]}}\n"
      }
    }
  ]
}
```

## Mocks file properties

| Property | Description | Required |
| -------- | ----------- | :------: |
| `request` | [Request object](#request-object) that defines the stdin to match | yes |
| `response` | [Response object](#response-object) that defines the response to return | yes |

### Request object

Each request has the following properties:

| Property | Description | Required | Default value | Sample value |
| -------- | ----------- | :------: | ------------- | ------------ |
| `bodyFragment` | A string that should be present in stdin | yes | | `tools/list` |
| `nth` | Respond only when intercepting the request for the nth time | no | | `2` |

### Response object

Each response has the following properties:

| Property | Description | Required | Default value | Sample value |
| -------- | ----------- | :------: | ------------- | ------------ |
| `stdout` | Content to send to stdout | no | _empty_ | `{"result": "ok"}\n` |
| `stderr` | Content to send to stderr | no | _empty_ | `Error: Something went wrong\n` |

### Response remarks

If you want to load response content from a file, set the `stdout` or `stderr` property to a string value that starts with `@` followed by file path relative to the mocks file. For example, `@response.json` returns the content stored in the `response.json` file in the same directory as the mocks file.

To dynamically include values from stdin in the response, use `@stdin.body.*` placeholders. For example, `@stdin.body.id` returns the value of the `id` property from the stdin JSON.

## Plugin remarks

This plugin is designed for use with the [`stdio` command](stdio.md) to intercept and mock STDIO communication with local executables. It's useful for testing and debugging Model Context Protocol (MCP) servers and other STDIO-based applications.

When `blockUnmockedRequests` is set to `true`, any stdin that doesn't match a mock is consumed and not forwarded to the child process. Blocking unmocked requests is useful when you want to fully mock the behavior of an executable without running its actual logic.

## Next step

> [!div class="nextstepaction"]
> [Mock STDIO responses for MCP servers](../how-to/mock-stdio-responses-mcp-servers.md)
