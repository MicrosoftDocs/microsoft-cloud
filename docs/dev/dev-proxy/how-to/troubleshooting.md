---
title: Troubleshoot Dev Proxy
description: Solutions for common Dev Proxy issues
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/27/2026
ms.topic: how-to
---

<!-- INTENT: Find solutions to common Dev Proxy problems -->
<!-- SOLUTION: Browse symptom-based troubleshooting guides -->
<!-- RESULT: Problem resolved with step-by-step diagnostics -->
<!-- PLUGINS: none -->
<!-- JOB: troubleshoot -->

# Troubleshoot Dev Proxy

Use this guide to diagnose and fix common Dev Proxy issues. Start with your symptom and work through the diagnostic questions to find the solution.

## Symptom: Dev Proxy isn't intercepting requests

You started Dev Proxy, but your app's requests aren't showing up in the Dev Proxy output. Work through these questions to identify the cause.

### Is Dev Proxy running?

Check that Dev Proxy is running and ready to intercept requests.

**What to look for:**

After starting Dev Proxy, you should see output similar to:

```text
 info    Dev Proxy API listening on http://127.0.0.1:8897...
 info    Dev Proxy listening on 127.0.0.1:8000...

Hotkeys: issue (w)eb request, (r)ecord, (s)top recording, (c)lear screen
Press CTRL+C to stop Dev Proxy
```

**If you don't see this output:**

- Check that Dev Proxy is installed correctly by running `devproxy --version`
- If you get an error, [reinstall Dev Proxy](../get-started/set-up.md)

### Is Dev Proxy registered as the system proxy?

Dev Proxy must be registered as the system proxy to intercept requests automatically. If it's not, your app's requests bypass Dev Proxy entirely.

**How to check on macOS:**

1. Open **System Settings** > **Network**
1. Select your active network connection
1. Go to **Details...** > **Proxies**
1. Look for **Secure HTTP Proxy (HTTPS)** set to `127.0.0.1:8000`

**How to check on Windows:**

1. Open **Settings** > **Network & Internet** > **Proxy**
1. Under **Manual proxy setup**, check that the proxy server is set to `127.0.0.1:8000`

**If Dev Proxy isn't registered as system proxy:**

Check your Dev Proxy configuration. Run `devproxy config` to open the config file and verify it doesn't contain:

```json
{
  "asSystemProxy": false
}
```

If this setting exists and is set to `false`, remove it or set it to `true`, then restart Dev Proxy.

### Are you watching the correct URLs?

Dev Proxy only intercepts requests to URLs that match patterns in the `urlsToWatch` configuration.

**How to check:**

1. Look at the Dev Proxy startup output for the list of URLs being watched
1. Compare them with the URL your app is calling

**Common issues:**

- **Missing wildcards**: `https://api.example.com` doesn't match `https://api.example.com/users`. Use `https://api.example.com/*` instead.
- **Wrong domain**: Double-check the domain name, including subdomains
- **HTTP vs HTTPS**: Make sure the protocol matches

**To test your URL pattern:**

```console
devproxy --urls-to-watch "https://your-api.com/*"
```

Then make a request to your API and check if Dev Proxy logs it.

### Are you using the correct configuration file?

Dev Proxy might be using a different configuration file than you expect.

**How Dev Proxy finds configuration files:**

1. If you specify `--config-file`, Dev Proxy uses that file
1. Otherwise, Dev Proxy looks for `devproxyrc.json` in the current directory
1. If not found, Dev Proxy uses default settings

**To verify which config file is being used:**

Run Dev Proxy with debug logging:

```console
devproxy --log-level debug
```

Look for output that shows which configuration file is loaded.

**To explicitly specify a config file:**

```console
devproxy --config-file ./my-config.json
```

### Are you using Dev Proxy with a Node.js application?

Node.js doesn't automatically use system proxy settings. You need to configure your Node.js app to use the proxy explicitly.

**Solution:**

Use the `global-agent` package or configure your HTTP library to use the proxy. See [Use Dev Proxy with Node.js applications](use-dev-proxy-with-nodejs.md) for detailed instructions.

**Quick fix with global-agent:**

1. Install global-agent:

   ```console
   npm install global-agent
   ```

1. Add to your app's entry point:

   ```javascript
   import { bootstrap } from 'global-agent';
   bootstrap();
   ```

1. Start your app with proxy environment variables:

   ```console
   NODE_TLS_REJECT_UNAUTHORIZED=0 GLOBAL_AGENT_HTTP_PROXY=http://127.0.0.1:8000 node app.js
   ```

### Are you using Dev Proxy with PowerShell on Windows?

PowerShell doesn't automatically use system proxy settings for web requests made with `Invoke-WebRequest` or `Invoke-RestMethod`.

**Solution:**

Configure PowerShell to use the proxy by setting the `-Proxy` parameter:

```powershell
Invoke-WebRequest -Uri "https://api.example.com/data" -Proxy "http://127.0.0.1:8000"
```

Or set the proxy for the entire session:

```powershell
$env:HTTPS_PROXY = "http://127.0.0.1:8000"
$env:HTTP_PROXY = "http://127.0.0.1:8000"
```

If you're using PowerShell 7+, you can also use:

```powershell
[System.Net.WebRequest]::DefaultWebProxy = New-Object System.Net.WebProxy("http://127.0.0.1:8000")
```

### Other platforms and frameworks

If you're using other platforms, check these guides:

| Platform | Guide |
|----------|-------|
| .NET applications | [Use Dev Proxy with .NET applications](use-dev-proxy-with-dotnet.md) |
| .NET 4.8 applications | [.NET 4.8 apps require special configuration](why-is-proxy-not-intercepting-requests-from-my-dotnet-4-8-app.md) |
| Docker containers | [Use Dev Proxy in a Docker container](use-dev-proxy-in-docker-container.md) |
| SharePoint Framework | [Use Dev Proxy with SPFx](use-dev-proxy-with-spfx.md) |

## Symptom: Requests are intercepted but passed through unchanged

Dev Proxy shows your requests in the output, but they're not being modified, mocked, or returning simulated errors.

### Are your plugins enabled?

Check that the plugins you want to use are enabled in your configuration.

**How to check:**

Open your configuration file and verify that:

1. The plugin is listed in the `plugins` array
1. The plugin has `"enabled": true`

```json
{
  "plugins": [
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ]
}
```

### Are your plugins in the correct order?

Plugin order matters in Dev Proxy. Plugins execute in the order they're listed, and some plugins might stop processing before others run.

**Common issue:**

If you have `MockResponsePlugin` before `GenericRandomErrorPlugin`, and a mock matches your request, the random error plugin never runs.

**Solution:**

Arrange plugins in the order you want them to process requests. For more information, see [Why are random errors not thrown when using mocks?](why-are-random-errors-not-thrown-when-using-mocks.md).

### Does your mock URL pattern match the request?

For mock responses, the URL pattern must exactly match the request URL.

**Things to check:**

- Query string parameters: `https://api.example.com/users?id=1` doesn't match `https://api.example.com/users`
- Trailing slashes: `https://api.example.com/users/` doesn't match `https://api.example.com/users`
- Case sensitivity: URL paths are case-sensitive

### Is the request method correct?

Mocks are method-specific. A mock for `GET` requests doesn't match `POST` requests.

**How to check:**

In your mocks file, verify the `method` property matches your request:

```json
{
  "request": {
    "url": "https://api.example.com/users",
    "method": "GET"
  },
  "response": {
    "statusCode": 200,
    "body": { "users": [] }
  }
}
```

## Symptom: SSL/certificate errors in your app

Your app throws SSL or certificate errors when trying to make requests through Dev Proxy.

### Do you have the Dev Proxy certificate installed?

Dev Proxy uses a self-signed certificate to decrypt HTTPS traffic. Your system must trust this certificate.

**Solution:**

Run the certificate installation command:

```console
devproxy cert ensure
```

This command installs and trusts the Dev Proxy certificate on your system.

### Is the certificate trusted?

The certificate might be installed but not trusted.

**How to check on macOS:**

1. Open **Keychain Access**
1. Search for "Dev Proxy"
1. Double-click the certificate
1. Expand **Trust**
1. Verify that **Secure Sockets Layer (SSL)** is set to **Always Trust**

**How to check on Windows:**

1. Run `certmgr.msc`
1. Navigate to **Trusted Root Certification Authorities** > **Certificates**
1. Look for the Dev Proxy certificate

**If the certificate isn't trusted:**

Remove and reinstall the certificate:

```console
devproxy cert remove
devproxy cert ensure
```

### Are you using Node.js?

Node.js doesn't use the system certificate store by default. You need to either:

**Option 1: Disable certificate validation (development only):**

```console
NODE_TLS_REJECT_UNAUTHORIZED=0 node app.js
```

> [!WARNING]
> Never use `NODE_TLS_REJECT_UNAUTHORIZED=0` in production. It disables all certificate validation.

**Option 2: Add the Dev Proxy certificate to Node.js:**

Export the Dev Proxy certificate and set the `NODE_EXTRA_CA_CERTS` environment variable:

```console
NODE_EXTRA_CA_CERTS=/path/to/devproxy-cert.pem node app.js
```

### Are you using a different runtime or framework?

Different platforms handle certificates differently:

| Platform | Solution |
|----------|----------|
| Python | Use `REQUESTS_CA_BUNDLE` or `SSL_CERT_FILE` environment variables |
| Java | Import certificate into Java keystore using `keytool` |
| .NET | Certificate is typically trusted via system store |
| Docker | Mount the certificate and update CA certificates in the container |

## Other common issues

### All requests fail with gateway timeout

**Cause:**

Dev Proxy can't reach the target API.

**Solution:**

See [Why do all requests fail with gateway timeout?](why-do-all-requests-fail-with-gateway-timeout.md)

### No internet connection after using Dev Proxy

**Cause:**

Dev Proxy doesn't properly deregister as system proxy.

**Solution:**

See [Why is my internet connection not working after using the proxy?](why-is-my-internet-connection-not-working-after-using-the-proxy.md)

### Getting 429 responses unexpectedly

**Cause:**

Rate limiting plugins are enabled and configured.

**Solution:**

See [Why do I keep getting 429 responses?](why-do-i-keep-getting-429-responses.md)

### Database errors

| Error | Solution |
|-------|----------|
| SqliteConnection initialization error | [Fix SQLite configuration](type-initializer-microsoft-data-sqlite-sqliteconnection-threw-exception.md) |
| "Database disk image is malformed" | [Rebuild the database](sqlite-error-11-database-disk-image-is-malformed.md) |

## Get more help

If you can't find a solution:

1. **Enable debug logging**: Run `devproxy --log-level debug` for detailed output
1. **Search existing issues**: [Dev Proxy GitHub issues](https://github.com/microsoft/dev-proxy/issues)
1. **Ask for help**: [Get help and support](get-help-and-support.md)

## See also

- [Set up Dev Proxy](../get-started/set-up.md)
- [Configuration reference](../technical-reference/devproxyrc.md)
- [Plugin architecture](../technical-reference/plugin-architecture.md)
