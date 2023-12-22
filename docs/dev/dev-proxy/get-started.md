---
title: Get started with Dev Proxy
description: Learn how to install and use Dev Proxy.
author: garrytrinder
ms.author: garrytrinder
ms.date: 12/08/2023
ms.topic: get-started
zone_pivot_groups: client-operating-system
#Customer intent: As a developer, I want to test the resilience of my application so that I can understand how my application reacts to cloud API failures.
---

# Get started with Dev Proxy

Dev Proxy is a command line tool that helps you simulate behaviors and errors of cloud APIs to help you build resilient apps.

In this article, you'll learn how to install and run Dev Proxy.

## Install

### PowerShell (#tab/powershell)

```powershell
(Invoke-webrequest https://aka.ms/devproxysetup.ps1).Content | Invoke-Expression
```

### Bash (#tab/bash)

```bash
curl -sL https://aka.ms/devproxy/setup.sh | bash
```

## Start Dev Proxy

::: zone pivot="client-operating-system-windows"

1. **Start Dev Proxy**. Open a terminal. Enter `devproxy` and press <kbd>Enter</kbd>.
2. **Trust certificate**. Dev Proxy installs a certificate named `Dev Proxy CA`. A warning shows. Select `Yes` to confirm that you want to install the certificate. Dev Proxy uses this certificate to decrypt HTTPS traffic sent from your machine.
3. **Allow firewall access**. Windows Firewall blocks the proxy. A warning shows. Select `Allow access` button to allow traffic through the firewall.

> [!CAUTION]
> If you're using the proxy with a .NET 4.8 app, you will also need to [register Dev Proxy on your system](./how-to/why-is-proxy-not-intercepting-requests-from-my-dotnet-4-8-app.md) using `netsh`.

The terminal displays the following output:

```text
  WARNING: File responses.json not found in the current directory. No mocks will be provided
Listening on 127.0.0.1: 8000
Set endpoint at Ip 127.0.0.1 and port: 8000 as System HTTP Proxy
Set endpoint at Ip 127.0.0.1 and port: 8000 as System HTTPS Proxy
Press Ctrl+C to stop the Dev Proxy
```

> [!NOTE]
> You won't have to repeat steps 2 and 3 after the first run.

::: zone-end  

::: zone pivot="client-operating-system-macos"

1. **Make files executable**. Open the `devproxy` installation folder in a terminal. Execute `chmod -x devproxy` and then `chmod -x libe_sqlite3.dylib`
1. **Trust the application**. macOS includes a security technology named [Gatekeeper](https://support.apple.com/en-gb/guide/security/sec5599b66df/web), which is designed to help ensure that only trusted software runs on a user’s Mac. The current release isn't signed by a verified developer, so you'll need to trust it manually.
    1. Open the `dev-proxy` installation folder in Finder.
    1. Press <kbd>Ctrl</kbd> and select the `devproxy` executable.
    1. Choose `Open` from the menu, and then select `Open` in the dialog that appears.
    1. Enter your admin name and password to open the app.
    1. The proxy starts in a terminal window.
1. **Accept incoming connections**. A warning shows. Select `Allow` to confirm.
1. **Trust the certificate**. The proxy installs a certificate named `Titanium Root Certificate Authority`.
    1. Open `Keychain Access`.
    1. Enter `Titanium Root Certificate Authority` in the search box.
    1. Open the certificate shown by double clicking it.
    1. Expand the `Trust` section.
    1. Change the `When using this certificate:` dropdown to `Always Trust`.
    1. Close the certificate window and confirm changes.
    1. Enter your admin name and password to confirm your settings.
1. **Configure your network device**.
    1. Open `Network` settings.
    1. Select the device to configure and select the `Advanced...` button.
    1. Go to the `Proxies` tab.
    1. Check `Secure Web Proxy (HTTPS)` in the list of configurable proxies.
    1. Enter `127.0.0.1`:`8000` in the `Secure Web Proxy Server` field.
    1. Select `OK` and then `Apply` to confirm the changes.

The terminal displays the following output:

```text
  WARNING: File responses.json not found in the current directory. No mocks will be provided
Listening on 127.0.0.1:8000...
  WARNING: Configure your operating system to use this proxy's port and address
Press CTRL+C to stop Dev Proxy
```

> [!NOTE]
> You won't need to repeat steps 1-4 after the first run.

::: zone-end

---

The proxy is now running with the following defaults:

- 50% chance of a request being failed with a random [supported HTTP error status code](./technical-reference/Supported-HTTP-error-status-codes.md).
- Dev Proxy intercepts all requests to Microsoft Graph and SharePoint Online APIs.
- Proxy doesn't mock any requests.

## Stop Dev Proxy safely

When you no longer require Dev Proxy to be running, you should always stop it safely.

Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to safely stop Dev Proxy.

If you shut down the terminal session, Dev Proxy doesn't unregister correctly, and you might experience some [common problems](./how-to/overview.md#common-problems).

::: zone pivot="client-operating-system-macos"
You should also disable the `Secure Web Proxy (HTTPS)` proxy setting on your network device.
::: zone-end

## Test from command line

To test that the proxy is set up correctly, stop the proxy if it's still running.

Start the proxy with the following options:

```sh
devproxy --no-mocks --allowed-errors 429 --failure-rate 100
```

The proxy fails all requests with a 429 error response and doesn't mock requests.

From the command line, issue an HTTP request to the Microsoft Graph.

Examples:

### [PowerShell](#tab/pwsh)

```pwsh
Invoke-WebRequest -Uri "https://graph.microsoft.com/v1.0/me"
```

### [curl](#tab/curl)

```sh
curl -ix http://localhost:8000 https://graph.microsoft.com/v1.0/me
```

---

The proxy shows the below output:

```text
 request     GET https://graph.microsoft.com/v1.0/me
 warning   ╭ To help Microsoft investigate errors, to each request to Microsoft Graph
           │ add the client-request-id header with a unique GUID.
           │ More info at https://aka.ms/devproxy/guidance/client-request-id
           ╰ GET https://graph.microsoft.com/v1.0/me
     tip   ╭ To more easily follow best practices for working with Microsoft Graph,
           │ use the Microsoft Graph SDK.
           │ More info at https://aka.ms/devproxy/guidance/move-to-js-sdk
           ╰ GET https://graph.microsoft.com/v1.0/me
   chaos   ╭ 429 TooManyRequests
           ╰ GET https://graph.microsoft.com/v1.0/me
 warning   ╭ To improve performance of your application, use the $select parameter.
           │ More info at https://aka.ms/devproxy/guidance/select
           ╰ GET https://graph.microsoft.com/v1.0/me
     tip   ╭ To handle API errors more easily, use the Microsoft Graph SDK.
           │ More info at https://aka.ms/devproxy/guidance/move-to-js-sdk
           ╰ GET https://graph.microsoft.com/v1.0/me
```

Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to safely stop Dev Proxy.

Dev Proxy has:

1. Intercepted a request made to Microsoft Graph.
1. Issued a chaos response with HTTP error status code, `429 Too Many Requests`.
1. Generated a warning that the request should include the `client-request-id` header.
1. Generated a warning that the request didn't include the OData `$select` parameter.
1. Generated a tip suggesting the request wasn't made using a Microsoft Graph SDK.

## Intercept requests to any API

Dev Proxy can issue error responses for any API. To add your own API, change the contents of the `devproxyrc.json` file.

1. Open the `devproxy` folder to find the `devproxyrc.json` file.
1. Open the `devproxyrc.json` file in a text editor and locate the `urlsToWatch` array in the root object (not in the plugins array).
1. Add a new entry containing the URL of the API you want to use the proxy with.

For example, if your API has the URL, `https://myapp/api/foo`, your `devproxyrc.json` file looks something like:

```json
{
  "plugins": [ ... ],
  "urlsToWatch": [
    "https://myapp/api/*"
    "https://graph.microsoft.com/v1.0/*",
    "https://graph.microsoft.com/beta/*",
    "https://graph.microsoft.us/v1.0/*",
    "https://graph.microsoft.us/beta/*",
    "https://dod-graph.microsoft.us/v1.0/*",
    "https://dod-graph.microsoft.us/beta/*",
    "https://microsoftgraph.chinacloudapi.cn/v1.0/*",
    "https://microsoftgraph.chinacloudapi.cn/beta/*",
    "https://*.sharepoint.*/*_api/*",
    "https://*.sharepoint.*/*_vti_bin/*",
    "https://*.sharepoint-df.*/*_api/*",
    "https://*.sharepoint-df.*/*_vti_bin/*"
  ],
  "mocksPlugin": { ... },
  "randomErrorsPlugin": { ... },
  "labelMode": "text",
  "logLevel": "info"
}
```

Stop and start the proxy for the change to take effect. Issue a network request from the command line to the API. The proxy will now intercept requests sent to your API.

## Get help

Display the different command-line options by using:

```text
devproxy --help
```

If you do run into any difficulties, don’t hesitate to contact us by raising a [new issue](https://github.com/microsoft/dev-proxy/issues/new) and we're glad to help you out.

## Next step

> [!div class="nextstepaction"]
> [Explore tutorials](./tutorials/test-a-javascript-client-side-web-application.md)
