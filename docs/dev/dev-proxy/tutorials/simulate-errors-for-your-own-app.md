---
title: Simulate random errors for your own application
description: Learn how to use Dev Proxy to simulate random errors for your own application.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/08/2024
---

# Simulate random errors for your own application

In this tutorial, you learn how to use Dev Proxy to simulate random errors for your own application.

## Prerequisites

This part of the tutorial assumes that you installed and configured Dev Proxy on your machine. If not, do that [now](../get-started.md).

To follow this tutorial, you need an application that calls APIs. You also need to know the URLs of the APIs that your application calls.

You can use Dev Proxy with any type of application and technology stack. Here are instructions for how to set up Dev Proxy with a few popular technologies.

# [JavaScript](#tab/javascript)

Use Dev Proxy with:

- [Node.js applications](../how-to/use-dev-proxy-with-nodejs.md)
- [Node.js applications in Docker](../how-to/use-dev-proxy-with-nodejs-docker.md)
- [JavaScript Azure Functions](../how-to/use-dev-proxy-with-javascript-azure-functions.md)
- [SharePoint Framework (SPFx) solutions](../how-to/use-dev-proxy-with-spfx.md)

# [.NET](#tab/dotnet)

Use Dev Proxy with:

- [.NET applications](../how-to/use-dev-proxy-with-dotnet.md)
- [.NET applications in Docker](../how-to/use-dev-proxy-with-dotnet-docker.md)
- [.NET Aspire applications](../how-to/use-dev-proxy-with-dotnet-aspire.md)

If you're using Dev Proxy with a .NET 4.8 app, you need to [register Dev Proxy on your system](../how-to/why-is-proxy-not-intercepting-requests-from-my-dotnet-4-8-app.md) using `netsh`.

# [Other](#tab/other)

If you want to use Dev Proxy with a client-side application, typically you don't need to take any extra steps. After you start Dev Proxy, it automatically intercepts all requests from your machine.

If you want to use Dev Proxy with a server-side application built with a technology not listed here, you might need to take extra steps to configure your application to use Dev Proxy. Check the documentation for your technology to learn how to configure it to use a network proxy. If you have any questions, ask for help in the [Dev Proxy community](https://aka.ms/devproxy/discord) on Discord.

---

## Start Dev Proxy with monitoring your URLs

Start Dev Proxy and monitor the URLs of the APIs that your application calls. For example, if your application calls an API located at `https://api.contoso.com/v1/customers`, start Dev Proxy and monitor the URL pattern `https://api.contoso.com/*`.

```bash
devproxy --urls-to-watch "https://api.contoso.com/*"
```

The `--urls-to-watch` parameter tells Dev Proxy, which requests to intercept. The wildcard `*` at the end of the URL tells Dev Proxy to intercept all requests to URLs that start with `https://api.contoso.com/`.

Start using your application as you normally would. Dev Proxy intercepts all requests to the URLs that you specified. In the terminal, you see messages about the requests that Dev Proxy intercepts.

```text
```text
 request     GET https://api.contoso.com/v1/customers
     api   ╭ Passed through
           ╰ GET https://api.contoso.com/v1/customers
 request     GET https://api.contoso.com/v1/customers
   chaos   ╭ 403 Forbidden
           ╰ GET https://api.contoso.com/v1/customers
```

> [!IMPORTANT]
> If you don't see any messages in the terminal, make sure that you correctly configured your application to use Dev Proxy. Also, check if Dev Proxy is intercepting requests to API URLs that your application uses. If you have any questions, ask for help in the [Dev Proxy community](https://aka.ms/devproxy/discord) on Discord.

## Create your own configuration files

By default, Dev Proxy uses the `devproxyrc.json` file in the Dev Proxy installation folder for its configuration settings. The file is configured to simulate random errors for the JSON Placeholder API. To get more realistic results, create your own configuration files with errors that are more relevant to your application and the APIs it uses, and use them with Dev Proxy.

Let's consider that you want to store a configuration file in the project folder for your app, so you can share the configuration settings with the rest of your team.

- In the Dev Proxy installation folder, copy `devproxyrc.json` and `devproxy-errors.json`.
- In your project folder, paste the files.

When using a configuration file that is stored outside of the Dev Proxy installation file, you need to ensure that the `pluginPath` references are correct. Rather than hard coding the paths to the Dev Proxy installation folder in your configuration file, you can use the `~appFolder` at the beginning of the path to include a dynamic reference back to the Dev Proxy installation folder.

- In a text editor, open the `devproxyrc.json` file.
- Locate the `GenericRandomErrorPlugin` plugin in the `plugins` array.
- Update the `pluginPath` to `~appFolder/plugins/dev-proxy-plugins.dll`.
- Locate the `RetryAfterPlugin` plugin in the `plugins` array.
- Update the `pluginPath` to `~appFolder/plugins/dev-proxy-plugins.dll`.
- In a terminal, change the working directory to your project folder.
- Enter `devproxy --config-file devproxyrc.json` and press <kbd>Enter</kbd> to start Dev Proxy using your configuration file.
- Send a request to the JSON Placeholder API from the command line and view the output.
- Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to safely stop Dev Proxy.

> [!TIP]
> Install the [Dev Proxy Toolkit](https://marketplace.visualstudio.com/items?itemName=garrytrinder.dev-proxy-toolkit) extension for Visual Studio Code which makes it easy to create and update configuration files.

## Next step

Dev Proxy supports many different scenarios that help you build more robust applications. Explore the how-to guides to learn how to use the different Dev Proxy features and improve your application.

> [!div class="nextstepaction"]
> [Explore how-to guides](../how-to/overview.md)
