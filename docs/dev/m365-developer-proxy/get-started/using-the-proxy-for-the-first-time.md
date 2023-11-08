---
title: Using the proxy for the first time
description: How to configure the Microsoft 365 Developer Proxy for the first time
author: garrytrinder
ms.author: garrytrinder
ms.contributors: garrytrinder
ms.date: 11/03/2023
ms.topic: conceptual
ms.service: microsoft-cloud-for-developers

categories:
  - developer-tools
products:
  - microsoft-365
  - microsoft-graph
  - sharepoint-online
  - m365
ms.custom:
  - fcp
  - team=cloud_advocates
---

This step-by-step introduction is intended for beginners.

> [!CAUTION]
> It assumes that you completed the installation steps. If not, do that [now](./installation.md).

You learn how to use the Microsoft 365 Developer Proxy for the first time.

## 1. Start the proxy

Depending on your operating system, you have a different first run experience.

Select the relevant section and follow the steps.

### [Windows](#tab/windows)

1. **Start the proxy**. Open a terminal. Enter `m365proxy` and press <kbd>Enter</kbd>.
2. **Trust certificate**. The proxy installs a certificate called `Titanium Root Certificate Authority`. A warning shows. Select `Yes` to confirm that you want to install the certificate. The proxy uses this certificate to decrypt HTTPS traffic sent from your machine.
3. **Allow firewall access**. Windows Firewall blocks the proxy. A warning shows. Select `Allow access` button to allow traffic through the firewall.

> [!CAUTION]
> If you are using the proxy with a .NET 4.8 app, you will also need to [configure the proxy](microsoft-cloud/dev/m365-developer-proxy/how-to/why-is-proxy-not-intercepting-requests-from-my-.net-4.8-app) using `netsh`.

The terminal displays the following output:

```text
  WARNING: File responses.json not found in the current directory. No mocks will be provided
Listening on 0.0.0.0: 8000
Set endpoint at Ip 0.0.0.0 and port: 8000 as System HTTP Proxy
Set endpoint at Ip 0.0.0.0 and port: 8000 as System HTTPS Proxy
Press Ctrl + C to stop the Microsoft 365 Developer Proxy
```

> [!NOTE]
> Steps 2 & 3 will not need to be repeated after the first run.

### [macOS](#tab/macos)

1. **Make files executables**. Open the `m365-developer-proxy` installation folder in a terminal. Execute `chmod -x m365proxy` and then `chmod -x libe_sqlite3.dylib`
1. **Trust the application**. macOS includes a security technology called [Gatekeeper](https://support.apple.com/en-gb/guide/security/sec5599b66df/web), which is designed to help ensure that only trusted software runs on a user’s Mac. As the current release isn't signed by a verified developer, you need to trust it. Open the `m365-developer-proxy` installation folder in Finder. Press <kbd>Ctrl</kbd> and select the `m365proxy` executable. Choose `Open` from the menu, and then select `Open` in the dialog that appears. Enter your admin name and password to open the app. The proxy starts in a terminal window.
1. **Accept incoming connections**. A warning shows. Select `Allow` to confirm.
1. **Trust the certificate**. The proxy installs a certificate called `Titanium Root Certificate Authority`. Open `Keychain Access`. Enter `Titanium Root Certificate Authority` in the search box. Open the certificate shown by double clicking it. Expand the `Trust` section. Change the `When using this certificate:` dropdown to `Always Trust`. Close the certificate window and confirm changes. Enter your admin name and password to confirm your settings.
1. **Configure your network device**. Open `Network` settings. Select the device to configure and select the `Advanced...` button. Go to the `Proxies` tab. Check `Secure Web Proxy (HTTPS)` in the list of configurable proxies. Enter `0.0.0.0`:`8000` in the `Secure Web Proxy Server` field. Select `OK` and then `Apply` to confirm the changes.

The terminal displays the following output:

```text
  WARNING: File responses.json not found in the current directory. No mocks will be provided
Listening on 0.0.0.0:8000...
  WARNING: Configure your operating system to use this proxy's port and address
Press CTRL+C to stop the Microsoft 365 Developer Proxy
```

> [!NOTE]
> Steps 1-3 will not need to be repeated after the first run.

The proxy is now running with the following defaults:

- 50% chance of a request being failed with a random [supported HTTP error status code](./Supported-HTTP-error-status-codes).
- All requests sent to Microsoft Graph and SharePoint Online APIs are intercepted.
- No requests are mocked.

## 2. Stop the proxy safely

When you no longer require the proxy to be running, you should always stop the proxy safely.

Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to safely stop the proxy.

If you shut down the terminal session, the proxy doesn't unregister correctly, and you might experience some [common problems](microsoft-cloud/dev/m365-developer-proxy/how-to).

If you're using macOS, you should also disable the `Secure Web Proxy (HTTPS)` proxy on your network device.

## 3. Test from command line

To test that the proxy is set up correctly, stop the proxy if it's still running.

Start the proxy with the following options:

```sh
m365proxy --no-mocks --allowed-errors 429 --failure-rate 100
```

The proxy fails all requests with a 429 error response and disable mocks.

From the command line, issue an HTTP request to the Microsoft Graph.

Examples:

### [PowerShell](#tab/pwsh)

```pwsh
Invoke-RestMethod -Uri "https://graph.microsoft.com/v1.0/me" -Proxy "http://localhost:8000"
```

</details>

### [curl](#tab/curl)

```sh
curl -ikx http://localhost:8000 https://graph.microsoft.com/v1.0/me
```

The proxy shows the below output:

```text
[ REQUEST ]    GET https://graph.microsoft.com/v1.0/me
[  CHAOS  ]  ┌ 429 TooManyRequests
             └ GET https://graph.microsoft.com/v1.0/me
[ WARNING ]  ┌ To improve performance of your application, use the $select parameter.
             │ More info at https://aka.ms/graph/proxy/guidance/select
             └ GET https://graph.microsoft.com/v1.0/me
[   TIP   ]  ┌ To handle API errors more easily, use the Graph SDK.
             │ More info at https://aka.ms/graph/proxy/guidance/move-to-js-sdk
             └ GET https://graph.microsoft.com/v1.0/me
```

The proxy has:

1. Intercepted a request made to Microsoft Graph.
1. Issued a chaos response with HTTP error status code, `429 Too Many Requests`.
1. Generated a warning as the request didn't include the OData `$select` parameter.
1. Generated a tip as the request wasn't made using a Microsoft Graph SDK.

## 4. Intercept requests to any API

The proxy can issue error responses for any API. To add your own API, change the contents of the `m365proxyrc.json` file.

1. Open the `m365proxy` folder to find the `m365proxyrc.json` file.
1. Open the `m365proxyrc.json` in a text editor and locate the `urlsToWatch` array in the root object (not in the plugins array).
1. Add a new entry containing the URL of the API you want to use the proxy with.

For example, if your API has the URL, `https://myapp/api`, your `m365proxyrc.json` file looks something like:

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

> [!NOTE]
> By default, the proxy simulates error responses based on Microsoft Graph. To add your own responses, follow this [guide](microsoft-cloud/dev/m365-developer-proxy/how-to/simulate-errors-from-non-Microsoft-365-APIs).

## 5. Show help and usage information

Display help to change [proxy settings](microsoft-cloud/dev/m365-developer-proxy/technical-reference/proxy-settings) using:

```text
m365proxy --help
```

## On to the next step

Now you’re ready to go on to the next step.

Take a look at our [How-to](microsoft-cloud/dev/m365-developer-proxy/how-to) guides to learn more about how to configure the proxy.

Or, try our [sample JavaScript client-side web application](microsoft-cloud/dev/m365-developer-proxy/get-started/test-a-sample-app-with-microsoft-365-developer-proxy) with the proxy.
