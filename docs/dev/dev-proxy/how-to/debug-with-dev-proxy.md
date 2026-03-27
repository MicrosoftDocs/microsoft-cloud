---
title: Debug with Dev Proxy
description: Learn how to debug your application while using Dev Proxy to simulate API errors
author: waldekmastykarz
ms.author: wmastyka
ms.date: 03/26/2026
ms.topic: how-to
---

<!-- INTENT: Debug app while using Dev Proxy to simulate API errors -->
<!-- SOLUTION: Configure Visual Studio (VS) Code launch.json with proxy and certificate settings -->
<!-- RESULT: Hit breakpoints when Dev Proxy returns errors, step through error handling code -->
<!-- PLUGINS: GenericRandomErrorPlugin -->
<!-- JOB: debug-error-handling -->
<!-- TIME: 15 minutes -->

# Debug with Dev Proxy

> **At a glance**  
> **Goal:** Debug your application while Dev Proxy simulates API errors  
> **Time:** 15 minutes  
> **Plugins:** [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md), VS Code

When you build applications that call APIs, you need to ensure your code handles errors properly. By combining Dev Proxy with Visual Studio (VS) Code's debugger, you can simulate API errors and step through your error handling code to verify it works correctly.

## Overview

Debugging with Dev Proxy involves three steps:

1. Configure VS Code to route HTTP requests through Dev Proxy
2. Configure Dev Proxy to simulate specific errors
3. Set breakpoints in your error handling code and debug

## Configure VS Code for debugging with Dev Proxy

How you configure VS Code depends on the programming language you use. The following sections show you how to configure VS Code for Node.js, .NET, and Python applications.

### Node.js

To debug a Node.js application with Dev Proxy, configure the `launch.json` file to set the proxy environment variables and disable TLS/SSL certificate verification.

**File:** .vscode/launch.json

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Debug with Dev Proxy",
      "skipFiles": [
        "<node_internals>/**"
      ],
      "program": "${workspaceFolder}/index.js",
      "env": {
        "NODE_ENV": "development",
        "http_proxy": "http://127.0.0.1:8000",
        "https_proxy": "http://127.0.0.1:8000",
        "GLOBAL_AGENT_HTTP_PROXY": "http://127.0.0.1:8000",
        "NODE_TLS_REJECT_UNAUTHORIZED": "0"
      }
    }
  ]
}
```

> [!IMPORTANT]
> The `NODE_TLS_REJECT_UNAUTHORIZED=0` setting disables TLS/SSL certificate verification. Only use this setting during development. For a more secure approach, use `NODE_EXTRA_CA_CERTS` to trust the Dev Proxy certificate directly:
>
> **macOS/Linux:**
> ```json
> "NODE_EXTRA_CA_CERTS": "${env:HOME}/.devproxy/rootCert.pem"
> ```
>
> **Windows:**
> ```json
> "NODE_EXTRA_CA_CERTS": "${env:USERPROFILE}\\.devproxy\\rootCert.pem"
> ```

The `GLOBAL_AGENT_HTTP_PROXY` variable is used by the [`global-agent`](https://www.npmjs.com/package/global-agent) package, which provides proxy support for many Node.js HTTP libraries. For more information on configuring different HTTP libraries, see [Use Dev Proxy with Node.js applications](./use-dev-proxy-with-nodejs.md).

### .NET

.NET applications automatically use the system proxy settings. To debug a .NET application with Dev Proxy, you typically don't need to set any environment variables. Create a basic debug configuration:

**File:** .vscode/launch.json

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Debug with Dev Proxy",
      "type": "coreclr",
      "request": "launch",
      "preLaunchTask": "build",
      "program": "${workspaceFolder}/bin/Debug/net8.0/MyApp.dll",
      "args": [],
      "cwd": "${workspaceFolder}",
      "console": "internalConsole",
      "stopAtEntry": false
    }
  ]
}
```

If your application doesn't pick up the system proxy, explicitly set the proxy environment variables:

**File:** .vscode/launch.json (with explicit proxy settings)

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Debug with Dev Proxy",
      "type": "coreclr",
      "request": "launch",
      "preLaunchTask": "build",
      "program": "${workspaceFolder}/bin/Debug/net8.0/MyApp.dll",
      "args": [],
      "cwd": "${workspaceFolder}",
      "console": "internalConsole",
      "stopAtEntry": false,
      "env": {
        "http_proxy": "http://127.0.0.1:8000",
        "https_proxy": "http://127.0.0.1:8000"
      }
    }
  ]
}
```

> [!TIP]
> On Windows and macOS, .NET trusts the Dev Proxy certificate automatically if you installed it as a trusted root certificate during the [Dev Proxy setup](../get-started/set-up.md). On Linux, you might need to trust the certificate manually. See your distribution's documentation for configuring trusted certificates.

### Python

To debug a Python application with Dev Proxy, configure the `launch.json` file to set the proxy environment variables. The configuration depends on whether you use the `requests` library or other HTTP libraries.

**File:** .vscode/launch.json

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Debug with Dev Proxy",
      "type": "debugpy",
      "request": "launch",
      "program": "${workspaceFolder}/main.py",
      "console": "integratedTerminal",
      "env": {
        "HTTP_PROXY": "http://127.0.0.1:8000",
        "HTTPS_PROXY": "http://127.0.0.1:8000",
        "REQUESTS_CA_BUNDLE": ""
      }
    }
  ]
}
```

> [!IMPORTANT]
> Setting `REQUESTS_CA_BUNDLE` to an empty string tells the `requests` library to skip TLS/SSL certificate verification. Only use this setting during development.
>
> For a more secure approach, point `REQUESTS_CA_BUNDLE` to the Dev Proxy certificate:
>
> **macOS/Linux:**
> ```json
> "REQUESTS_CA_BUNDLE": "${env:HOME}/.devproxy/rootCert.pem"
> ```
>
> **Windows:**
> ```json
> "REQUESTS_CA_BUNDLE": "${env:USERPROFILE}\\.devproxy\\rootCert.pem"
> ```

If you use the `httpx` library, configure TLS/SSL verification using the `TLS/SSL_CERT_FILE` environment variable instead:

```json
{
  "env": {
    "HTTP_PROXY": "http://127.0.0.1:8000",
    "HTTPS_PROXY": "http://127.0.0.1:8000",
    "TLS/SSL_CERT_FILE": "${env:HOME}/.devproxy/rootCert.pem"
  }
}
```

## Configure Dev Proxy to simulate errors

To debug error handling, configure Dev Proxy to return specific errors when your app calls APIs. Use the [GenericRandomErrorPlugin](../technical-reference/genericrandomerrorplugin.md) to simulate errors like `429 Too Many Requests` or `500 Internal Server Error`.

### Simulate a 429 (Too Many Requests) error

Create a Dev Proxy configuration file that simulates throttling:

**File:** devproxyrc.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.3.0/rc.schema.json",
  "plugins": [
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "errorsConfig"
    }
  ],
  "urlsToWatch": [
    "https://api.contoso.com/*"
  ],
  "rate": 100,
  "errorsConfig": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.3.0/genericrandomerrorplugin.schema.json",
    "errorsFile": "errors.json"
  }
}
```

Create the errors file with the specific error responses:

**File:** errors.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.3.0/genericrandomerrorplugin.errorsfile.schema.json",
  "errors": [
    {
      "request": {
        "url": "https://api.contoso.com/*"
      },
      "responses": [
        {
          "statusCode": 429,
          "headers": [
            {
              "name": "content-type",
              "value": "application/json"
            },
            {
              "name": "Retry-After",
              "value": "60"
            }
          ],
          "body": {
            "error": {
              "code": "TooManyRequests",
              "message": "Rate limit exceeded. Retry after 60 seconds."
            }
          }
        }
      ]
    }
  ]
}
```

> [!TIP]
> Set `rate` to `100` to make every request fail so that you consistently hit your error handling code during debugging. Lower the rate when you want to test intermittent errors.

### Simulate a 500 (Internal Server Error)

To simulate server errors, add another error response:

**File:** errors.json (multiple errors)

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.3.0/genericrandomerrorplugin.errorsfile.schema.json",
  "errors": [
    {
      "request": {
        "url": "https://api.contoso.com/*"
      },
      "responses": [
        {
          "statusCode": 429,
          "headers": [
            {
              "name": "content-type",
              "value": "application/json"
            },
            {
              "name": "Retry-After",
              "value": "60"
            }
          ],
          "body": {
            "error": {
              "code": "TooManyRequests",
              "message": "Rate limit exceeded. Retry after 60 seconds."
            }
          }
        },
        {
          "statusCode": 500,
          "headers": [
            {
              "name": "content-type",
              "value": "application/json"
            }
          ],
          "body": {
            "error": {
              "code": "InternalServerError",
              "message": "An unexpected error occurred."
            }
          }
        }
      ]
    }
  ]
}
```

When you define multiple error responses, Dev Proxy randomly selects one for each intercepted request.

## Debug your error handling code

With VS Code and Dev Proxy configured, you can now debug your error handling code.

### Step 1: Set breakpoints

Open your source code and set breakpoints in your error handling code. For example, in a Node.js application:

```javascript
async function fetchData() {
  try {
    const response = await fetch('https://api.contoso.com/data');
    
    if (!response.ok) {
      // Set a breakpoint here to debug error responses
      throw new Error(`HTTP error: ${response.status}`);
    }
    
    return await response.json();
  } catch (error) {
    // Set a breakpoint here to debug exceptions
    console.error('Failed to fetch data:', error);
    throw error;
  }
}
```

### Step 2: Start Dev Proxy

Start Dev Proxy with your configuration file:

```console
devproxy --config-file devproxyrc.json
```

### Step 3: Start debugging in VS Code

1. Open VS Code
1. Press **F5** or select **Run > Start Debugging**
1. When your application makes an API call, Dev Proxy intercepts it and returns an error
1. VS Code pauses at your breakpoint
1. Use the debug controls to step through your code and inspect variables

### Step 4: Inspect the error

When VS Code pauses at your breakpoint, use the debug panel to:

- View the error response status code and message
- Check the values of local variables
- Step through your retry logic
- Verify that your error messages are user-friendly

## Automatically start and stop Dev Proxy

To streamline your debugging workflow, configure VS Code to automatically start Dev Proxy when you begin debugging and stop it when you're done. Install the [Dev Proxy Toolkit extension](https://marketplace.visualstudio.com/items?itemName=garrytrinder.dev-proxy-toolkit) and configure tasks:

**File:** .vscode/tasks.json

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "devproxy-start",
      "type": "devproxy",
      "command": "start",
      "args": [
        "--config-file",
        "devproxyrc.json"
      ],
      "isBackground": true,
      "problemMatcher": "$devproxy-watch"
    },
    {
      "label": "devproxy-stop",
      "type": "devproxy",
      "command": "stop"
    }
  ]
}
```

To use these tasks, update your `launch.json`:

**File:** .vscode/launch.json (Node.js with autostart)

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "type": "node",
      "request": "launch",
      "name": "Debug with Dev Proxy",
      "skipFiles": [
        "<node_internals>/**"
      ],
      "program": "${workspaceFolder}/index.js",
      "preLaunchTask": "devproxy-start",
      "postDebugTask": "devproxy-stop",
      "env": {
        "NODE_ENV": "development",
        "http_proxy": "http://127.0.0.1:8000",
        "https_proxy": "http://127.0.0.1:8000",
        "GLOBAL_AGENT_HTTP_PROXY": "http://127.0.0.1:8000",
        "NODE_TLS_REJECT_UNAUTHORIZED": "0"
      }
    }
  ]
}
```

## Tips for effective debugging

- **To consistently hit your breakpoints**, set `"rate": 100` in your Dev Proxy configuration so that every request fails.
- **To understand how your code handles a specific error**, use a single error response when debugging.
- **To verify your code respects throttling**, check that it reads and follows the `Retry-After` header when debugging 429 errors.
- **To ensure your code handles unusual responses gracefully**, use Dev Proxy to simulate edge cases like malformed JSON or missing headers.
- **To break only on specific errors**, use conditional breakpoints in VS Code by right-clicking a breakpoint and adding a condition like `response.status === 429`.

## See also

- [Use Dev Proxy with Visual Studio Code debug configurations](./use-dev-proxy-with-vs-code-debug-configurations.md) - autostart Dev Proxy in VS Code
- [Test my app with random errors](./test-my-app-with-random-errors.md) - simulate random API errors
- [Change request failure rate](./change-request-failure-rate.md) - adjust how often errors occur
- [Use Dev Proxy with Node.js applications](./use-dev-proxy-with-nodejs.md) - Node.js proxy configuration
- [Use Dev Proxy with .NET applications](./use-dev-proxy-with-dotnet.md) - .NET proxy configuration
