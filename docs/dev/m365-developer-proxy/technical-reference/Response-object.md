---
title: Response object
description: Structure of the response object
author: garrytrinder
ms.author: garrytrinder
ms.contributors: garrytrinder
ms.date: 11/03/2023
ms.topic: conceptual
ms.service: microsoft-cloud-for-developers

categories:
  - developer-tools
products:
  - microsoft-365
  - microsoft-graph
  - sharepoint-online
  - m365
ms.custom:
  - fcp
  - team=cloud_advocates
---

Each response has the following properties:

| Property          | Description                                                 | Required | Default value | Sample value                          |
| ----------------- | ----------------------------------------------------------- | :------: | ------------- | ------------------------------------- |
| `url`             | Absolute URL to an API endpoint to respond to               |   yes    |               | `https://graph.microsoft.com/v1.0/me` |
| `method`          | Http verb used to match request with `url`   |   yes    |               | `GET`                                 |
| `nth`             | Determines the nth request that the proxy should respond on |    no    | _empty_       | 2                                     |
| `responseBody`    | Body to send as the response to the request                 |    no    | _empty_       | `{ "foo": "bar" }`                    |
| `responseCode`    | Response status code                                        |    no    | `200`         | `404`                                 |
| `responseHeaders` | Collection of headers to include in the response            |    no    | _empty_       | `{ "foo": "bar" }`                    |

### Examples

Respond with body

```json
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
}
```

Respond with error

```json
{
  "url": "https://graph.microsoft.com/v1.0/me/photo",
  "method": "GET",
  "responseCode": 404
}
```

Respond with binary data

```json
{
  "url": "https://graph.microsoft.com/v1.0/users/*/photo/$value",
  "method": "GET",
  "responseBody": "@picture.jpg",
  "responseHeaders": {
    "content-type": "image/jpeg"
  }
}
```

Respond on `nth` request

```json
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
}
```

A sample responses file is bundled with the download ZIP called `response.sample.json`.
