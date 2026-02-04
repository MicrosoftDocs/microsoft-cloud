---
title: stdio
description: Dev Proxy stdio command reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/13/2026
---

# `stdio`

Proxies STDIN/STDOUT/STDERR communication with local executables. This command is particularly useful for testing and debugging Model Context Protocol (MCP) servers and other STDIO-based applications.

## Synopsis

```text
devproxy stdio [options] <command> [args...]
```

## Description

The `stdio` command starts a child process and proxies all STDIN/STDOUT/STDERR communication. This allows you to:

- Inspect messages flowing between clients (like VS Code) and MCP servers
- Mock responses to test client behavior without running actual server logic
- Debug communication issues using Chrome DevTools
- Test error handling by injecting mock responses or blocking requests
- Simulate latency on STDIO communication

## Usage

```console
devproxy stdio npx -y @modelcontextprotocol/server-filesystem
```

## Arguments

| Name | Description | Required |
| ---- | ----------- | :------: |
| `<command>` | The command to execute | yes |
| `[args...]` | Arguments to pass to the command | no |

## Options

| Name | Description | Allowed values | Default |
| ---- | ----------- | -------------- | ------- |
| `-c, --config-file <configFile>` | The path to the configuration file | Local file path | `devproxyrc.json` |
| `--no-stdio-mocks` | Disable loading STDIO mock responses | n/a | n/a |
| `--stdio-mocks-file <file>` | Path to the file containing STDIO mock responses | Local file path | - |
| `--log-level <loglevel>` | Level of messages to log | `trace`, `debug`, `information`, `warning`, `error` | `information` |
| `-?, -h, --help` | Show help and usage information | n/a | n/a |

## Examples

### Start an MCP server with Dev Proxy

```console
devproxy stdio npx -y @modelcontextprotocol/server-filesystem
```

### Specify a custom configuration file

```console
devproxy stdio --config-file ./my-config.json npx my-server
```

### Specify a custom mocks file

```console
devproxy stdio --stdio-mocks-file ./my-mocks.json npx my-server
```

### Disable stdio mocks

```console
devproxy stdio --no-stdio-mocks npx my-server
```

## Configuration example

To use the `stdio` command with plugins, create a configuration file:

**File:** devproxyrc.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
  "plugins": [
    {
      "name": "MockStdioResponsePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "mockStdioResponsePlugin"
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

## Supported plugins

The following plugins support STDIO interception:

| Plugin | Description |
| ------ | ----------- |
| [MockStdioResponsePlugin](mockstdioresponseplugin.md) | Mock STDIN/STDOUT/STDERR responses |
| [DevToolsPlugin](devtoolsplugin.md) | Inspect STDIO traffic in Chrome DevTools |
| [LatencyPlugin](latencyplugin.md) | Add artificial latency to STDIO communication |

## Architecture

When using the `stdio` command, Dev Proxy acts as a middleman between the parent process (such as VS Code) and the child process (such as an MCP server):

```text
┌─────────────────┐     STDIN      ┌──────────────────┐     STDIN      ┌─────────────────┐
│  Parent Process │ ─────────────▶ │   Dev Proxy      │ ─────────────▶ │  Child Process  │
│   (VS Code)     │                │                  │                │   (MCP Server)  │
│                 │ ◀───────────── │  ┌────────────┐  │ ◀───────────── │                 │
└─────────────────┘    STDOUT      │  │  Plugins   │  │    STDOUT      └─────────────────┘
                                   │  └────────────┘  │
                                   └──────────────────┘
```

Plugins are invoked in order:

1. `BeforeStdinAsync` - Before forwarding stdin to child
2. `AfterStdoutAsync` - After receiving stdout from child
3. `AfterStderrAsync` - After receiving stderr from child

## DevTools integration

When you enable the [DevToolsPlugin](devtoolsplugin.md), you can inspect STDIO traffic in Chrome DevTools:

- Messages appear with `stdio://command-name` URLs
- Requests show as `STDIN` method
- Responses show as `STDOUT` (200 status) or `STDERR` (500 status)
- Message bodies are formatted as JSON when applicable

## Next step

> [!div class="nextstepaction"]
> [Mock STDIO responses for MCP servers](../how-to/mock-stdio-responses-mcp-servers.md)
