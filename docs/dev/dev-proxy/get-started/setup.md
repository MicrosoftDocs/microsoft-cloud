---
title: Setup Dev Proxy
description: Learn how to install and run Dev Proxy.
author: garrytrinder
ms.author: garrytrinder
ms.date: 02/03/2025
ms.topic: get-started
zone_pivot_groups: client-operating-system
#Customer intent: As a developer, I want to test the resilience of my application so that I can understand how my application reacts to cloud API failures.
---

# Setup Dev Proxy

Dev Proxy is a command line tool that helps you simulate behaviors and errors of cloud APIs to help you build resilient apps.

In this tutorial, you learn how to install and run Dev Proxy.

## Install Dev Proxy

::: zone pivot="client-operating-system-windows"

The easiest way to install Dev Proxy is by using winget. Alternatively, you can install Dev Proxy manually.

# [Automated](#tab/automated)

To install Dev Proxy using winget, run the following command:

```console
winget install Microsoft.DevProxy --silent
```

> [!IMPORTANT]
> Dev Proxy installer adds a new entry to PATH. To use Dev Proxy after installation, you must restart the command prompt to refresh the PATH environment variable.
>

# [Manual](#tab/manual)

[Download](https://aka.ms/devproxy/download/) the latest release and extract the files into a folder. For this tutorial, we assume you extract the files into a folder named `devproxy` located in your home directory.

To start Dev Proxy from any directory, add its installation folder location to your PATH.

1. Open the `Start` menu.
1. Enter `Edit environment variables for your account` into the search box. Select the result in the list to open the `Environment Variables` dialog box.
1. In the `User variables for <username>` section, select the row with the variable name of `Path` and select the `Edit...` button.
1. In the `Edit environment variable` dialog box, select the `New` button.
1. Enter `%USERPROFILE%\devproxy` into the new row and select `OK`.
1. To confirm changes, select `OK`.

---

> [!NOTE]
> To try the latest preview features, install the beta version of Dev Proxy.
>
> # [Automated](#tab/automated)
>
> To install Dev Proxy using winget, run the following command:
>
> ```console
> winget install Microsoft.DevProxy.Beta --silent
> ```
>
>
> # [Manual](#tab/manual)
>
> [Download](https://aka.ms/devproxy/beta/) the latest beta and extract the files into a folder. Follow the manual setup steps as described previously.
>
> ---
>
> To run the beta version of Dev Proxy, use `devproxy-beta`
>

::: zone-end

::: zone pivot="client-operating-system-macos"

The easiest way to install Dev Proxy is by using Homebrew. Alternatively, you can install Dev Proxy manually.

# [Automated](#tab/automated)

To install Dev Proxy using Homebrew, run the following commands:

```console
brew tap microsoft/dev-proxy
brew install dev-proxy
```

# [Manual](#tab/manual)

[Download](https://aka.ms/devproxy/download/) the latest release and extract the files into a folder. For this tutorial, we assume you extract the files into a folder named `devproxy` located in your home directory.

To start Dev Proxy from any directory, add its installation folder location to your PATH.

The below steps show how to add the proxy to PATH when using [zsh](https://www.zsh.org/) shell. Depending on the shell you use, your profile file might differ.

  1. Open your shell profile in a text editor > `~/.zshrc`.
  1. Update `PATH` environment variable with location of the proxy > `export PATH=".:$PATH:$HOME/devproxy"`.
  1. Reload your profile > `source ~/.zshrc`.

---

> [!NOTE]
> To try the latest preview features, install the beta version of Dev Proxy.
>
> # [Automated](#tab/automated)
>
> To install Dev Proxy using Homebrew, run the following commands:
>
> ```console
> brew tap microsoft/dev-proxy
> brew install dev-proxy-beta
> ```
>
> # [Manual](#tab/manual)
>
> [Download](https://aka.ms/devproxy/beta/) the latest beta and extract the files into a folder. Follow the manual setup steps as described previously.
>
> ---
>
> To run the beta version of Dev Proxy, use `devproxy-beta`
>

::: zone-end

::: zone pivot="client-operating-system-linux"

The easiest way to install Dev Proxy is by using the setup script. Alternatively, you can install Dev Proxy manually.

# [Automated](#tab/automated)

To install Dev Proxy using the setup script, run the following commands:

```console
bash -c "$(curl -sL https://aka.ms/devproxy/setup.sh)"
```

If you use PowerShell, run the following command:

```powershell
(Invoke-WebRequest https://aka.ms/devproxy/setup.ps1).Content | Invoke-Expression
```

# [Manual](#tab/manual)

[Download](https://aka.ms/devproxy/download/) the latest release and extract the files into a folder. For this tutorial, we assume you extract the files into a folder named `devproxy` located in your home directory.

To start Dev Proxy from any directory, add its installation folder location to your PATH.

The below steps show how to add the proxy to PATH when using bash shell. Depending on the shell you use, your profile file might differ.

  1. Open your shell profile in a text editor > `~/.bashrc`.
  1. Update `PATH` environment variable with location of the proxy > `export PATH=".:$PATH:$HOME/devproxy"`.
  1. Reload your profile > `source ~/.bashrc`.

---

> [!NOTE]
> To try the latest preview features, install the beta version of Dev Proxy.
>
> # [Automated](#tab/automated)
>
> To install Dev Proxy using the setup script, run the following commands:
>
> ```console
> bash -c "$(curl -sL https://aka.ms/devproxy/setup-beta.sh)"
> ```
>
> If you use PowerShell, run the following command:
>
> ```powershell
> (Invoke-WebRequest https://aka.ms/devproxy/setup-beta.ps1).Content | Invoke-Expression
> ```
>
> # [Manual](#tab/manual)
>
> [Download](https://aka.ms/devproxy/beta/) the latest beta and extract the files into a folder. Follow the manual setup steps as described previously.
>
> ---
>
> To run the beta version of Dev Proxy, use `devproxy-beta`
>

::: zone-end

## Start Dev Proxy for the first time

The first time you start Dev Proxy on your machine there are a few steps to follow to ensure that Dev Proxy can intercept requests from your machine and respond successfully. You won't have to repeat these steps after the first run.

::: zone pivot="client-operating-system-windows"

1. **Start Dev Proxy**. Open a command prompt session. Enter `devproxy` and press <kbd>Enter</kbd>.
1. **Trust certificate**. Dev Proxy installs a certificate named `Dev Proxy CA`. A warning shows. Select `Yes` to confirm that you want to install the certificate. Dev Proxy uses this certificate to decrypt HTTPS traffic sent from your machine.
1. **Allow firewall access**. Windows Firewall blocks the proxy. A warning shows. Select `Allow access` button to allow traffic through the firewall.

::: zone-end

::: zone pivot="client-operating-system-macos"

1. **Start Dev Proxy**. Open a command prompt session. Enter `devproxy` and press <kbd>Enter</kbd>.
1. **Trust certificate**. Dev Proxy installs a certificate named `Dev Proxy CA`, which it uses to decrypt HTTPS traffic sent from your machine. A warning shows. Press <kbd>y</kbd> to confirm that you want to trust the certificate.
1. **Accept incoming connections**. A warning shows. Select `Allow` to confirm.

::: zone-end

::: zone pivot="client-operating-system-linux"

1. **Start Dev Proxy**. Open a command prompt session. Enter `devproxy` and press <kbd>Enter</kbd>.
1. **Trust certificate**. Dev Proxy uses a custom SSL certificate to decrypt HTTPS traffic sent from your machine.

    > [!IMPORTANT]
    > The following instructions are for Ubuntu. For other Linux distributions, the steps might differ.

    To install and trust the certificate, in a new command prompt, run the following commands:

    ```console
    # Export Dev Proxy root certificate
    openssl pkcs12 -in ~/.config/dev-proxy/rootCert.pfx -clcerts -nokeys -out dev-proxy-ca.crt -passin pass:""
    # Install the certificate
    sudo cp dev-proxy-ca.crt /usr/local/share/ca-certificates/
    # Update certificates
    sudo update-ca-certificates
    ```

::: zone-end

The command prompt displays the following output:

```text
 info    8 error responses loaded from devproxy-errors.json
 info    Dev Proxy API listening on http://localhost:8897...
 info    Dev Proxy Listening on 127.0.0.1:8000...

Hotkeys: issue (w)eb request, (r)ecord, (s)top recording, (c)lear screen
Press CTRL+C to stop Dev Proxy
```

By default, Dev Proxy is configured to:

- Intercept requests made to any [JSON Placeholder API](https://jsonplaceholder.typicode.com/) endpoint
- Simulate API error responses and API throttling with a failure rate of 50%

## Confirm that Dev Proxy is working correctly

Dev Proxy intercepts requests that applications on your machine make to URLs that you register with Dev Proxy. When Dev Proxy detects a request, it either passes it through to the API (take no action), or returns a response. Let's confirm that Dev Proxy is working as expected.

In PowerShell, use the `Invoke-WebRequest` cmdlet to send a GET request to the JSON Placeholder API.

```powershell
Invoke-WebRequest -Uri https://jsonplaceholder.typicode.com/posts
```

If you use `curl`, send a GET request to the JSON Placeholder API using the following command.

```console
curl -ikx http://localhost:8000 https://jsonplaceholder.typicode.com/posts
```

You can also use an API client like [Postman](https://www.postman.com/product/api-client/) to send a GET request to `https://jsonplaceholder.typicode.com/posts`.

In the command line where Dev Proxy is running, you see the information about the request and the action that Dev Proxy performed. By default, Dev Proxy simulates an error response with a 50% chance. If your request doesn't return an error, Dev Proxy passes it through.

```text
 req   ╭ GET https://jsonplaceholder.typicode.com/posts
 time  │ 1/31/2025 12:12:14 PM +00:00
 skip  │ RetryAfterPlugin: Request not throttled
 skip  │ GenericRandomErrorPlugin: Pass through
 api   ╰ Passed through
```

If Dev Proxy returns an error response, you see the error message in the output.

```text
 req   ╭ GET https://jsonplaceholder.typicode.com/posts
 time  │ 1/31/2025 12:12:37 PM +00:00
 skip  │ RetryAfterPlugin: Request not throttled
 oops  ╰ 403 Forbidden
```

> [!IMPORTANT]
> If you don't see any output in the command prompt, it's likely that Dev Proxy isn't intercepting requests. Check the [common problems](../how-to/overview.md#common-problems) section for help.

## Stop Dev Proxy safely

When you no longer require Dev Proxy to be running, you should always stop it safely.

- Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to safely stop Dev Proxy.

If you shut down the command prompt session, Dev Proxy doesn't unregister correctly as the system proxy, and you might experience some [common problems](../how-to/overview.md#common-problems).

## Next step

Learn how to configure Dev Proxy to your needs. Dev Proxy is highly flexible and supports many different scenarios. Learn more about how to configure it to your specific scenario.

> [!div class="nextstepaction"]
> [Configure Dev Proxy](./configure.md)
