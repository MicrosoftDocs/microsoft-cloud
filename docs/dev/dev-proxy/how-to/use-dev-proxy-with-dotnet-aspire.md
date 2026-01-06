---
title: Use Dev Proxy with .NET Aspire applications
description: How to use Dev Proxy with Use Dev Proxy with .NET Aspire applications
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/03/2026
---

<!-- INTENT: Use Dev Proxy with .NET Aspire -->
<!-- SOLUTION: Configure HttpClient to use Dev Proxy in Aspire app -->
<!-- RESULT: .NET Aspire HTTP requests intercepted by Dev Proxy -->
<!-- PLUGINS: various -->
<!-- JOB: intercept-requests -->
<!-- TIME: 15 minutes -->

# Use Dev Proxy with .NET Aspire applications

> **At a glance**  
> **Goal:** Use Dev Proxy with .NET Aspire  
> **Time:** 15 minutes  
> **Plugins:** Various  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md), [.NET Aspire](/dotnet/aspire/get-started/aspire-overview)

[.NET Aspire](/dotnet/aspire/get-started/aspire-overview) is an opinionated, cloud ready stack for building observable, production ready, distributed applications. It's built on top of .NET and provides a modern, fast, and scalable platform for building web applications.

To use Dev Proxy with your .NET Aspire application, use the [DevProxy.Hosting NuGet package](https://www.nuget.org/packages/DevProxy.Hosting). The package provides Dev Proxy .NET Aspire extensions to conveniently integrate Dev Proxy into your .NET Aspire application.

## Install the Dev Proxy .NET Aspire extensions NuGet package

To install the Dev Proxy .NET Aspire extensions NuGet package, run the following command in the root folder of your .NET Aspire application:

```console
dotnet add package DevProxy.Hosting
```

Using the Dev Proxy .NET Aspire extensions package, you can integrate Dev Proxy either from the locally installed executable or from a Docker container.

## Integrate Dev Proxy from the locally installed executable

If you have Dev Proxy installed locally, the most convenient way to integrate it into your .NET Aspire application is to reference the local executable. The following code snippet shows how to integrate Dev Proxy from the locally installed executable with the .NET Aspire starter application.

> [!IMPORTANT]
> When you configure Dev Proxy to use the local executable, ensure that the executable is available on all machines where you run your application. If you want to use Dev Proxy in a containerized environment, consider using the Docker container instead.

```csharp
using DevProxy.Hosting;

var builder = DistributedApplication
    .CreateBuilder(args);

// Add an API service to the application
var apiService = builder.AddProject<Projects.AspireStarterApp_ApiService>("apiservice")
    .WithHttpsHealthCheck("/health");

var devProxy = builder.AddDevProxyExecutable("devproxy")
    .WithConfigFile(".devproxy/config/devproxy.json")
    .WithUrlsToWatch(() => [$"{apiService.GetEndpoint("https").Url}/*"]);

// Add a web frontend project and configure it to use Dev Proxy
builder.AddProject<Projects.AspireStarterApp_Web>("webfrontend")
    .WithExternalHttpEndpoints()
    .WithHttpsHealthCheck("/health")
    .WithEnvironment("HTTPS_PROXY", devProxy.GetEndpoint(DevProxyResource.ProxyEndpointName))
    .WithReference(apiService)
    .WaitFor(apiService)
    .WaitFor(devProxy);

// Build and run the application
builder.Build().Run();
```

First, using the Dev Proxy .NET Aspire extensions, you add a Dev Proxy service to your application. The `AddDevProxyExecutable` method specifies the name of the Dev Proxy executable. Using the `WithConfigFile` method, you specify the path to the Dev Proxy configuration file. Using the `WithUrlsToWatch` method, you specify the list of URLs to watch. In this example, you want Dev Proxy to intercept requests that the web app makes to the API service.

> [!IMPORTANT]
> Notice, that the `WithUrlsToWatch` method accepts a function that returns a list of URLs to watch. This is because the API service endpoint is not available when you configure Dev Proxy, so you cannot pass the URL directly. Instead, you use a lambda expression that returns the URL of the API service when it's available.

Next, in the web app, you use the `HTTPS_PROXY` environment variable to configure the web app to use Dev Proxy. Using the `WaitFor` method you instruct the web app to wait for Dev Proxy to be available before starting.

## Integrate Dev Proxy from a Docker container

Alternatively, you can integrate Dev Proxy into your .NET Aspire application from a Docker container. Using the Dev Proxy Docker image is convenient, because .NET Aspire automatically pull the image if it's not available locally. The downside is, that there are a few more steps to configure Dev Proxy in your application.

The following code snippet shows how to integrate Dev Proxy from a Docker container with the .NET Aspire starter application.

```csharp
using DevProxy.Hosting;

var builder = DistributedApplication
    .CreateBuilder(args);

// Add an API service to the application
var apiService = builder.AddProject<Projects.AspireStarterApp_ApiService>("apiservice")
    .WithHttpsHealthCheck("/health");

// Add Dev Proxy as a container resource
var devProxy = builder.AddDevProxyContainer("devproxy")
    // specify the Dev Proxy configuration file; relative to the config folder
    .WithConfigFile("./devproxy.json")
    // mount the local folder with PFX certificate for intercepting HTTPS traffic
    .WithCertFolder(".devproxy/cert")
    // mount the local folder with Dev Proxy configuration
    .WithConfigFolder(".devproxy/config")
    // let Dev Proxy intercept requests to the API service
    .WithUrlsToWatch(() => [$"{apiService.GetEndpoint("https").Url}/*"]);

// Add a web frontend project and configure it to use Dev Proxy
builder.AddProject<Projects.AspireStarterApp_Web>("webfrontend")
    .WithExternalHttpEndpoints()
    .WithHttpsHealthCheck("/health")
    // set the HTTPS_PROXY environment variable to the Dev Proxy endpoint
    .WithEnvironment("HTTPS_PROXY", devProxy.GetEndpoint(DevProxyResource.ProxyEndpointName))
    .WithReference(apiService)
    .WaitFor(apiService)
    .WaitFor(devProxy);

// Build and run the application
builder.Build().Run();
```

The basic steps are the same as when using the locally installed executable. The main difference is how you specify the configuration file and the certificate for intercepting HTTPS traffic.

When integrating Dev Proxy from a Docker container, you need to mount the local folders with the configuration file and the certificate into the container. In this example, in your .NET Aspire solution, you have the following folder structure:

```plaintext
AspireStarterApp
├── .devproxy
│   ├── cert
│   │   └── rootCert.pfx
│   └── config
│       └── devproxy.json
├── Projects
│   ├── AspireStarterApp_ApiService
│   └── AspireStarterApp_Web
└── AspireStarterApp.sln
```

The `cert` folder contains the Personal Information Exchange (PFX) certificate that Dev Proxy uses to intercept HTTPS traffic.

> [!IMPORTANT]
> You must trust the certificate in the `cert` folder on your machine, or requests to the API service will fail. Also, for Dev Proxy to load the certificate, it must be in the PFX format, must be named `rootCert.pfx`, and must not be protected with a password.

The `config` folder contains the Dev Proxy configuration file and other Dev Proxy files such as mocks or errors.

Because you're mounting the certificate and configuration files to separate volumes in the container, they must be stored in separate folders.

## Use Dev Proxy with the .NET Aspire starter application

After you start the application, Dev Proxy shows as a resource in the application.

:::image type="content" source="../media/aspire-dashboard-resource.png" alt-text="Screenshot of the .NET Aspire dashboard showing application resources including Dev Proxy." lightbox="../media/aspire-dashboard-resource.png":::

When you use the web application so that it makes requests to the API service, Dev Proxy intercepts the requests and handles according to your configuration. You can see the Dev Proxy output in the **Console** section of the .NET Aspire dashboard.

:::image type="content" source="../media/aspire-logs.png" alt-text="Screenshot of the .NET Aspire dashboard showing Dev Proxy console output." lightbox="../media/aspire-logs.png":::

## See also

- [Use Dev Proxy with .NET applications](./use-dev-proxy-with-dotnet.md)
- [Use Dev Proxy with .NET Azure Functions](./use-dev-proxy-with-dotnet-azure-functions.md)
- [Use Dev Proxy in a Docker container](./use-dev-proxy-in-docker-container.md)
