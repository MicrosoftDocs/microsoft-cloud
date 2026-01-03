---
title: Troubleshooting Dev Proxy
description: Solutions to common problems when using Dev Proxy
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/03/2026
ms.topic: troubleshooting
---

<!-- INTENT: Find solutions to common Dev Proxy problems -->
<!-- SOLUTION: Browse categorized troubleshooting guides -->
<!-- RESULT: Problem resolved with step-by-step fix -->
<!-- PLUGINS: none -->
<!-- JOB: troubleshoot -->

# Troubleshooting Dev Proxy

Use this guide to diagnose and fix common issues with Dev Proxy.

## Quick diagnostics

Before diving into specific issues, try these steps:

1. **Check Dev Proxy is running**: Look for `Listening on 127.0.0.1:8000...` in the terminal
2. **Verify your URL pattern**: Run `devproxy --urls-to-watch "https://your-api.com/*"` and check the output
3. **Test with a simple request**: Use `curl` or your browser to make a request and watch Dev Proxy output

## Requests not being intercepted

| Problem | Solution |
|---------|----------|
| Dev Proxy shows no output | [Dev Proxy doesn't intercept requests](no-requests-intercepted.md) |
| .NET 4.8 app requests not captured | [.NET 4.8 apps require special configuration](why-is-proxy-not-intercepting-requests-from-my-dotnet-4-8-app.md) |
| Node.js requests not captured | [Configure Node.js to use the proxy](use-dev-proxy-with-nodejs.md) |
| Docker container requests not captured | [Configure Docker networking](use-dev-proxy-in-docker-container.md) |

## Connection and network issues

| Problem | Solution |
|---------|----------|
| No internet after using Dev Proxy | [Reset system proxy settings](why-is-my-internet-connection-not-working-after-using-the-proxy.md) |
| All requests fail with gateway timeout | [Check proxy connectivity](why-do-all-requests-fail-with-gateway-timeout.md) |
| Certificate trust errors | Run `devproxy cert ensure` to reinstall the certificate |

## Mock and error simulation issues

| Problem | Solution |
|---------|----------|
| Mocks not being returned | Check URL patterns match exactly, including query strings |
| Random errors not thrown with mocks | [Understand plugin ordering](why-are-random-errors-not-thrown-when-using-mocks.md) |
| Binary responses not mocked | [Configure binary response handling](why-is-proxy-not-mocking-my-binary-response.md) |
| Getting 429 responses unexpectedly | [Check rate limiting configuration](why-do-i-keep-getting-429-responses.md) |

## Configuration issues

| Problem | Solution |
|---------|----------|
| "Unknown option" error | [Update Dev Proxy or check option spelling](why-is-the-proxy-saying-that-an-option-is-unknown.md) |
| Config file not found | Use `--config-file path/to/config.json` or ensure `devproxyrc.json` exists |
| Plugin not loading | Verify `pluginPath` points to correct DLL location |

## Database and storage issues

| Problem | Solution |
|---------|----------|
| SqliteConnection initialization error | [Fix SQLite configuration](type-initializer-microsoft-data-sqlite-sqliteconnection-threw-exception.md) |
| "Database disk image is malformed" | [Rebuild the database](sqlite-error-11-database-disk-image-is-malformed.md) |
| Microsoft Graph database outdated | Run `devproxy msgraphdb` to refresh |

## Still stuck?

If you can't find a solution here:

1. **Check the logs**: Run with `--log-level debug` for detailed output
2. **Search GitHub Issues**: [Dev Proxy issues](https://github.com/microsoft/dev-proxy/issues)
3. **Get help**: [Get help and support](get-help-and-support.md)

## See also

- [Set up Dev Proxy](../get-started/set-up.md) - Installation and first run
- [Configuration reference](../technical-reference/devproxyrc.md) - All configuration options
- [Plugin architecture](../technical-reference/plugin-architecture.md) - How plugins work
