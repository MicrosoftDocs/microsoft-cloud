---
title: Use Dev Proxy with a browser
description: Learn how to attach Dev Proxy directly to a browser instance without changing system proxy settings
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/26/2026
ms.topic: how-to
zone_pivot_groups: client-operating-system
---

<!-- INTENT: Launch browser with proxy attached directly -->
<!-- SOLUTION: Use browser command-line flags or manual proxy settings -->
<!-- RESULT: Intercept browser traffic without affecting system proxy -->
<!-- PLUGINS: none -->
<!-- JOB: intercept-requests -->
<!-- TIME: 5 minutes -->

# Use Dev Proxy with a browser

> **At a glance**  
> **Goal:** Attach Dev Proxy to a browser instance without changing system proxy settings  
> **Time:** 5 minutes  
> **Plugins:** None  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

By default, Dev Proxy registers itself as the system proxy, which routes all network traffic through the proxy. While this approach works for most scenarios, sometimes you want to intercept only traffic from a specific browser instance while keeping other applications unaffected.

## Why use a browser-specific proxy?

Attaching Dev Proxy directly to a browser instance has several benefits:

- **Isolation**: Only the specific browser instance uses the proxy, leaving other apps and browsers unaffected
- **No system changes**: You don't need to modify system proxy settings
- **Parallel testing**: Run multiple browser instances with different proxy configurations
- **Cleaner traffic**: See only the requests from the browser you're testing, not background system traffic

## Prerequisites

Before you start, configure Dev Proxy to not register as the system proxy. In your `devproxyrc.json` file, set:

```json
{
  "asSystemProxy": false
}
```

Or, start Dev Proxy with the `--as-system-proxy false` command-line option.

## Google Chrome

Google Chrome supports proxy configuration via command-line flags. To launch Chrome with Dev Proxy:

::: zone pivot="client-operating-system-windows"

```console
"C:\Program Files\Google\Chrome\Application\chrome.exe" --proxy-server="http://127.0.0.1:8000"
```

::: zone-end

::: zone pivot="client-operating-system-macos"

```console
/Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --proxy-server="http://127.0.0.1:8000"
```

::: zone-end

::: zone pivot="client-operating-system-linux"

```console
google-chrome --proxy-server="http://127.0.0.1:8000"
```

::: zone-end

> [!TIP]
> Use a separate user profile to avoid affecting your main browser data. Add the `--user-data-dir` flag to specify a different profile directory:
>
> ::: zone pivot="client-operating-system-windows"
>
> ```console
> "C:\Program Files\Google\Chrome\Application\chrome.exe" --proxy-server="http://127.0.0.1:8000" --user-data-dir="%TEMP%\chrome-dev-proxy"
> ```
>
> ::: zone-end
>
> ::: zone pivot="client-operating-system-macos"
>
> ```console
> /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --proxy-server="http://127.0.0.1:8000" --user-data-dir="/tmp/chrome-dev-proxy"
> ```
>
> ::: zone-end
>
> ::: zone pivot="client-operating-system-linux"
>
> ```console
> google-chrome --proxy-server="http://127.0.0.1:8000" --user-data-dir="/tmp/chrome-dev-proxy"
> ```
>
> ::: zone-end

> [!IMPORTANT]
> Close all existing Chrome instances before launching Chrome with the proxy flag. Otherwise, the new instance joins the existing Chrome process and ignores the proxy settings.

## Microsoft Edge

Microsoft Edge doesn't support the `--proxy-server` command-line flag. Even though Microsoft Edge is based on Chromium, Microsoft doesn't expose this functionality.

To use Dev Proxy with Microsoft Edge, you must use the system proxy settings. Configure Dev Proxy as your system proxy by keeping the `asSystemProxy` setting at its default value of `true`, or omitting it from your configuration.

## Mozilla Firefox

Firefox doesn't support proxy configuration via command-line flags, but you can configure it manually through the browser settings.

To configure Firefox to use Dev Proxy:

1. Open Firefox
1. Go to **Settings** > **General** > **Network Settings** > **Settings...**
1. Select **Manual proxy configuration**
1. Set **HTTP Proxy** to `127.0.0.1` and **Port** to `8000`
1. Check **Also use this proxy for HTTPS**
1. Select **OK**

> [!TIP]
> Create a separate Firefox profile for testing with Dev Proxy. This way, you can keep your regular browsing profile unchanged. To create a new profile, run `firefox -P` and create a new profile dedicated to Dev Proxy testing.

## Trust the Dev Proxy certificate

When you first start Dev Proxy, it installs and trusts a root certificate to decrypt HTTPS traffic. If you're using a separate browser profile or if the browser doesn't use the system certificate store, you might need to manually trust the certificate.

### Chrome and Microsoft Edge

Chrome and Microsoft Edge use the operating system's certificate store. If you've already run Dev Proxy and trusted the certificate during the first-run experience, Chrome and Microsoft Edge automatically trust it.

### Firefox

Firefox uses its own certificate store. To trust the Dev Proxy certificate in Firefox:

1. Open Firefox
1. Go to **Settings** > **Privacy & Security** > **Certificates** > **View Certificates...**
1. Select the **Authorities** tab
1. Select **Import...**
1. Navigate to the Dev Proxy certificate:
   ::: zone pivot="client-operating-system-windows"
   - Location: `%USERPROFILE%\.config\dev-proxy\rootCert.pfx`
   ::: zone-end
   ::: zone pivot="client-operating-system-macos"
   - Location: `~/.config/dev-proxy/rootCert.pfx`
   ::: zone-end
   ::: zone pivot="client-operating-system-linux"
   - Location: `~/.config/dev-proxy/rootCert.pfx`
   ::: zone-end
1. Check **Trust this CA to identify websites**
1. Select **OK**

> [!NOTE]
> The certificate password is empty. Leave the password field blank when prompted.

## See also

- [Configure Dev Proxy](../get-started/configure.md)
- [Intercept requests to localhost](./intercept-localhost-requests.md)
- [Dev Proxy doesn't intercept requests](./no-requests-intercepted.md)
