---
title: Mock responses
description: How to simulate API responses
author: garrytrinder
ms.author: garrytrinder
ms.date: 04/16/2024
---

# Mock responses

To define mock responses, create a file named `mocks.json` in the current working directory. This file allows you to define a specific set of mocks for each project that you work with. The file contains an object with a `mocks` array containing [mock](../technical-reference/mockresponseplugin.md#response-object) objects.

> [!TIP]
> Instead of creating the mocks file manually, you can use the [`MockGeneratorPlugin`](../technical-reference/mockgeneratorplugin.md) to generate the mocks file based on the intercepted requests.

The following configuration demonstrates two mock responses for retrieving information about the current user. When you request information about the current user, proxy responds with a mock response. When you request the information about the user's photo, proxy returns a 404 status code.

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v0.14.1/mockresponseplugin.schema.json",
  "mocks": [
    {
      "request": {
        "url": "https://graph.microsoft.com/v1.0/me",
        "method": "GET"
      },
      "response": {
        "body": {
          "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#users/$entity",
          "businessPhones": ["+1 412 555 0109"],
          "displayName": "Megan Bowen",
          "givenName": "Megan",
          "jobTitle": "Auditor",
          "mail": "MeganB@M365x214355.onmicrosoft.com",
          "mobilePhone": null,
          "officeLocation": "12/1110",
          "preferredLanguage": "en-US",
          "surname": "Bowen",
          "userPrincipalName": "MeganB@M365x214355.onmicrosoft.com",
          "id": "48d31887-5fad-4d73-a9f5-3c356e68a038"
        },
        "headers": [
          {
            "name": "content-type",
            "value": "application/json; odata.metadata=minimal"
          }
        ]
      }
    },
    {
      "request": {
        "url": "https://graph.microsoft.com/v1.0/me/photo",
        "method": "GET"
      },
      "response": {
        "statusCode": 404
      }
    }
  ]
}
```

## Order precedence

Mocks are matched in the order in which they're defined in the `mocks.json` file. If you define multiple responses with the same URL and method, the first matching response is used.

When you use the following configuration, proxy responds to all `GET` requests to `https://graph.microsoft.com/v1.0/me/photo` with `500 Internal Server Error`.

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v0.14.1/mockresponseplugin.schema.json",
  "mocks": [
    {
      "request": {
        "url": "https://graph.microsoft.com/v1.0/me/photo",
        "method": "GET"
      },
      "response": {
        "statusCode": 500
      }
    },
    {
      "request": {
        "url": "https://graph.microsoft.com/v1.0/me/photo",
        "method": "GET"
      },
      "response": {
        "statusCode": 404
      }
    }
  ]
}
```

## Wildcard support

The proxy supports the use of wildcards in the URL property. You can use the asterisk character (`*`) to match any series of characters in the URL.

When you use the following configuration, proxy responds to all requests to get any user's profile with the same response.

```json
{
  "request": {
    "url": "https://graph.microsoft.com/v1.0/users/*"
  },
  "response": {
    "body": {
      "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#users/$entity",
      "businessPhones": ["+1 425 555 0109"],
      "displayName": "Adele Vance",
      "givenName": "Adele",
      "jobTitle": "Product Marketing Manager",
      "mail": "AdeleV@M365x214355.onmicrosoft.com",
      "mobilePhone": null,
      "officeLocation": "18/2111",
      "preferredLanguage": "en-US",
      "surname": "Vance",
      "userPrincipalName": "AdeleV@M365x214355.onmicrosoft.com",
      "id": "87d349ed-44d7-43e1-9a83-5f2406dee5bd"
    }
  }
}
```

When you use the following configuration, proxy returns the same image from disk when you request to get the binary of any user's photo.

```json
{
  "request": {
    "url": "https://graph.microsoft.com/v1.0/users/*/photo/$value"
  },
  "response": {
    "body": "@picture.jpg",
    "headers": [
      {
        "name": "content-type",
        "value": "image/jpeg"
      }
    ]
  }
}
```

When you use the following configuration, proxy returns the same response when you request to get the current user's profile with any query string parameter.

```json
{
  "request": {
    "url": "https://graph.microsoft.com/v1.0/me?*"
  },
  "response": {
    "body": {
      "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#users/$entity",
      "businessPhones": [
        "+1 412 555 0109"
      ],
      "displayName": "Megan Bowen",
      "givenName": "Megan",
      "jobTitle": "Auditor",
      "mail": "MeganB@M365x214355.onmicrosoft.com",
      "mobilePhone": null,
      "officeLocation": "12/1110",
      "preferredLanguage": "en-US",
      "surname": "Bowen",
      "userPrincipalName": "MeganB@M365x214355.onmicrosoft.com",
      "id": "48d31887-5fad-4d73-a9f5-3c356e68a038"
    }
  }
},
```

## Respond with contents of a file

To keep your mocks file clean and organized, you can store the contents of the response in a separate file and reference it in the mocks file. To instruct Dev Proxy, to load the mock response body from a file, set the `body` property to `@` followed by the file path relative to the mocks file.

For example, the following mock response configuration, instructs Dev Proxy to respond to any request to `https://graph.microsoft.com/v1.0/me` with the contents of the `response.json` file located in the same folder as the mocks file.

```json
{
  "request": {
    "url": "https://graph.microsoft.com/v1.0/me",
    "method": "GET"
  },
  "response": {
    "body": "@response.json",
    "headers": [
      {
        "name": "content-type",
        "value": "application/json; odata.metadata=minimal"
      }
    ]
  }
}
```

Using the `@`-token works with text and [binary files](./mock-responses-that-return-binary-data.md).

## Microsoft Graph Batch support

Dev Proxy supports mocking responses that are sent in batch requests to Microsoft Graph.

There are no special requirements for including responses to batch requests in your mock files, however if a request isn't matched with a mocked response, a `502 Bad Gateway` response is returned.

## Unmocked request support

Dev Proxy supports throwing an error when proxy intercepts an unmocked request. The ability to fail unmocked requests is useful for identifying requests that you missed in your mocks file.

To enable this feature, add and enable the `blockUnmockedRequests` setting to [MockResponsePlugin](../technical-reference/MockResponsePlugin.md) config section in the [devproxyrc](../technical-reference/devproxyrc.md) file.

```json
{
  "mocksPlugin": {
    "mocksFile": "mocks.json",
    "blockUnmockedRequests": true
  }
}
```

When an unmocked request is intercepted, a `502 Bad Gateway` response is returned.

## Next step

Learn more about the MockResponsePlugin.

> [!div class="nextstepaction"]
> [MockResponsePlugin](../technical-reference/mockresponseplugin.md)

## Samples

See also the related Dev Proxy samples:

- [Microsoft Graph mocks from Microsoft Graph API docs](https://adoption.microsoft.com/sample-solution-gallery/sample/pnp-devproxy-microsoft-graph-docs-mocks/)
- [Microsoft Graph mocks from Microsoft Graph API docs with sandbox data](https://adoption.microsoft.com/sample-solution-gallery/sample/pnp-devproxy-microsoft-graph-sandbox-mocks/)
- [Simulate creating a Microsoft Graph connector and its schema](https://adoption.microsoft.com/sample-solution-gallery/sample/pnp-devproxy-microsoft-graph-connector/)
