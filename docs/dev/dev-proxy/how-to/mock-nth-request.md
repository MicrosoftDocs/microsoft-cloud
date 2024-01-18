---
title: Mock nth request
description: How to simulate different responses from the same endpoint
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/18/2024
---

# Mock nth request

In v0.12, we introduced support for mocking n-th request and extended the [mock request](../technical-reference/Response-object.md) object with a new property called `nth`.

> [!TIP]
> Download this preset by running in the terminal `devproxy preset get microsoft-graph-connector`.

Using the following mock file as an example, we can see that it contains two mocks to the same request URL. Proxy uses the first response that uses the `nth` property, when it intercepts a request with the specified URL for the second time. For all other requests, the proxy returns the second response.

> [!TIP]
> Mocks with the `nth` property should be first. Proxy uses mocks based on the first match.

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v1.0/mockresponseplugin.schema.json",
  "mocks": [
    {
      "request": {
        "url": "https://graph.microsoft.com/v1.0/external/connections/*/operations/*",
        "method": "GET",
        "nth": 2
      },
      "response": {
        "statusCode": 200,
        "body": {
          "id": "1.neu.0278337E599FC8DBF5607ED12CF463E4.6410CCF8F6DB8758539FB58EB56BF8DC",
          "status": "completed",
          "error": null
        }
      }
    },
    {
      "request": {
        "url": "https://graph.microsoft.com/v1.0/external/connections/*/operations/*",
        "method": "GET"
      },
      "response": {
        "statusCode": 200,
        "body": {
          "id": "1.neu.0278337E599FC8DBF5607ED12CF463E4.6410CCF8F6DB8758539FB58EB56BF8DC",
          "status": "inprogress",
          "error": null
        }
      }
    }
  ]
}
```

## Next step

Learn more about the MockResponsePlugin.

> [!div class="nextstepaction"]
> [MockResponsePlugin](../technical-reference/mockresponseplugin.md)

## Samples

See also the related Dev Proxy samples:

- [Simulate creating a Microsoft Graph connector and its schema](https://adoption.microsoft.com/sample-solution-gallery/sample/pnp-devproxy-microsoft-graph-connector/)
