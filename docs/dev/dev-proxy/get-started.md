---
title: Get started with Dev Proxy
description: Learn how to install, run, and configure Dev Proxy.
author: garrytrinder
ms.author: garrytrinder
ms.date: 04/11/2024
ms.topic: get-started
zone_pivot_groups: client-operating-system
#Customer intent: As a developer, I want to test the resilience of my application so that I can understand how my application reacts to cloud API failures.
---

# Get started with Dev Proxy

Dev Proxy is a command line tool that helps you simulate behaviors and errors of cloud APIs to help you build resilient apps.

In this tutorial, you learn how to install, run, and configure Dev Proxy.

If you do run into any difficulties, don’t hesitate to contact us by raising a [new issue](https://github.com/microsoft/dev-proxy/issues/new) and we're glad to help you out.

## Install Dev Proxy

::: zone pivot="client-operating-system-windows"

The easiest way to install Dev Proxy is by using winget. Alternatively, you can install Dev Proxy manually.

# [Automated](#tab/automated)

Install Dev Proxy using winget, run the following command:

```console
winget install Microsoft.DevProxy --silent
```

# [Manual](#tab/manual)

[Download](https://aka.ms/devproxy/download/) the latest release and extract the files into a folder. For this tutorial, we assume you extract the files into a folder named `devproxy` located in your home directory.

To start Dev Proxy from any directory, add its installation folder location to your PATH.

1. Open the `Start` menu.
1. Enter `Edit environment variables for your account` into the search box, select the result in the list to open the `Environment Variables` dialog box.
1. In the `User variables for <username>` section, select the row with the variable name of `Path` and select the `Edit...` button.
1. In the `Edit environment variable` dialog box, select the `New` button.
1. Enter `%USERPROFILE%\devproxy` into the new row and select `OK`.
1. Select `OK` to confirm changes.

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
> To run the beta version of Dev Proxy use `devproxy-beta`
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
> To run the beta version of Dev Proxy use `devproxy-beta`
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

The command prompt displays the following output:

```text
8 error responses loaded from devproxy-errors.json
Listening on 127.0.0.1:8000...
Hotkeys: issue (w)eb request, (r)ecord, (s)top recording, (c)lear screen
Press CTRL+C to stop Dev Proxy
```

By default, Dev Proxy is configured to:

- Intercept requests made to any [JSON Placeholder API](https://jsonplaceholder.typicode.com/) endpoint
- Simulate API error responses and API throttling with a failure rate of 50%

## Intercept requests

Dev Proxy will intercept requests made to known URLs from any application on your machine. When a request is detected, Dev Proxy passes the request through to the API (take no action), or return a response.

- Send a request to the JSON Placeholder API from the command line and switch back to the proxy process to view the output.

In PowerShell, use the `Invoke-WebRequest` cmdlet to send a GET request to the JSON Placeholder API.

```powershell
Invoke-WebRequest -Uri https://jsonplaceholder.typicode.com/posts
```

If you use `curl`, send a GET request to the JSON Placeholder API using the following command.

```console
curl -ix http://localhost:8000 https://jsonplaceholder.typicode.com/posts
```

You can also use an API client like [Postman](https://www.postman.com/product/api-client/) to send a GET request to `https://jsonplaceholder.typicode.com/posts`.

An entry is shown with some basic information about the incoming request and the action that Dev Proxy performed. Dev Proxy simulates an error response with a 50% chance. If your request doesn't return an error, Dev Proxy passes it through.

```text
 request     GET https://jsonplaceholder.typicode.com/posts
     api   ╭ Passed through
           ╰ GET https://jsonplaceholder.typicode.com/posts
```

- Repeat sending requests to the JSON Placeholder API from the command line, until an error response is returned.

```text
 request     GET https://jsonplaceholder.typicode.com/posts
     api   ╭ Passed through
           ╰ GET https://jsonplaceholder.typicode.com/posts
 request     GET https://jsonplaceholder.typicode.com/posts
   chaos   ╭ 403 Forbidden
           ╰ GET https://jsonplaceholder.typicode.com/posts
```

When Dev Proxy returns an error response, a `chaos` label is displayed in the entry.

- Try sending requests to other endpoints available on the JSON Placeholder API
  - `https://jsonplaceholder.typicode.com/posts`
  - `https://jsonplaceholder.typicode.com/posts/1`
  - `https://jsonplaceholder.typicode.com/posts/1/comments`
  - `https://jsonplaceholder.typicode.com/comments?postId=1`

## Stop Dev Proxy safely

When you no longer require Dev Proxy to be running, you should always stop it safely.

- Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to safely stop Dev Proxy.

If you shut down the command prompt session, Dev Proxy doesn't unregister correctly as the system proxy, and you might experience some [common problems](./how-to/overview.md#common-problems).

## Update the URLs to watch

By default, Dev Proxy is configured to intercept any request made to the [JSON Placeholder API](https://jsonplaceholder.typicode.com/). You can configure Dev Proxy to intercept requests to any HTTP API.

::: zone pivot="client-operating-system-macos"
- In a command prompt, run `brew list dev-proxy` to locate the installation folder.
- Open the Dev Proxy installation folder in Finder.
::: zone-end
- In the Dev Proxy installation folder, open `devproxyrc.json` in a text editor.
- Locate the `urlsToWatch` array.

```json
"urlsToWatch": [
  "https://jsonplaceholder.typicode.com/*"
],
```

The `urlsToWatch` array represents the known URLs. Dev Proxy watches requests from the current entry to any endpoint. The entry uses an asterisk after the URL as a wildcard. Adding more entries into this array expands the URLs that Dev Proxy watches out for.

Let's consider that you don't want Dev Proxy to intercept requests made to a specific endpoint.

- Add a new entry to the `urlsToWatch` array.

```json
"urlsToWatch": [
  "!https://jsonplaceholder.typicode.com/posts/2",
  "https://jsonplaceholder.typicode.com/*"
],
```

The exclamation mark at the beginning of the URL tells Dev Proxy to ignore any requests that match that URL. You can mix and match exclamation marks and asterisks in a URL.

- At the command line, enter `devproxy` and press <kbd>Enter</kbd> to start Dev Proxy.
- Send a request to `https://jsonplaceholder.typicode.com/posts/2` from the command line and view the output.

An entry is shown confirming that the request was ignored and passed through to the API.

```text
request     GET https://jsonplaceholder.typicode.com/posts/2
     api   ╭ Passed through
           ╰ GET https://jsonplaceholder.typicode.com/posts/2
```

The order in which the URLs are listed in the `urlsToWatch` array is important. Dev Proxy processes these URLs in order. When a URL matches, it isn’t processed again. Therefore, placing the URL first ensures that the request is ignored before the next URL is processed.

## Change failure rate

By default, Dev Proxy is configured to fail requests with a 50% chance to URLs that are being watched. You can increase or decrease the chance of a request returning an error response.

Let’s update the failure rate so that every request to the JSON Placeholder API returns an error response.

- In the Dev Proxy installation folder, open `devproxyrc.json` in a text editor.
- Locate the `rate` property and update the value from `50` to `100`.

The `devproxyrc.json` file contains configuration settings that are used when you start Dev Proxy. When changing configuration settings, you should always stop and start Dev Proxy for the changes to be persisted.

- At the command line, enter `devproxy` and press <kbd>Enter</kbd> to start Dev Proxy.
- Send a request to the JSON Placeholder API from the command line and view the output.

Alternatively, you can override configuration settings at runtime by using the `--failure-rate` option when starting Dev Proxy.

```text
devproxy --failure-rate 100
```

- Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to safely stop Dev Proxy.

## Simulate throttling

By default, Dev Proxy returns a range of generic 400 and 500 error responses. You can customize these error responses to your own needs.

Dev Proxy uses [plugins](./technical-reference/overview.md#plugins) to enable different API behaviors, by default, we enable two plugins for you.

- [GenericRandomErrorPlugin](./technical-reference/genericrandomerrorplugin.md) plugin provides the ability for Dev Proxy to respond with an error response.
- [RetryAfterPlugin](./technical-reference/retryafterplugin.md) plugin provides the ability for Dev Proxy to inject a dynamic value into the Retry-After header in the error response.

Let’s change the configuration so that Dev Proxy always returns a `429 Too Many requests` error response to simulate throttling.

First let's locate the location of the file that contains the error definitions.

- In the Dev Proxy installation folder, open `devproxyrc.json` in a text editor.
- In the `plugins` array, locate the entry for the [GenericRandomErrorPlugin](./technical-reference/genericrandomerrorplugin.md) plugin. Note the value of the `configSection` property.
- Further down the file, locate the `genericRandomErrorPlugin` object. Note the value of the `errorsFile` property.

> [!TIP]
> The location of the errors file is also displayed in the output when you start Dev Proxy.

- In the Dev Proxy installation folder, open `devproxy-errors.json` in a text editor.
- Remove all response entries in the `responses` array, except for the `429` response.

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v0.17.0/genericrandomerrorplugin.schema.json",
  "responses": [
    {
      "statusCode": 429,
      "body": {
        "message": "Too Many Requests",
        "details": "The user has sent too many requests in a given amount of time (\"rate limiting\")."
      },
      "headers": {
        "Retry-After": "@dynamic"
      }
    }
  ]
}
```

- At the command line, enter `devproxy` and press <kbd>Enter</kbd> to start Dev Proxy.
- Send a request to the JSON Placeholder API from the command line and view the output.

```text
 request     GET https://jsonplaceholder.typicode.com/posts
   chaos   ╭ 429 TooManyRequests
           ╰ GET https://jsonplaceholder.typicode.com/posts
```

- Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to safely stop Dev Proxy.

## Next step

Learn how to use Dev Proxy to simulate random errors for your own application.

> [!div class="nextstepaction"]
> [Simulate errors for your own app](./tutorials/simulate-errors-for-your-own-app.md)
