---
title: Test a sample app with Microsoft 365 Developer Proxy
description: Test a sample app with Microsoft 365 Developer Proxy
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

This is a step-by-step introduction intended for beginners.

In this tutorial you will learn how to use the Microsoft 365 Developer Proxy for the first time against a sample application.

> [!IMPORTANT]
> Have you installed and configured the proxy on your local machine? If not, [do that now](./Installation).

## Before you begin

The sample you will use has been designed with keeping dependencies to an absolute minimum, however there are a few things that you will need.

You will need a Microsoft 365 Tenant and be able to use an account that has permissions to create an Azure AD App Registrations in your tenant.

We highly recommend that you use a Microsoft 365 Developer Tenant with content packs installed when using this sample. You can create a developer tenant by [signing up to the Microsoft 365 Developer Program](https://aka.ms/m365/).

You will need to have the following installed or configured to run the sample app used in this tutorial:

* Git (see [GitHub’s set up Git guide](https://help.github.com/en/github/getting-started-with-github/set-up-git))
* [nodejs LTS](https://nodejs.org/en/)

## 1. Clone the sample app

Clone this repository to your local machine and follow the instructions in [samples/readme.md](https://github.com/microsoft/m365-developer-proxy/blob/main/samples/readme.md) to configure the sample app.

```sh
git clone https://github.com/microsoft/m365-developer-proxy.git
```

## 2. Start the proxy for the first time

Open up a terminal session and change to the `samples` directory.

> [!IMPORTANT]
> If you are using macOS, have you completed the additional configuration steps for macOS? If not, [do that now](microsoft-cloud/dev/m365-developer-proxy/get-started/installation) and ensure that you have enabled the `Secure Web Proxy (HTTPS)` protocol on your network adaptor.

To start the proxy, enter `m365proxy` and press <kbd>Enter</kbd>.

> [!NOTE]
> If you are using Windows, you will be prompted twice the first time you run the proxy.
>
> A Security Warning prompt stating that you are about to install a certificate, `Titanium Root Certificate Authority` which will be used by the proxy to decrypt HTTPS traffic from your application. Select `Yes` to confirm that you want to install the certificate.
>
> A Windows Security Alert prompt stating that Windows Defender Firewall has blocked the proxy. To allow traffic through the firewall, check the options appropriate to your preferences and then select the `Allow access` button to continue.

When the proxy has started, the terminal will display the following output.

```cmd
Configuring proxy for cloud global - graph.microsoft.com
Listening on 'ExplicitProxyEndPoint' endpoint at Ip 0.0.0.0 and port: 8000
Set endpoint at Ip 0.0.0.0 and port: 8000 as System HTTP Proxy
Set endpoint at Ip 0.0.0.0 and port: 8000 as System HTTPS Proxy
Press Ctrl + C to stop the Microsoft 365 Developer Proxy
```

## 3. Launch the sample app

With the proxy running, let's start the sample app so that we can test it.

Open up a terminal session and change to the `samples` directory.

Enter `npx lite-server` and press <kbd>Enter</kbd> to start the sample app web server.

A web browser will be automatically launched and the sample app will be displayed.

<img width="1920" alt="Screenshot of Microsoft 365 Developer Proxy sample app running in Microsoft Edge browser on Windows 11. The app displays a large Microsoft Graph globe icon with two buttons below it, a primary button with the text 'With SDK' on it and a secondary button with the text 'Without SDK' on it" src="https://user-images.githubusercontent.com/11563347/204579849-6659d1a3-c2d9-45b2-8f64-530634ddeff5.png">

## 4. Test the sample app

In the running sample app, select the `Without SDK` button.

> [!NOTE]
> If you get an empty page after clicking the `Without SDK` button, check that you have [configured the Azure AD App Registration](https://github.com/microsoft/m365-developer-proxy/tree/main/samples#configure-azure-ad-app-registration) and created the `.env` file containing the `Client ID` of your App Registration.

Next, select the `Login` button and complete the authentication flow.

<img width="1925" alt="image" src="https://user-images.githubusercontent.com/11563347/204581193-65c2aa39-fec7-4d2a-b642-d7350b881904.png">

At this point, the Microsoft 365 Developer Proxy will start to introduce faults into your application by intercepting requests made to Microsoft Graph and failing 50% of those requests selecting randomly from the list of [supported HTTP error status codes](microsoft-cloud/dev/m365-developer-proxy/technical-reference/supported-HTTP-error-status-codes).

If you take a look at the terminal session where the proxy is running, you will see messages telling you which requests have been detected, and whether they have been passed through the proxy (successful), or responded to with an error code (failed).

In addition to the request messages, the proxy is able to detect whether or not your application is using a Microsoft Graph SDK. If an SDK is not being used it provides some additional guidance to help you can migrate your application to use a Microsoft Graph SDK and take advantage of the benefits that they offer, which make it easier for you to handle API errors like throttling, send complex batch operations and handle binary responses.

Example output:

```cmd
saw a graph request: GET /v1.0/users
        TIP: To handle API errors more easily, use the Graph SDK. More info at https://aka.ms/move-to-graph-js-sdk
        Failed /v1.0/users with GatewayTimeout
saw a graph request: GET /v1.0/users
        TIP: To handle API errors more easily, use the Graph SDK. More info at https://aka.ms/move-to-graph-js-sdk
        Failed /v1.0/users with BadGateway
saw a graph request: GET /v1.0/users
        TIP: To handle API errors more easily, use the Graph SDK. More info at https://aka.ms/move-to-graph-js-sdk
        Failed /v1.0/users with ServiceUnavailable
saw a graph request: GET /v1.0/users
        Passed through /v1.0/users
saw a graph request: GET /v1.0/users/ff4983ef-4d36-478a-b054-166c2c085645/presence
        TIP: To handle API errors more easily, use the Graph SDK. More info at https://aka.ms/move-to-graph-js-sdk
        Failed /v1.0/users/ff4983ef-4d36-478a-b054-166c2c085645/presence with GatewayTimeout
saw a graph request: GET /v1.0/users/53f56359-28d7-401a-bc1d-f93c23d7e6f1/presence
        Passed through /v1.0/users/53f56359-28d7-401a-bc1d-f93c23d7e6f1/presence
```

Take a moment to refresh the sample app and view the different errors that are generated, as well as how the sample app handles (or not, in this case) the failures introduced by the proxy.

<img width="1920" alt="Screenshot of the Microsoft 365 Developer Proxy sample app in Microsoft Edge in side by side view with Edge Developer Tools. On the left the application displays the user avatars and on the right the the Console tab in the Developer Tools displaying errors in the log" src="https://user-images.githubusercontent.com/11563347/204582466-b7fac795-63cb-4758-bb26-80b2f94f4d28.png">

## 5. Show help and usage information

You can change the behaviour of the Microsoft 365 Developer Proxy using several settings that can be provided when starting the proxy process.

To view the available [proxy settings](microsoft-cloud/dev/m365-developer-proxy/technical-reference/proxy-settings) on the command line:

```text
m365proxy --help
```

## 6. Stop the proxy process safely

When you no longer require the proxy to be running you should always end the proxy process safely.

To safely end the proxy process:

1. Switch to the terminal window where the proxy process is running
2. Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to stop the process

> [!CAUTION]
> If you shutdown the terminal session, the proxy will not be unregistered correctly which may lead to you experiencing some [common problems](microsoft-cloud/dev/m365-developer-proxy/how-to/index#common-problems) when using the proxy.


> [!NOTE]
> If you are using macOS, you should also uncheck the `Secure Web Proxy (HTTPS)` proxy configured on your network adaptor after shutting down the proxy process.

## On to the next step

Learn how to mock responses, simulate errors and behaviours.

Take a look at our [How-to](microsoft-cloud/dev/m365-developer-proxy/how-to) guides.
