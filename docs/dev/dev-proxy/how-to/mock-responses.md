---
title: Mock responses
description: How to simulate API responses
author: garrytrinder
ms.author: garrytrinder
ms.date: 04/30/2025
---

# Mock responses

Using Dev Proxy is the easiest way to mock an API. Whether you're building the front-end and the API isn't ready yet, you need to integrate your back-end with an external service, or you want to test your application with different responses, Dev Proxy can help you simulate API responses. What's great about using Dev Proxy is that it doesn't require any changes to your application code. You define mock responses for any API that your application interacts with and Dev Proxy intercepts the requests and responds with the mock responses that you defined.

To mock API responses, you need to do two things:

1. Create a file with mock responses.
2. Configure Dev Proxy to use the mock responses.

> [!TIP]
> If you use Visual Studio Code, consider installing the [Dev Proxy Toolkit](https://aka.ms/devproxy/toolkit) extension. It significantly simplifies working with Dev Proxy configuration files.

## Create a file with mock responses

Dev Proxy mocks API responses using the [`MockResponsePlugin`](../technical-reference/mockresponseplugin.md). The plugin allows you to define a set of mock responses. You define the mocks in a [separate file](../technical-reference/mockresponseplugin.md#mocks-file-properties). The following code snippet demonstrates a simple mock response for a `GET` request to `https://jsonplaceholder.typicode.com/posts/1`.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/mockresponseplugin.schema.json",
  "mocks": [
    {
      "request": {
        "url": "https://jsonplaceholder.typicode.com/posts/1",
        "method": "GET"
      },
      "response": {
        "statusCode": 200,
        "body": {
          "userId": 1,
          "id": 1,
          "title": "sunt aut facere repellat provident occaecati excepturi optio reprehenderit",
          "body": "quia et suscipit\nsuscipit recusandae consequuntur expedita et cum\nreprehenderit molestiae ut ut quas totam\nnostrum rerum est autem sunt rem eveniet architecto"
        },
        "headers": [
          {
            "name": "Date",
            "value": "Wed, 19 Feb 2025 09:03:37 GMT"
          },
          {
            "name": "Content-Type",
            "value": "application/json; charset=utf-8"
          },
          {
            "name": "Content-Length",
            "value": "292"
          },
          // [...] trimmed for brevity
        ]
      }
    }
  ]
}
```

> [!TIP]
> Instead of creating the mocks file manually, you can use the [`MockGeneratorPlugin`](../technical-reference/mockgeneratorplugin.md) to generate the mocks file based on the intercepted requests.

### Order precedence

Dev Proxy matches mocks in the order in which you define them in the mocks file. If you define multiple responses with the same URL and method, Dev Proxy uses the first matching response.

When you use the following configuration, proxy responds to all `GET` requests to `https://graph.microsoft.com/v1.0/me/photo` with `500 Internal Server Error`.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/mockresponseplugin.schema.json",
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

### Wildcard support

Dev Proxy supports the use of wildcards in the URL property. You can use the asterisk character (`*`) to match any series of characters in the URL.

When you use the following configuration, Dev Proxy responds to all requests to get any user's profile with the same response.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/mockresponseplugin.mocksfile.schema.json",
  "mocks": [
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
  ]
}
```

When you use the following configuration, Dev Proxy returns the same image from disk when you request to get the binary of any user's photo.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/mockresponseplugin.mocksfile.schema.json",
  "mocks": [
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
  ]
}
```

When you use the following configuration, Dev Proxy returns the same response when you request to get the current user's profile with any query string parameter.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/mockresponseplugin.mocksfile.schema.json",
  "mocks": [
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
    }
  ]
}
```

### Respond with contents of a file

To keep your mocks file clean and organized, you can store the contents of the response in a separate file and reference it in the mocks file. To instruct Dev Proxy, to load the mock response body from a file, set the `body` property to `@` followed by the file path relative to the mocks file.

For example, the following mock response configuration, instructs Dev Proxy to respond to any request to `https://graph.microsoft.com/v1.0/me` with the contents of the `response.json` file located in the same folder as the mocks file.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/mockresponseplugin.mocksfile.schema.json",
  "mocks": [
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
  ]
}
```

Using the `@`-token works with text and [binary files](./mock-responses-that-return-binary-data.md).

## Configure Dev Proxy to use the mock responses

After you create the mocks file, you need to configure Dev Proxy to use the mock responses. To configure Dev Proxy to mock responses, add the `MockResponsePlugin` to the list of plugins in the [devproxyrc](../technical-reference/devproxyrc.md) file.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "MockResponsePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "mockResponsePlugin"
    }
  ],
  "urlsToWatch": [
    "https://jsonplaceholder.typicode.com/*"
  ],
  "mockResponsePlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/mockresponseplugin.schema.json",
    "mocksFile": "mocks.json"
  },
  "logLevel": "information",
  "newVersionNotification": "stable",
  "showSkipMessages": true
}
```

First, you add the `MockResponsePlugin` to the list of plugins. You include a reference to its configuration section where you specify the path to your mocks file.

When you start Dev Proxy, it reads the mocks file and uses the mock responses to respond to the requests that match the defined mocks.

## Unmocked request support

Dev Proxy supports throwing an error when proxy intercepts an unmocked request. The ability to fail unmocked requests is useful for identifying requests that you missed in your mocks file.

To enable this feature, add and enable the `blockUnmockedRequests` setting to [MockResponsePlugin](../technical-reference/MockResponsePlugin.md) config section in the [devproxyrc](../technical-reference/devproxyrc.md) file.

```json
{
  "mocksPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/mockresponseplugin.schema.json",
    "mocksFile": "mocks.json",
    "blockUnmockedRequests": true
  }
}
```

When Dev Proxy intercepts a request which it can't mock, it returns a `502 Bad Gateway` response.

## Next step

Learn more about the MockResponsePlugin.

> [!div class="nextstepaction"]
> [MockResponsePlugin](../technical-reference/mockresponseplugin.md)

## Samples

See also the related Dev Proxy samples:

- [Microsoft Graph mocks from Microsoft Graph API docs](https://adoption.microsoft.com/sample-solution-gallery/sample/pnp-devproxy-microsoft-graph-docs-mocks/)
- [Microsoft Graph mocks from Microsoft Graph API docs with sandbox data](https://adoption.microsoft.com/sample-solution-gallery/sample/pnp-devproxy-microsoft-graph-sandbox-mocks/)
- [Simulate creating a Microsoft Graph connector and its schema](https://adoption.microsoft.com/sample-solution-gallery/sample/pnp-devproxy-microsoft-graph-connector/)
