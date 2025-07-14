---
title: Test a JavaScript client-side web application
description: Learn how to use Dev Proxy with a sample JavaScript client-side web application.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 04/08/2024
---

# Test a JavaScript client-side web application

In this tutorial, you learn how to use Dev Proxy to test how a sample JavaScript client-side web application handles API errors.

## Prerequisites

This part of the tutorial assumes that you installed and configured Dev Proxy on your machine. If not, do that [now](../get-started.md).

To follow this tutorial, you need:

- [Node.js LTS](https://nodejs.org).

## Download the sample app

Download the [sample app](https://pnp.github.io/download-partial/?url=https://github.com/pnp/proxy-samples/tree/main/samples/demo-randomerror-js).

> [!TIP]
> You can also download the sample app by running in the command prompt `devproxy config get demo-randomerror-js`.

The sample app comes with a Dev Proxy preset. The preset is configured to simulate random errors on API requests issued by the app. The preset also includes the [`RetryAfterPlugin`](../technical-reference/retryafterplugin.md), which helps you control if the app backs off from calling API after it's throttled.

## Start Dev Proxy and the sample app

1. In a command prompt, change the working directory to where the sample app is located.
1. Start the sample app and Dev Proxy by running `npm start`

## Test the sample app

1. In a web browser, navigate to `http://localhost:3000`
   
   :::image type="content" source="../media/tutorial-js-webapp.png" alt-text="Screenshot of a web browser with the test web application." lightbox="../media/tutorial-js-webapp.png":::
   - If you see an empty page, check the Console window. It could be that Dev Proxy has already simulated an API error, which the app didn't handle!
1. Navigate through the list of articles to see how the app handles API errors that Dev Proxy simulates.
   - You can find more information about the errors in the Console window and in the command prompt where Dev Proxy is running.

   :::image type="content" source="../media/tutorial-js-devproxy-random-errors.png" alt-text="Screenshot of VS Code with the command prompt window open showing errors simulated by Dev Proxy." lightbox="../media/tutorial-js-devproxy-random-errors.png":::
1. Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to stop Dev Proxy.

## Next step

> [!div class="nextstepaction"]
> [Explore How-to guides](../how-to/overview.md)
