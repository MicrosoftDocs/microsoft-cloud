---
title: Mock multiple responses to the same endpoint
description: How to mock multiple responses to the same endpoint
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/06/2026
---

<!-- INTENT: Return different mocks based on URL patterns -->
<!-- SOLUTION: Use URL wildcards or query params in mock definitions -->
<!-- RESULT: Different responses returned based on URL pattern match -->
<!-- PLUGINS: MockResponsePlugin -->
<!-- JOB: mock-api -->
<!-- TIME: 10 minutes -->

# Mock multiple responses to the same endpoint

> **At a glance**  
> **Goal:** Return different mocks based on URL patterns  
> **Time:** 10 minutes  
> **Plugins:** [MockResponsePlugin](../technical-reference/mockresponseplugin.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

When defining mock responses, you can define a specific URL to mock, but also a URL pattern by replacing part of the URL with an `*` (asterisk), for example:

**File:** `mocks.json`

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/mockresponseplugin.mocksfile.schema.json",
  "mocks": [
    {
      "request": {
        "url": "https://graph.microsoft.com/v1.0/users/*",
        "method":  "GET"
      },
      "response": {
        "body": {
          "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#users/$entity",
          "businessPhones": [
            "+1 425 555 0109"
          ],
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

would respond to`GET` requests for `https://graph.microsoft.com/v1.0/users/bob@contoso.com` and `https://graph.microsoft.com/v1.0/users/steve@contoso.com` with the same mock response.

If a URL of a mock response contains an `*`, the proxy considers it a regular expression, where each `*` is converted into a `.*`, basically matching any sequence of characters. Expanding wildcards is important to keep in mind, because if a pattern is too broad and defined before more specific mocks, it could unintentionally return unexpected responses, for example:

**File:** `mocks.json` (incorrect ordering)

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/mockresponseplugin.mocksfile.schema.json",
  "mocks": [
    {
      "request": {
        "url": "https://graph.microsoft.com/v1.0/users/*",
        "method":  "GET"
      },
      "response": {
        "body": {
          "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#users/$entity",
          "businessPhones": [
            "+1 425 555 0109"
          ],
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
    },
    {
      "request": {
        "url": "https://graph.microsoft.com/v1.0/users/48d31887-5fad-4d73-a9f5-3c356e68a038",
        "method":  "GET"
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

for request `GET https://graph.microsoft.com/v1.0/users/48d31887-5fad-4d73-a9f5-3c356e68a038`, the proxy would return `Adele Vance` instead of `Megan Bowen`, because the asterisk at the end matches any series of characters. The correct way to define these responses, would be to change their order in the array:

**File:** `mocks.json` (correct ordering)

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/mockresponseplugin.mocksfile.schema.json",
  "mocks": [
    {
      "request": {
        "url": "https://graph.microsoft.com/v1.0/users/48d31887-5fad-4d73-a9f5-3c356e68a038",
        "method":  "GET"
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
    {
      "request": {
        "url": "https://graph.microsoft.com/v1.0/users/*",
        "method":  "GET"
      },
      "response": {
        "body": {
          "@odata.context": "https://graph.microsoft.com/v1.0/$metadata#users/$entity",
          "businessPhones": [
            "+1 425 555 0109"
          ],
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

> [!TIP]
> As a rule of thumb, define the mocks with the longest (most specific) URLs first. Put mocks with shorter URLs and URLs with wildcards (less specific) towards the end of the array.

## See also

- [Mock responses](./mock-responses.md)
- [MockResponsePlugin](../technical-reference/mockresponseplugin.md)
- [Mock responses that return binary data](./mock-responses-that-return-binary-data.md)
