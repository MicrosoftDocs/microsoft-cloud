---
title: Use Dev Proxy with .NET Aspire applications
description: How to use Dev Proxy with Use Dev Proxy with .NET Aspire applications
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/07/2024
---

# Use Dev Proxy with .NET Aspire applications

[.NET Aspire](/dotnet/aspire/get-started/aspire-overview) is an opinionated, cloud ready stack for building observable, production ready, distributed applications. It's built on top of .NET and provides a modern, fast, and scalable platform for building web applications.

If you want to use Dev Proxy with your .NET Aspire application, you first need to find out the internal URLs that your application uses to communicate with other services. Once you have the URLs, configure Dev Proxy to intercept the requests and simulate different scenarios, such as throttling, errors, or latency.

## Discover internal URLs

To discover the internal URLs that your .NET Aspire application uses:

1. In a terminal, start the app host project

    ```sh
    dotnet run --project src/MyApp.Host/MyApp.Host.csproj
    ```

1. In the web browser, open the dashboard of your .NET Aspire application
1. From the list of services, find the service that you want to simulate errors for, and note its internal URL, for example `http://localhost:5222`
1. In a terminal, stop the app host project, by pressing <kbd>Ctrl</kbd>+<kbd>C</kbd>

## Start Dev Proxy monitoring the internal URLs

Start Dev Proxy and configure it to intercept the requests to the internal URLs that you discovered in the previous step:

```sh
devproxy --urls-to-watch "http://localhost:5222/*"
```

> [!TIP]
> You can specify multiple URLs to watch, for example `--urls-to-watch "http://localhost:5222/*" "http://localhost:5223/*"`

## Start your .NET Aspire application to use Dev Proxy

Start your .NET Aspire application and configure it to use Dev Proxy:

```sh
HTTP_PROXY=http://127.0.0.1:8000 dotnet run --project src/MyApp.Host/MyApp.Host.csproj
```

When you use your .NET Aspire application, Dev Proxy intercepts the requests and simulates the scenarios that you configured.
