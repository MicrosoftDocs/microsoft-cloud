---
title: Mock response object
description: Structure of the mock response object
author: garrytrinder
ms.author: garrytrinder
ms.date: 1/09/2024
---

# Mock object

Each mock object consists of two properties:

| Property  | Description | Required |
| --------- | ----------- | :------: |
| `request` | [Request object](#request-object) that defines the request to respond to | yes |
| `response` | [Response object](#response-object) that defines the response to return | yes |

## Request object

Each request has the following properties:

| Property | Description | Required | Default value | Sample value |
| -------- | ----------- | :------: | ------------- | ------------ |
| `url` | Absolute URL to an API endpoint to respond to | yes | | `https://jsonplaceholder.typicode.com/posts` |
| `method` | HTTP verb used to match request with `url` | no | `GET` | `GET` |
| `nth` | Determines that proxy should respond only after when intercepting the request for the nth time | no | | `2` |

### Remarks

Use asterisk (`*`) in the `url` property if you want to match any series of characters in the URL. For example, `https://jsonplaceholder.typicode.com/*` matches `https://jsonplaceholder.typicode.com/posts` and `https://jsonplaceholder.typicode.com/comments`. On runtime, Dev Proxy converts each `*` into a regular expression `.*`.

When defining mocks, place the most specific mocks first. For example, if you have two mocks, one for `https://jsonplaceholder.typicode.com/posts` and one for `https://jsonplaceholder.typicode.com/*`, place the first mock first. Otherwise, Dev Proxy matches the second mock first and return the response for `https://jsonplaceholder.typicode.com/*` for all requests.

Use the `nth` property if you need to send a different to the same request URL. For example, use it to simulate a long-running operation. The first time you call the API, it returns a response with an `inprogress` message. The second time you call the API, it returns a response with the `completed` message. For more information about the `nth` property, see [Mock nth request](../how-to/mock-nth-request.md).

## Response object

Each response has the following properties:

| Property | Description | Required | Default value | Sample value |
| -------- | ------------| :------: | ------------- | ------------ |
| `body` | Body to send as the response to the request | no | _empty_ | `{ "foo": "bar" }` |
| `statusCode` | Response HTTP status code | no | `200` | `404` |
| `headers` | Array of headers to include in the response | no | _empty_ | `[{ name: "content-type", "value": "application/json" }]` |

### Remarks

If you want to return binary data, set the `body` property to a string value that starts with `@` followed by file path relative to the mocks file. For example, `@picture.jpg` returns the image stored in the `picture.jpg` file in the same directory as the mocks file.

## Examples

Respond with body

```json
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
```

Respond with error

```json
{
  "request": {
    "url": "https://graph.microsoft.com/v1.0/me/photo",
    "method": "GET"
  },
  "response": {
    "statusCode": 404
  }
}
```

Respond with binary data

```json
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
```

Respond on `nth` request

```json
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
```
