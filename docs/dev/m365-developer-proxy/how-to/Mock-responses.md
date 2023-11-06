To define mock responses, create a file named `responses.json` in the current working directory.

This file allows you to define a specific set of responses for each project that you work with.

The file contains an object with a `responses` array containing [response](https://github.com/microsoft/m365-developer-proxy/wiki/Response-object) objects.

[responses.sample.json](https://github.com/microsoft/m365-developer-proxy/blob/main/m365-developer-proxy/responses.sample.json) is provided in the download ZIP which contains examples of mocked responses.

Using following configuration demonstrates a simple example that will return a mocked response when a request is made to get the current user details and return a 404 status code when a request is made to return the current users photo details.

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

Mocks are matched in the order in which they are defined in the `responses.json` file. If you define multiple responses with the same URL and method, the first matching response will be used.

Using the following configuration, all `GET` requests to `https://graph.microsoft.com/v1.0/me/photo` would respond with `500 Internal Server Error`.

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

Using the following configuration, all requests to get any users profile details will return the same response.

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

Using the following configuration, requests to get the binary of any users photo will return the same image from disk in the response.

```json
{
  "url": "https://graph.microsoft.com/v1.0/users/*/photo/$value",
  "responseBody": "@picture.jpg",
  "responseHeaders": {
    "content-type": "image/jpeg"
  }
}
```

Using the following configuration, requests to get the current users profile contains any query string parameter, will return the same response.

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

In v0.12, we introduced support for mocking n-th request and extended the [response](./Response-object) object with a new property called `nth`.

Using the following mock file as an example, we can see that it contains two responses to the same request URL. The first response, that uses the `nth` property, will be used when proxy detects the second request, for all other requests the proxy will return the second response.

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

There are no special requirements for including responses to batch requests in your mock files, however if a request is not matched with a mocked response, a `502 Bad Gateway` response is returned.

## Unmocked request support

In v0.12, we introduced support for throwing an error when an unmocked request is intercepted by the proxy, this is useful for identifying requests that you may have missed in your mocks file.

To enable this feature, add and enable the `blockUnmockedRequests` setting to [MockResponsePlugin](./MockResponsePlugin) config section in the [m365proxyrc](./m365proxyrc) file.

```json
{
  "mocksPlugin": {
    "mocksFile": "responses.json",
    "blockUnmockedRequests": true
  }
}
```

When an unmocked request is intercepted, a `502 Bad Gateway` response is returned.