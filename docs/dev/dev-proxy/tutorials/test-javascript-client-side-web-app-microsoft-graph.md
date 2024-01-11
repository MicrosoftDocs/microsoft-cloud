---
title: Test a JavaScript client-side web application that calls Microsoft Graph
description: Learn how to use Dev Proxy with a sample JavaScript client-side web application that calls Microsoft Graph.
author: garrytrinder
ms.author: garrytrinder
ms.date: 11/1/2024
---

# Test a JavaScript client-side web application that calls Microsoft Graph

In this tutorial, you learn how to use Dev Proxy to test a sample JavaScript client-side web application that calls Microsoft Graph.

## Prerequisites

This part of the tutorial assumes that you installed and configured Dev Proxy on your machine. If not, do that [now](../get-started.md).

To follow this tutorial, you need:

- Microsoft 365 tenant.
- Account with permissions to create Microsoft Entra app registrations.
- Git (see [GitHub’s set up Git guide](https://help.github.com/en/github/getting-started-with-github/set-up-git)).
- [nodejs LTS](https://nodejs.org).

> [!TIP]
> We recommend that you use a Microsoft 365 Developer Tenant with content packs installed. Get your free tenant by [signing up to the Microsoft 365 Developer Program](https://aka.ms/m365/).

## Clone and configure the sample app

- Clone the repository.

```sh
git clone https://github.com/microsoft/dev-proxy.git
```

- Follow the instructions in [samples/readme.md](https://github.com/microsoft/dev-proxy/blob/main/samples/readme.md) to configure the app.

## Start Dev Proxy

Dev Proxy comes with a preset configuration for testing apps that send requests to Microsoft Graph and SharePoint Online APIs.

- Open a terminal, enter `devproxy --config-file presets\m365.json` and press <kbd>Enter</kbd> to start Dev Proxy with the preset configuration.

## Launch the sample app

- Open a terminal and change to the `samples` directory.
- Enter `npx lite-server` and press <kbd>Enter</kbd> to start the sample app web server.

:::image type="content" source="../media/test-app-js-welcome.png" alt-text="Screenshot of the sample app running in Microsoft Edge browser on macOS. The app shows a large Microsoft logo with two buttons below it. A primary button with the text 'With SDK' and a secondary button with the text 'Without SDK'.":::

## Test the sample app

- In the running app, select the `Without SDK` button.

> [!CAUTION]
> If you got an empty page after clicking the `Without SDK` button, check that you have [configured the Azure AD App Registration](https://github.com/microsoft/dev-proxy/tree/main/samples#configure-azure-ad-app-registration). The issue occurs when the `.env` file containing the `Client ID` of your app registration is missing.

- Select the `Login` button and complete the sign in flow.

:::image type="content" source="../media/test-app-js-login.png" alt-text="Screenshot of the sample app running in Microsoft Edge browser on Windows 11. The app shows a large Microsoft logo with two buttons below it. A primary button with the text 'Login' and a secondary button with the text 'Back'.":::

Dev Proxy introduces faults into your application by intercepting requests to Microsoft Graph. It uses 50% chance for failing requests with a random [supported HTTP error status code](../technical-reference/Supported-HTTP-error-status-codes.md).

View the proxy output and take a moment to refresh the sample app. See how the sample app handles (or not, in this case) the failures introduced by the proxy.

:::image type="content" source="../media/test-app-js-requests.png" alt-text="Screenshot of the sample app running in Microsoft Edge. User avatars aren't shown in the app. The Microsoft Edge Developer Tools are open to the side with errors shown in the console log.":::

- Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to stop Dev Proxy.

## Next step

> [!div class="nextstepaction"]
> [Explore How-to guides](../how-to/overview.md)
