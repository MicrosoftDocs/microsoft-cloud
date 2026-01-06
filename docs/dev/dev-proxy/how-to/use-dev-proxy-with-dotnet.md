---
title: Use Dev Proxy with .NET applications
description: How to use Dev Proxy with .NET applications
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/03/2026
---

<!-- INTENT: Configure .NET applications to use Dev Proxy -->
<!-- SOLUTION: Set HTTP_PROXY/HTTPS_PROXY environment variables -->
<!-- RESULT: .NET HTTP requests routed through Dev Proxy -->
<!-- JOB: intercept-requests -->
<!-- TIME: 5 minutes -->

# Use Dev Proxy with .NET applications

> **At a glance**  
> **Goal:** Use Dev Proxy with .NET applications  
> **Time:** 5 minutes  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md), .NET application

.NET automatically uses the system proxy settings. If you want to use Dev Proxy with your .NET application, you don't need to make any changes to your application. Start Dev Proxy and it will automatically intercept web requests made by your .NET application.

Following is a simple .NET app that makes a web request to `https://jsonplaceholder.typicode.com/posts`:

```csharp
var client = new HttpClient();
var response = await client.GetStringAsync("https://jsonplaceholder.typicode.com/posts");
Console.WriteLine(response);
```

To simulate errors from this request, start Dev Proxy with the default preset, which is configured for intercepting requests to `https://jsonplaceholder.typicode.com/*`.

```console
devproxy
```

When you run your .NET application, Dev Proxy intercepts the request and returns a 429 error.

:::image type="content" source="../media/throttling-json-placeholder-api.png" alt-text="Screenshot of a command prompt with Dev Proxy simulating throttling error on a web request from a .NET application." lightbox="../media/throttling-json-placeholder-api.png":::

## See also

- [Use Dev Proxy with .NET applications in Docker containers](./use-dev-proxy-with-dotnet-docker.md) - Docker setup
- [Use Dev Proxy with .NET Azure Functions](./use-dev-proxy-with-dotnet-azure-functions.md) - Azure Functions
- [Glossary](../concepts/glossary.md) - Dev Proxy terminology
