---
title: Use Dev Proxy with JavaScript Azure Functions
description: How to use Dev Proxy with JavaScript Azure Functions
author: waldekmastykarz
ms.author: wmastyka
ms.date: 06/03/2025
---

# Use Dev Proxy with JavaScript Azure Functions

If you build Azure Functions using JavaScript and want to use Dev Proxy, follow the general guidance for [using Dev Proxy with Node.js applications](./use-dev-proxy-with-nodejs.md).

> [!IMPORTANT]
> To prevent Azure Functions from failing on startup, start Dev Proxy without registering it as a system proxy either by using the `--as-system-proxy false` option or by configuring `asSystemProxy` to `false` in the `devproxyrc.json` file. If you register Dev Proxy as a system proxy, Azure Functions fails on startup with an error message similar to:
>
> ```text
> Grpc.Core.RpcException: 'Status(StatusCode="Internal", Detail="Error starting gRPC call. HttpRequestException: Requesting HTTP version 2.0 with version policy RequestVersionOrHigher while unable to establish HTTP/2 connection.", DebugException="System.Net.Http.HttpRequestException: Requesting HTTP version 2.0 with version policy RequestVersionOrHigher while unable to establish HTTP/2 connection.")'
> ```

To be able to easily switch between using Dev Proxy in development and not using it in production, you can best configure the proxy in your Azure Functions app using environment variables. Change the `local.settings.json` file to include the `HTTPS_PROXY` environment variable. Additionally, disable certificate validation to allow the Azure Functions app to trust the self-signed certificate used by Dev Proxy.

```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "node",
    "AzureWebJobsFeatureFlags": "EnableWorkerIndexing",
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    "HTTPS_PROXY": "http://127.0.0.1:8000",
    "NODE_TLS_REJECT_UNAUTHORIZED": "0"
  }
}
```

In your Azure Functions app, use the `process.env` object to read the environment variables and configure the proxy for your HTTP requests.

```javascript
import { app, HttpRequest, HttpResponseInit, InvocationContext } from "@azure/functions";
import fetch from 'node-fetch';
import { HttpsProxyAgent } from 'https-proxy-agent';

export async function MyFnHttpTrigger(request: HttpRequest, context: InvocationContext): Promise<HttpResponseInit> {
    const options = process.env.HTTPS_PROXY ? { agent: new HttpsProxyAgent(process.env.HTTPS_PROXY) } : {};
    const resp = await fetch('https://jsonplaceholder.typicode.com/posts', options);
    const data = await resp.json();
    return {
      status: 200,
      jsonBody: data
    };
};

app.http('MyFnHttpTrigger', {
    methods: ['GET', 'POST'],
    authLevel: 'anonymous',
    handler: MyFnHttpTrigger
});
```
