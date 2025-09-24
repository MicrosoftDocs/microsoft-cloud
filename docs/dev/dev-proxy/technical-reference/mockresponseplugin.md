---
title: MockResponsePlugin
description: MockResponsePlugin reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 09/24/2025
---

# MockResponsePlugin

Simulates responses.

:::image type="content" source="../media/mock-response-plugin.png" alt-text="Screenshot of a command prompt with Dev Proxy simulating response for a request to GitHub API." lightbox="../media/mock-response-plugin.png":::

## Plugin instance definition

```json
{
  "name": "MockResponsePlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
  "configSection": "mocksPlugin"
}
```

## Configuration example

```json
{
  "mocksPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/mockresponseplugin.schema.json",
    "mocksFile": "mocks.json"
  }
}
```

## Configuration properties

| Property              | Description                                                        |     Default      |
| --------------------- | ------------------------------------------------------------------ | :--------------: |
| `mocksFile`             | Path to the file containing mock responses                         | `mocks.json` |
| `blockUnmockedRequests` | Return `502 Bad Gateway` response for requests that aren't mocked |     `false`      |

## Command line options

| Name             | Description                                | Default |
| ---------------- | ------------------------------------------ | :-----: |
| `-n, --no-mocks` | Disable loading mock requests              | `false` |
| `--mocks-file`   | Path to the file containing mock responses |    -    |

## Mocks file examples

Following are examples of mock objects.

### Respond with body

Response to a request with a 200 OK response and a JSON body.

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
    }
  ]
}
```

### Respond with error

Respond to a request with a 404 Not Found response.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/mockresponseplugin.mocksfile.schema.json",
  "mocks": [
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

### Respond with binary data

Respond to a request with a binary image loaded from a file on disk.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/mockresponseplugin.mocksfile.schema.json",
  "mocks": [
    {
      "request": {
        "url": "https://graph.microsoft.com/v1.0/users/*/photo/$value",
        "method": "GET"
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

### Respond on `nth` request

Respond to a request only after the second time it's called.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/mockresponseplugin.mocksfile.schema.json",
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
    }
  ]
}
```

### Respond matching the request body

Respond to a request that contains a specific string in the body.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/mockresponseplugin.mocksfile.schema.json",
  "mocks": [
    {
      "request": {
        "url": "https://login.microsoftonline.com/fa15d692-e9c7-4460-a743-29f29522229/oauth2/v2.0/token",
        "method": "POST",
        "bodyFragment": "scope=https%3A%2F%2Fapi.contoso.com%2FDocuments.Read"
      },
      "response": {
        "headers": [
          {
            "name": "Content-Type",
            "value": "application/json; charset=utf-8"
          }
        ],
        "body": {
          "token_type": "Bearer",
          "expires_in": 3599,
          "ext_expires_in": 3599,
          "access_token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJSU..."
        }
      }
    }
  ]
}
```

### Mirror request data in response

Respond with data that mirrors values from the request body using `@request.body.*` placeholders.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.2.0/mockresponseplugin.mocksfile.schema.json",
  "mocks": [
    {
      "request": {
        "url": "https://graph.microsoft.com/v1.0/users",
        "method": "POST"
      },
      "response": {
        "statusCode": 201,
        "body": {
          "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#users/$entity",
          "id": "12345678-1234-1234-1234-123456789abc",
          "businessPhones": "@request.body.businessPhones",
          "displayName": "@request.body.displayName",
          "givenName": "@request.body.givenName",
          "jobTitle": "@request.body.jobTitle",
          "mail": "@request.body.mail",
          "userPrincipalName": "@request.body.userPrincipalName",
          "accountEnabled": "@request.body.accountEnabled",
          "createdDateTime": "2024-01-15T10:30:00Z"
        },
        "headers": [
          {
            "name": "Content-Type",
            "value": "application/json; odata.metadata=minimal"
          },
          {
            "name": "Location",
            "value": "https://graph.microsoft.com/v1.0/users/12345678-1234-1234-1234-123456789abc"
          }
        ]
      }
    }
  ]
}
```

Given the following request:

```http
POST https://graph.microsoft.com/v1.0/users
Content-Type: application/json"

{
  "displayName": "Megan Bowen",
  "userPrincipalName": "MeganB@M365x214355.onmicrosoft.com",
  "accountEnabled": true,
  "givenName": "Megan",
  "surname": "Bowen",
  "jobTitle": "Product Manager"
}
```

The response is:

```http
HTTP/1.1 200 Connection Established
Content-Length: 0

HTTP/1.1 201 Created
Cache-Control: no-store
x-ms-ags-diagnostic: 
Strict-Transport-Security: 
request-id: d28ce889-febd-4c2b-a8b7-0a83e2531467
client-request-id: d28ce889-febd-4c2b-a8b7-0a83e2531467
Date: 9/10/2025 10:28:35?AM
Content-Type: application/json; odata.metadata=minimal
Location: https://graph.microsoft.com/v1.0/users/12345678-1234-1234-1234-123456789abc
OData-Version: 4.0
Content-Length: 648

{
  "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#users/$entity",
  "id": "12345678-1234-1234-1234-123456789abc",
  "businessPhones": null,
  "displayName": "Megan Bowen",
  "givenName": "Megan",
  "jobTitle": "Product Manager",
  "mail": null,
  "mobilePhone": null,
  "officeLocation": null,
  "preferredLanguage": null,
  "surname": "Bowen",
  "userPrincipalName": "MeganB@M365x214355.onmicrosoft.com",
  "accountEnabled": true,
  "createdDateTime": "2024-01-15T10:30:00Z",
  "department": null,
  "companyName": null,
  "city": null,
  "country": null,
  "postalCode": null,
  "state": null,
  "streetAddress": null,
  "usageLocation": null
}
```

## Mocks file properties

| Property  | Description | Required |
| --------- | ----------- | :------: |
| `request` | [Request object](#request-object) that defines the request to respond to | yes |
| `response` | [Response object](#response-object) that defines the response to return | yes |

### Request object

Each request has the following properties:

| Property | Description | Required | Default value | Sample value |
| -------- | ----------- | :------: | ------------- | ------------ |
| `url` | Absolute URL to an API endpoint to respond to | yes | | `https://jsonplaceholder.typicode.com/posts` |
| `method` | HTTP verb used to match request with `url` | no | `GET` | `GET` |
| `nth` | Determines that proxy should respond only after when intercepting the request for the nth time | no | | `2` |
| `bodyFragment` | A string that should be present in the request body | no | | `foo` |

#### Remarks

Use asterisk (`*`) in the `url` property if you want to match any series of characters in the URL. For example, `https://jsonplaceholder.typicode.com/*` matches `https://jsonplaceholder.typicode.com/posts` and `https://jsonplaceholder.typicode.com/comments`. On runtime, Dev Proxy converts each `*` into a regular expression `.*`.

When defining mocks, place the most specific mocks first. For example, if you have two mocks, one for `https://jsonplaceholder.typicode.com/posts` and one for `https://jsonplaceholder.typicode.com/*`, place the first mock first. Otherwise, Dev Proxy matches the second mock first and return the response for `https://jsonplaceholder.typicode.com/*` for all requests.

Use the `nth` property if you need to send a different to the same request URL. For example, use it to simulate a long-running operation. The first time you call the API, it returns a response with an `inprogress` message. The second time you call the API, it returns a response with the `completed` message. For more information about the `nth` property, see [Mock nth request](../how-to/mock-nth-request.md).

Using the `bodyFragment` property, you can match requests based on the body content. For example, if you want to match requests that contain the `foo` string in the body, set the `bodyFragment` property to `foo`. Dev Proxy uses `bodyFragment` only for requests other than `GET`.

### Response object

Each response has the following properties:

| Property | Description | Required | Default value | Sample value |
| -------- | ------------| :------: | ------------- | ------------ |
| `body` | Body to send as the response to the request | no | _empty_ | `{ "foo": "bar" }` |
| `statusCode` | Response HTTP status code | no | `200` | `404` |
| `headers` | Array of headers to include in the response | no | _empty_ | `[{ name: "content-type", "value": "application/json" }]` |

#### Remarks

If you want to return binary data, set the `body` property to a string value that starts with `@` followed by file path relative to the mocks file. For example, `@picture.jpg` returns the image stored in the `picture.jpg` file in the same directory as the mocks file.

To mirror request data in the response, use `@request.body.*` placeholders in the response body. For example, `@request.body.displayName` returns the value of the `displayName` property from the request body. This feature works with nested objects and arrays, allowing you to create dynamic responses that reflect the incoming request data. The placeholder replacement supports various JSON value types including strings, numbers, booleans, and complex objects.

## Next step

> [!div class="nextstepaction"]
> [Mock responses](../how-to/mock-responses.md)
