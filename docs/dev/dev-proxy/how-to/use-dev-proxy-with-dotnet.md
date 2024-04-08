---
title: Use Dev Proxy with .NET applications
description: How to use Dev Proxy with .NET applications
author: waldekmastykarz
ms.author: wmastyka
ms.date: 04/08/2024
---

# Use Dev Proxy with .NET applications

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

:::image type="content" source="../media/throttling-json-placeholder-api.png" alt-text="Screenshot of a terminal with Dev Proxy simulating throttling error on a web request from a .NET application." lightbox="../media/throttling-json-placeholder-api.png":::
