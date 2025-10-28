---
title: HarGeneratorPlugin
description: HarGeneratorPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/28/2025
---

# HarGeneratorPlugin

Generates HTTP Archive (HAR) files from the intercepted requests and responses.

## Plugin instance definition

```json
{
  "name": "HarGeneratorPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
  "configSection": "harGeneratorPlugin"
}
```

## Configuration example

```json
{
  "harGeneratorPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.3.0/hargeneratorplugin.schema.json",
    "includeSensitiveInformation": false,
    "includeResponse": true
  }
}
```

## Configuration properties

| Property | Description | Default |
| -------- | ----------- | :-----: |
| `includeSensitiveInformation` | Determines whether to include sensitive information (authorization headers, cookies, etc.) in the generated HAR file. When set to `false`, sensitive headers are redacted with the value `REDACTED` | `false` |
| `includeResponse` | Determines whether to include response body content in the generated HAR file | `false` |

## Command line options

None

## Remarks

The HAR (HTTP Archive) format is a JSON-based format for logging HTTP transactions. Various tools widely support it and used it to:

- Analyze network traffic and performance
- Debug API interactions
- Share HTTP session data
- Import into browser developer tools and other analysis tools

When `includeSensitiveInformation` is set to `false`, the plugin automatically redacts the following sensitive headers:

- `authorization`
- `cookie`
- `from`
- `proxy-authenticate`
- `proxy-authorization`
- `set-cookie`
- `www-authenticate`
- `x-api-key`
- `x-auth-token`
- `x-csrf-token`
- `x-forwarded-for`
- `x-real-ip`
- `x-session-token`
- `x-xsrf-token`

The generated HAR file includes:

- HTTP request details (method, URL, headers, query parameters, cookies)
- HTTP response details (status, headers, cookies)
- Request and response body data (when applicable)
- Content types and sizes
- HTTP version information

The plugin creates a HAR file named `devproxy-{timestamp}.har` in the current directory after recording stops.
