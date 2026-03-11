---
title: Use the system proxy option
description: Learn how to control whether Dev Proxy registers as the system proxy
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/26/2026
ms.topic: how-to
zone_pivot_groups: client-operating-system
---

<!-- INTENT: Control whether Dev Proxy registers as system proxy -->
<!-- SOLUTION: Use asSystemProxy option in config file or CLI -->
<!-- RESULT: Fine-grained control over traffic routing -->
<!-- JOB: configure-proxy -->
<!-- TIME: 5 minutes -->

# Use the system proxy option

> **At a glance**  
> **Goal:** Control whether Dev Proxy captures all system traffic or only explicitly routed traffic  
> **Time:** 5 minutes  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

By default, when you start Dev Proxy, it registers itself as the system proxy. As a result, all HTTP/HTTPS traffic on your machine is automatically routed through Dev Proxy. Proxying all traffic through Dev Proxy works well for most scenarios—you start Dev Proxy, and it immediately captures requests from your application without any extra configuration.

However, there are situations where you might want more control over which traffic goes through Dev Proxy. The `asSystemProxy` option lets you disable automatic system proxy registration, giving you fine-grained control over which applications use Dev Proxy.

## When to disable system proxy registration

Consider setting `asSystemProxy` to `false` when:

- **You're on a corporate network** with existing proxy settings that you don't want to override
- **You only want to test a specific application** without affecting other apps running on your machine
- **Other applications fail** when Dev Proxy intercepts their traffic (for example, Azure Functions)
- **You're running multiple Dev Proxy instances** and want to route traffic to specific instances
- **You want to minimize interference** with system services and background processes

## Configure the system proxy option

You can configure the `asSystemProxy` option in two ways: using the configuration file or command line.

### Configuration file

To persistently disable system proxy registration, add the `asSystemProxy` setting to your configuration file.

**File:** `devproxyrc.json`  
**Purpose:** Disable automatic system proxy registration

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.2.0/rc.schema.json",
  "asSystemProxy": false,
  "urlsToWatch": [
    "https://api.contoso.com/*"
  ]
}
```

### Command line

To disable system proxy registration for a single session, use the `--as-system-proxy` option.

```console
devproxy --as-system-proxy false
```

## Route traffic to Dev Proxy manually

When you disable system proxy registration, applications don't automatically route their traffic through Dev Proxy. You need to configure each application to use Dev Proxy explicitly.

### Using environment variables

The most common way to route traffic to Dev Proxy is by setting the `HTTPS_PROXY` environment variable.

::: zone pivot="client-operating-system-windows"

**PowerShell:**

```powershell
$env:HTTPS_PROXY = "http://127.0.0.1:8000"
node app.js
```

**Command Prompt:**

```cmd
set HTTPS_PROXY=http://127.0.0.1:8000
node app.js
```

::: zone-end

::: zone pivot="client-operating-system-macos,client-operating-system-linux"

```console
HTTPS_PROXY=http://127.0.0.1:8000 node app.js
```

::: zone-end

> [!TIP]
> Some applications also support the `HTTP_PROXY` environment variable. Set both if your application makes both HTTP and HTTPS requests.

### Language-specific configuration

Different programming languages and frameworks have their own ways of configuring proxies:

- **Node.js:** Use the [global-agent](./use-dev-proxy-with-nodejs.md) package or configure the HTTP agent directly
- **.NET:** Set the `HTTPS_PROXY` environment variable or configure `HttpClient` with a proxy (see [Use Dev Proxy with .NET applications](./use-dev-proxy-with-dotnet.md))
- **Azure Functions:** Configure proxy in `local.settings.json` (see [Use Dev Proxy with .NET Azure Functions](./use-dev-proxy-with-dotnet-azure-functions.md))

## Example: Test Azure Functions without interfering with startup

Azure Functions uses gRPC for internal communication, which fails when Dev Proxy is registered as the system proxy. To use Dev Proxy with Azure Functions, disable system proxy registration and configure the proxy through environment variables.

**File:** `devproxyrc.json`

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.2.0/rc.schema.json",
  "asSystemProxy": false,
  "urlsToWatch": [
    "https://jsonplaceholder.typicode.com/*"
  ]
}
```

**File:** `local.settings.json`

```json
{
  "IsEncrypted": false,
  "Values": {
    "HTTPS_PROXY": "http://127.0.0.1:8000"
  }
}
```

With this configuration, Azure Functions starts normally while your HTTP requests to the watched URLs are routed through Dev Proxy.

## Example: Test one application while keeping others unaffected

When you're developing multiple applications simultaneously, you might want to use Dev Proxy with only one of them. Disable system proxy registration and set the environment variable only for the target application.

Start Dev Proxy without system proxy registration:

```console
devproxy --as-system-proxy false
```

In a separate terminal, run your application with the proxy configured:

::: zone pivot="client-operating-system-windows"

```powershell
$env:HTTPS_PROXY = "http://127.0.0.1:8000"
npm start
```

::: zone-end

::: zone pivot="client-operating-system-macos,client-operating-system-linux"

```console
HTTPS_PROXY=http://127.0.0.1:8000 npm start
```

::: zone-end

Other applications on your machine continue to work normally without any proxy interference.

## See also

- [Proxy settings](../technical-reference/proxy-settings.md) - All configuration options
- [Use Dev Proxy with Node.js applications](./use-dev-proxy-with-nodejs.md)
- [Use Dev Proxy with .NET applications](./use-dev-proxy-with-dotnet.md)
- [Use Dev Proxy with .NET Azure Functions](./use-dev-proxy-with-dotnet-azure-functions.md)
- [Dev Proxy doesn't intercept requests](./no-requests-intercepted.md)
