---
title: Test a JavaScript client-side web application
description: Sample scenario of testing a JavaScript client-side web application
author: garrytrinder
ms.author: garrytrinder
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

# Test a JavaScript client-side web application

> [!CAUTION]
> This part of the tutorial assumes that you started Dev Proxy on your machine before. If not, do that [now](./using-the-proxy-for-the-first-time.md).

Learn how to use Dev Proxy with a sample JavaScript client-side web application that calls Microsoft Graph.

## Before you begin

To follow this tutorial you need:

- Microsoft 365 tenant.
- Account with permissions to create Entra app registrations.
- Node.js LTS.

We recommend that you use a Microsoft 365 Developer Tenant with content packs installed.

> [!TIP]
> Get your free tenant by [signing up to the Microsoft 365 Developer Program](https://aka.ms/m365/).

You also need to have the following installed and configured on your machine:

- Git (see [GitHub’s set up Git guide](https://help.github.com/en/github/getting-started-with-github/set-up-git)).
- [nodejs LTS](https://nodejs.org).

## 1. Clone and configure the sample app

Clone the repository.

```sh
git clone https://github.com/microsoft/dev-proxy.git
```

Follow the instructions in [samples/readme.md](https://github.com/microsoft/dev-proxy/blob/main/samples/readme.md) to configure the app.

## 2. Start Dev Proxy

Open a terminal, enter `devproxy` and press <kbd>Enter</kbd>.

> [!NOTE]
> If you're using macOS, you will also need to enable the `Secure Web Proxy (HTTPS)` proxy on your network device.

## 3. Launch the sample app

Open a new terminal and change to the `samples` directory.

Enter `npx lite-server` and press <kbd>Enter</kbd> to start the sample app web server.

:::image type="content" source="../media/test-app-js-welcome.png" alt-text="Screenshot of the sample app running in Microsoft Edge browser on macOS. The app shows a large Microsoft logo with two buttons below it. A primary button with the text 'With SDK' and a secondary button with the text 'Without SDK'.":::

## 4. Test the sample app

In the running app, select the `Without SDK` button.

> [!CAUTION]
> If you get an empty page after clicking the `Without SDK` button, check that you have [configured the Azure AD App Registration](https://github.com/microsoft/dev-proxy/tree/main/samples#configure-azure-ad-app-registration). The issue occurs when the `.env` file containing the `Client ID` of your app registration is missing.

Select the `Login` button and complete the sign in flow.

:::image type="content" source="../media/test-app-js-login.png" alt-text="Screenshot of the sample app running in Microsoft Edge browser on Windows 11. The app shows a large Microsoft logo with two buttons below it. A primary button with the text 'Login' and a secondary button with the text 'Back'.":::

Dev Proxy introduces faults into your application by intercepting requests to Microsoft Graph. It uses 50% chance for failing requests with a random [supported HTTP error status code](../technical-reference/Supported-HTTP-error-status-codes.md).

View the proxy output and take a moment to refresh the sample app. See how the sample app handles (or not, in this case) the failures introduced by the proxy.

:::image type="content" source="../media/test-app-js-requests.png" alt-text="Screenshot of the sample app running in Microsoft Edge. User avatars are not shown in the app. The Edge Developer Tools are open to the side with errors shown in the console log.":::

## 5. Stop Dev Proxy

Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to stop the process.

> [!NOTE]
> If you are using macOS, you will also need to disable the `Secure Web Proxy (HTTPS)` proxy on your network device.

## On to the next step

Now you’re ready to go on to the next step.

> [!div class="nextstepaction"]
> [Explore how-to guides](../how-to/overview.md)
