---
title: Use Dev Proxy with .NET Azure Functions
description: How to use Dev Proxy with .NET Azure Functions
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/03/2026
---

<!-- INTENT: Use Dev Proxy with .NET Azure Functions -->
<!-- SOLUTION: Set HTTP_PROXY environment variable for Functions -->
<!-- RESULT: Azure Functions HTTP requests intercepted -->
<!-- PLUGINS: various -->
<!-- JOB: intercept-requests -->
<!-- TIME: 10 minutes -->

# Use Dev Proxy with .NET Azure Functions

> **At a glance**  
> **Goal:** Use Dev Proxy with .NET Azure Functions  
> **Time:** 10 minutes  
> **Plugins:** Various  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md), Azure Functions Core Tools

If you build Azure Functions using .NET and want to use Dev Proxy, follow the general guidance for [using Dev Proxy with .NET applications](./use-dev-proxy-with-dotnet.md).

> [!IMPORTANT]
> To prevent Azure Functions from failing on startup, start Dev Proxy without registering it as a system proxy either by using the `--as-system-proxy false` option or by configuring `asSystemProxy` to `false` in the `devproxyrc.json` file. If you register Dev Proxy as a system proxy, Azure Functions fails on startup with an error message similar to:
>
> ```text
> Grpc.Core.RpcException: 'Status(StatusCode="Internal", Detail="Error starting gRPC call. HttpRequestException: Requesting HTTP version 2.0 with version policy RequestVersionOrHigher while unable to establish HTTP/2 connection.", DebugException="System.Net.Http.HttpRequestException: Requesting HTTP version 2.0 with version policy RequestVersionOrHigher while unable to establish HTTP/2 connection.")'
> ```

To be able to easily switch between using Dev Proxy in development and not using it in production, you can best configure the proxy in your Azure Functions app using environment variables. Change the `local.settings.json` file to include the `HTTPS_PROXY` environment variable.

**File:** local.settings.json

```json
{
  "IsEncrypted": false,
  "Values": {
    "HTTPS_PROXY": "http://127.0.0.1:8000"
  }
}
```

`HttpClient` in .NET automatically picks up the `HTTPS_PROXY` environment variable and uses it to configure the proxy for outgoing HTTP requests.

**File:** MyFn.cs

```csharp
using Microsoft.Azure.Functions.Worker;
using Microsoft.Extensions.Logging;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;

namespace dotnet_azure_fn;

public class MyFn(ILogger<MyFn> logger, IHttpClientFactory httpClientFactory)
{
    private readonly ILogger<MyFn> _logger = logger;
    private readonly HttpClient _httpClient = httpClientFactory.CreateClient();

    [Function("MyFn")]
    public async Task<IActionResult> Run([HttpTrigger(AuthorizationLevel.Anonymous, "get", "post")] HttpRequest req)
    {
        _logger.LogInformation("C# HTTP trigger function processed a request.");

        var result = await _httpClient.GetAsync("https://jsonplaceholder.typicode.com/posts");
        if (result.IsSuccessStatusCode)
        {
            return new OkObjectResult(await result.Content.ReadAsStringAsync());
        }
        else
        {
            _logger.LogError("HTTP request failed.");
            return new StatusCodeResult(StatusCodes.Status500InternalServerError);
        }
    }
}
```

## See also

- [Use Dev Proxy with .NET applications](./use-dev-proxy-with-dotnet.md)
- [Use Dev Proxy with JavaScript Azure Functions](./use-dev-proxy-with-javascript-azure-functions.md)
- [Use Dev Proxy with .NET applications running in Docker](./use-dev-proxy-with-dotnet-docker.md)
