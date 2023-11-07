To define mock responses, create a file named `responses.json` in the current working directory.

This file allows you to define a specific set of responses for each project that you work with.

The file contains an object with a `responses` array containing [response](https://github.com/microsoft/m365-developer-proxy/wiki/Response-object) objects.

The ZIP file with proxy binaries contains a [responses.sample.json](https://github.com/microsoft/m365-developer-proxy/blob/main/m365-developer-proxy/responses.sample.json) file, which contains examples of mocked responses.

The following configuration demonstrates two mock responses for retrieving information about the current user. When you request information about the current user, proxy responds with a mock response. When you request the information about the user's photo, proxy returns a 404 status code.

```json
{
  "responses": [
    {
      "url": "https://graph.microsoft.com/v1.0/me",
      "method": "GET",
      "responseBody": {
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
      "responseHeaders": {
        "content-type": "application/json; odata.metadata=minimal"
      }
    },
    {
      "url": "https://graph.microsoft.com/v1.0/me/photo",
      "method": "GET",
      "responseCode": 404
    }
  ]
}
```

## Order precedence

Mocks are matched in the order in which they're defined in the `responses.json` file. If you define multiple responses with the same URL and method, the first matching response is used.

When you use the following configuration, proxy responds to all `GET` requests to `https://graph.microsoft.com/v1.0/me/photo` with `500 Internal Server Error`.

```json
{
  "responses": [
    {
      "url": "https://graph.microsoft.com/v1.0/me/photo",
      "method": "GET",
      "responseCode": 500
    },
    {
      "url": "https://graph.microsoft.com/v1.0/me/photo",
      "method": "GET",
      "responseCode": 404
    }
  ]
}
```

## Wildcard support

The proxy supports the use of wildcards the URL field.

The following examples demonstrate how to use the `*` and `?` wildcards.

When you use the following configuration, proxy responds to all requests to get any user's profile with the same response.

```json
{
  "url": "https://graph.microsoft.com/v1.0/users/*",
  "responseBody": {
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
```

When you use the following configuration, proxy returns the same image from disk when you request to get the binary of any user's photo.

```json
{
  "url": "https://graph.microsoft.com/v1.0/users/*/photo/$value",
  "responseBody": "@picture.jpg",
  "responseHeaders": {
    "content-type": "image/jpeg"
  }
}
```

When you use the following configuration, proxy returns the same response when you request to get the current user's profile with any query string parameter.

```json
{
  "url": "https://graph.microsoft.com/v1.0/me?*",
  "responseBody": {
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
},
```

## n-th request support

In v0.12, we introduced support for mocking n-th request and extended the [response](../technical-reference//Response-object.md) object with a new property called `nth`.

Using the following mock file as an example, we can see that it contains two responses to the same request URL. Proxy uses the first response that uses the `nth` property, when it intercepts a request with a matching URL for the second time. For all other requests, proxy returns the second response.

> ℹ️ Responses with the `nth` property should be first. Proxy uses responses based on the first match.

```json
{
  "responses": [
    {
      "url": "https://graph.microsoft.com/v1.0/external/connections/*/operations/*",
      "method": "GET",
      "nth": 2,
      "responseCode": 200,
      "responseBody": {
        "id": "1.neu.0278337E599FC8DBF5607ED12CF463E4.6410CCF8F6DB8758539FB58EB56BF8DC",
        "status": "completed",
        "error": null
      }
    },
    {
      "url": "https://graph.microsoft.com/v1.0/external/connections/*/operations/*",
      "method": "GET",
      "responseCode": 200,
      "responseBody": {
        "id": "1.neu.0278337E599FC8DBF5607ED12CF463E4.6410CCF8F6DB8758539FB58EB56BF8DC",
        "status": "inprogress",
        "error": null
      }
    }
  ]
}
```

## Microsoft Graph Batch support

In v0.12, we introduced support for mocking responses that are sent in batch requests to Microsoft Graph.

There are no special requirements for including responses to batch requests in your mock files, however if a request isn't matched with a mocked response, a `502 Bad Gateway` response is returned.

## Unmocked request support

In v0.12, we introduced support for throwing an error when proxy intercepts an unmocked request. The ability to fail unmocked requests is useful for identifying requests that you missed in your mocks file.

To enable this feature, add and enable the `blockUnmockedRequests` setting to [MockResponsePlugin](../technical-reference/MockResponsePlugin.md) config section in the [m365proxyrc](../technical-reference/m365proxyrc.md) file.

```json
{
  "mocksPlugin": {
    "mocksFile": "responses.json",
    "blockUnmockedRequests": true
  }
}
```

When an unmocked request is intercepted, a `502 Bad Gateway` response is returned.
