---
title: Simulate mock responses across the internet
description: How to simulate mock responses across the internet.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 06/25/2025
---

# Simulate mock responses across the internet

Using Dev Proxy is the easiest way to mock an API. Whether you're building the front-end and the API isn't ready yet, you need to integrate your back-end with an external service, or you want to test your application with different responses, Dev Proxy can help you simulate API responses. When you integrate your API with cloud services, you need to expose your API across the internet so that the cloud service can access it. To expose mock responses simulated by Dev Proxy across the internet, use [Dev Tunnels](/azure/developer/dev-tunnels/). This article explains how to configure mock responses to be exposed across the internet using Dev Tunnels.

## Define mock responses

To expose mock responses simulated by Dev Proxy across the internet, start by configuring the mocks.

> [!IMPORTANT]
> At this moment, Dev Tunnels only support exposing HTTP mock responses across the internet.

### Create mock responses

Create a file named `mocks.json` that contains the mock responses for your custom API. For example, to simulate a `GET` request to `http://api.contoso.com/products`, define:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/mockresponseplugin.mocksfile.schema.json",
  "mocks": [
    {
      "request": {
        "url": "http://api.contoso.com/products",
        "method": "GET"
      },
      "response": {
        "headers": [
          {
            "name": "content-type",
            "value": "application/json"
          }
        ],
        "body": [
          {
            "id": 1,
            "name": "Contoso Coffee Beans",
            "price": 12.99
          },
          {
            "id": 2,
            "name": "Contoso Espresso Machine",
            "price": 249.99
          }
        ]
      }
    }
  ]
}
```

You can also simulate errors, binary responses, or conditional responses using `statusCode`, `nth`, or `bodyFragment`.

### Configure the MockResponsePlugin

Create a Dev Proxy configuration file named `devproxyrc.json` and enable the `MockResponsePlugin`:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "MockResponsePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "mocksPlugin"
    }
  ],
  "urlsToWatch": [
    "http://api.contoso.com/*"
  ],
  "mocksPlugin": {
    "mocksFile": "mocks.json"
  }
}
```

> [!IMPORTANT]
> To match multiple endpoints, use wildcards in `urlsToWatch`. To ensure correct matching, place more specific mocks first in your `mocks.json`.

### Verify configuration

Verify that the mocks are working correctly by running Dev Proxy and sending requests to the mocked API.

Start Dev Proxy, assuming you saved the Dev Proxy configuration in a file named `devproxyrc.json` in the current working directory:

```console
devproxy
```

Test your mock by sending a request through Dev Proxy:

```console
curl -x http://127.0.0.1:8000 http://api.contoso.com/products
```

You should receive the mocked product list defined in `mocks.json`.

## Expose the mock API with Dev Tunnels

To expose mock responses across the internet, start a dev tunnel mapped to the Dev Proxy port. Configure the tunnel to use the host name configured for the mocked API.

> [!WARNING]
> Allowing anonymous access to a dev tunnel means anyone on the internet is able to connect to your local server, if they can guess the dev tunnel ID.

```console
devtunnel host -p 8000 -a --host-header api.contoso.com
```

This command maps the Dev Proxy port to a public HTTP URL. You can now access your mock API from anywhere:

```console
curl http://<your-tunnel-id>-8000.<region>.devtunnels.ms/products
```
