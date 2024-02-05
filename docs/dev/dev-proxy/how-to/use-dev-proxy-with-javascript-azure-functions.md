---
title: Use Dev Proxy with JavaScript Azure Functions
description: How to use Dev Proxy with JavaScript Azure Functions
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/05/2024
---

# Use Dev Proxy with JavaScript Azure Functions

If you build Azure Functions using JavaScript and want to use Dev Proxy, follow the general guidance for [using Dev Proxy with Node.js applications](./use-dev-proxy-with-nodejs.md).

To be able to easily switch between using Dev Proxy in development and not using it in production, you can best configure the proxy in your Azure Functions app using environment variables. Change the `local.settings.json` file to include the `HTTPS_PROXY` environment variable. Additionally, disable certificate validation to allow the Azure Functions app to trust the self-signed certificate used by Dev Proxy.

```json
{
  "IsEncrypted": false,
  "Values": {
    "FUNCTIONS_WORKER_RUNTIME": "node",
    "AzureWebJobsFeatureFlags": "EnableWorkerIndexing",
    "AzureWebJobsStorage": "UseDevelopmentStorage=true",
    "HTTP_PROXY": "http://127.0.0.1:8000",
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
    const options = process.env.HTTP_PROXY ? { agent: new HttpsProxyAgent(process.env.HTTP_PROXY) } : {};
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
