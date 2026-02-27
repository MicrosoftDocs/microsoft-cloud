---
title: Use the MCP server
description: Learn how to control Dev Proxy from AI coding assistants using the MCP server
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/26/2026
ms.topic: how-to
#Customer intent: As a developer, I want to use AI coding assistants to help me configure Dev Proxy so that I can work more efficiently.
---

<!-- INTENT: Use AI coding assistants to work with Dev Proxy -->
<!-- JOB: Set up the Dev Proxy MCP server -->
<!-- TIME: 5 minutes -->

# Use the MCP server

The Dev Proxy MCP (Model Context Protocol) server enables AI coding assistants to help you work with Dev Proxy. Using this server, you can ask your AI assistant to create Dev Proxy configurations, find documentation, and get contextual help—all using natural language.

## What is MCP?

[Model Context Protocol (MCP)](https://modelcontextprotocol.io) is an open standard that allows AI assistants to connect to external tools and data sources. The Dev Proxy MCP server gives your AI assistant access to Dev Proxy documentation and best practices, enabling it to provide accurate, context-aware help.

## Prerequisites

Before you begin, ensure you have:

- [Dev Proxy installed](../get-started/set-up.md)
- [Node.js](https://nodejs.org/) Long Term Support (LTS) installed
- An MCP-compatible AI assistant:
  - [Visual Studio (VS) Code with GitHub Copilot](https://code.visualstudio.com/docs/copilot/chat/mcp-servers)
  - [GitHub Copilot CLI](https://docs.github.com/en/copilot/how-tos/set-up/install-copilot-cli)
  - [Claude Code](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code/overview)
  - [Cursor](https://www.cursor.com/)
  - Other MCP-compatible clients

## Configure the MCP server

The Dev Proxy MCP server is published on npm as [`@devproxy/mcp`](https://www.npmjs.com/package/@devproxy/mcp). Configure it in your AI assistant using these values:

| Setting | Value |
| ------- | ----- |
| Type | `stdio` |
| Command | `npx` |
| Arguments | `-y`, `@devproxy/mcp` |

The exact configuration steps vary by AI assistant.

### Visual Studio Code with GitHub Copilot

1. Open Visual Studio Code.
1. Open the Command Palette (<kbd>Ctrl</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd> or <kbd>Cmd</kbd>+<kbd>Shift</kbd>+<kbd>P</kbd>).
1. Type **MCP: Add Server** and select it.
1. Select **Command (stdio)**.
1. Enter `npx` as the command.
1. Enter `-y @devproxy/mcp` as the arguments.
1. Press <kbd>Enter</kbd> to save.

Alternatively, add the following to your VS Code settings JSON (`.vscode/mcp.json` in your workspace or user settings):

```json
{
  "servers": {
    "Dev Proxy": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@devproxy/mcp"]
    }
  }
}
```

### GitHub Copilot CLI

Add the following to your Copilot CLI MCP configuration:

**File**: `~/.copilot/mcp-config.json`

```json
{
  "mcpServers": {
    "devproxy": {
      "command": "npx",
      "args": ["-y", "@devproxy/mcp"]
    }
  }
}
```

Alternatively, use the `/mcp add` slash command in an interactive Copilot CLI session to add the server.

### Claude Code

Run the following command in your terminal to add the Dev Proxy MCP server:

```console
claude mcp add devproxy -- npx -y @devproxy/mcp
```

### Cursor

Add the following to your Cursor MCP configuration:

**File**: `~/.cursor/mcp.json`

```json
{
  "mcpServers": {
    "devproxy": {
      "command": "npx",
      "args": ["-y", "@devproxy/mcp"]
    }
  }
}
```

Restart Cursor after saving the configuration.

## Available tools

The Dev Proxy MCP server provides the following tools to AI assistants:

| Tool | Description |
| ---- | ----------- |
| `GetBestPractices` | Returns best practices for configuring and using Dev Proxy. The AI assistant typically calls this tool before generating any Dev Proxy configuration. |
| `FindDocs` | Searches Dev Proxy documentation for information related to a query. Useful for finding specific plugin documentation or how-to guides. |
| `GetVersion` | Returns the installed Dev Proxy version. Helps ensure generated configurations are compatible with your version. |

## Example prompts

After configuring the MCP server, you can ask your AI assistant questions about Dev Proxy in natural language. Here are some example prompts:

### Create a configuration to mock an API

> Create a Dev Proxy configuration to mock a REST API at `https://api.example.com/products` that returns a list of products.

### Simulate API errors

> Set up Dev Proxy to randomly return 500 errors for 30% of requests to `https://api.contoso.com/*`.

### Get help with a specific plugin

> How do I configure the RateLimitingPlugin to simulate throttling?

### Test Microsoft Graph API permissions

> Create a Dev Proxy configuration to check if my app is using minimal permissions for Microsoft Graph.

The AI assistant uses the MCP server to find relevant documentation and best practices, then generates accurate Dev Proxy configurations tailored to your needs.

## Verify the MCP server works

To verify the MCP server is working:

1. Open your AI assistant's chat interface.
1. Ask a question about Dev Proxy, for example: "What version of Dev Proxy do I have installed?"
1. The AI assistant should use the `GetVersion` tool and report your installed version.

If the AI assistant can't access the tools, verify:

- Node.js is installed and available in your PATH
- The MCP server configuration is correct
- You restarted your AI assistant after configuration

## Next step

Learn how to configure Dev Proxy for your specific needs:

> [!div class="nextstepaction"]
> [Configuration](../get-started/configure.md)
