---
title: Simulate a CRUD API secured with Microsoft Entra
description: How to simulate a CRUD API secured with Microsoft Entra
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/06/2026
---

<!-- INTENT: Simulate CRUD API with Entra auth -->
<!-- SOLUTION: Configure CrudApiPlugin with Entra auth settings -->
<!-- RESULT: CRUD API requires valid Entra tokens -->
<!-- PLUGINS: CrudApiPlugin -->
<!-- JOB: mock-api -->
<!-- TIME: 20 minutes -->

# Simulate a CRUD API secured with Microsoft Entra

> **At a glance**  
> **Goal:** Simulate CRUD API with Entra auth  
> **Time:** 20 minutes  
> **Plugins:** [CrudApiPlugin](../technical-reference/crudapiplugin.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

When building apps, you often interact with backend APIs. Sometimes, these APIs aren't yet available, or other teams are updating them to meet the latest requirements. To avoid waiting, you typically create a mock API that returns the data you need. While this approach unblocks you, it requires you to spend time on building an API that you eventually replace with the real one. It gets even more complicated, when you need to secure your API with Microsoft Entra. To avoid wasting time, you can use Dev Proxy to simulate a CRUD API and speed up development.

Using the [`CrudApiPlugin`](../technical-reference/crudapiplugin.md), you can [simulate a CRUD (Create, Read, Update, Delete) API](./simulate-crud-api.md) with an in-memory data store. Using a simple configuration file, you can define which URLs your mock API supports and what data it returns. The plugin also supports CORS for cross-domain usage from client-side applications. The plugin also supports Microsoft Entra authentication, so you can secure your mock API with Microsoft Entra and implement the same authentication flow for your app as in your production environment.

## Scenario

Say, you're building an app that allows users to manage customers. To get the data, you need to call the `/customers` endpoint of the backend API. The API is secured with Microsoft Entra. To avoid waiting for the backend team to finish their work, you decide to use Dev Proxy to simulate the API and return the data you need.

## Before you begin

You start by [creating a simulated CRUD API with customer data](./simulate-crud-api.md). After confirming that the API works, you can secure it with Microsoft Entra.

## Example 1: Simulate a CRUD API secured with Microsoft Entra using a single scope

In the first example, you secure the whole API with a single scope. No matter if users need to get information about customers or update them, they use the same permission.

In the `customers-api.json` file, add information about Entra.

**File:** `customers-api.json`

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/crudapiplugin.apifile.schema.json",
  "baseUrl": "https://api.contoso.com/v1/customers",
  "dataFile": "customers-data.json",
  "auth": "entra",
  "entraAuthConfig": {
    "audience": "https://api.contoso.com",
    "issuer": "https://login.microsoftonline.com/contoso.com",
    "scopes": ["api://contoso.com/user_impersonation"]
  },
  "actions": [
    {
      "action": "getAll"
    },
    {
      "action": "getOne",
      "url": "/{customer-id}",
      "query": "$.[?(@.id == {customer-id})]"
    },
    {
      "action": "create"
    },
    {
      "action": "merge",
      "url": "/{customer-id}",
      "query": "$.[?(@.id == {customer-id})]"
    },
    {
      "action": "delete",
      "url": "/{customer-id}",
      "query": "$.[?(@.id == {customer-id})]"
    }
  ]
}
```

By setting the `auth` property to `entra` you specify, that the API is secured with Microsoft Entra. In the `entraAuthConfig` property, you specify the configuration details. The `audience` property specifies the audience of the API, the `issuer` property specifies the issuer of the tokens, and the `scopes` property specifies the scopes required to access the API. Because you define `scopes` at the root level of the API file, all actions require the same scope.

If you try to call the API without a token with the specified audience, issuer, and scopes, you get a `401 Unauthorized` response.

> [!NOTE]
> At this stage, Dev Proxy doesn't validate the token. It only checks if the token is present and has the required audience, issuer, and scopes. This is convenient during early development, when you don't have a real Microsoft Entra app registration yet and can't get a real token.

## Example 2: Simulate a CRUD API secured with Microsoft Entra using different scopes for different actions

In many cases, different API operations require different permissions. For example, getting information about customers might require a different permission than updating them. In this example, you secure different API actions with different scopes.

Update the `customers-api.json` file as follows:

**File:** `customers-api.json` (with action-level scopes)

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/crudapiplugin.apifile.schema.json",
  "baseUrl": "https://api.contoso.com/v1/customers",
  "dataFile": "customers-data.json",
  "auth": "entra",
  "entraAuthConfig": {
    "audience": "https://api.contoso.com",
    "issuer": "https://login.microsoftonline.com/contoso.com"
  },
  "actions": [
    {
      "action": "getAll",
      "auth": "entra",
      "entraAuthConfig": {
        "scopes": ["api://contoso.com/customer.read"]
      }
    },
    {
      "action": "getOne",
      "url": "/{customer-id}",
      "query": "$.[?(@.id == {customer-id})]",
      "auth": "entra",
      "entraAuthConfig": {
        "scopes": ["api://contoso.com/customer.read"]
      }
    },
    {
      "action": "create",
      "auth": "entra",
      "entraAuthConfig": {
        "scopes": ["api://contoso.com/customer.write"]
      }
    },
    {
      "action": "merge",
      "url": "/{customer-id}",
      "query": "$.[?(@.id == {customer-id})]",
      "auth": "entra",
      "entraAuthConfig": {
        "scopes": ["api://contoso.com/customer.write"]
      }
    },
    {
      "action": "delete",
      "url": "/{customer-id}",
      "query": "$.[?(@.id == {customer-id})]",
      "auth": "entra",
      "entraAuthConfig": {
        "scopes": ["api://contoso.com/customer.write"]
      }
    }
  ]
}
```

This time, you don't specify the `scopes` at the root level of the API file. Instead, you specify them for each action. This way, you can secure different actions with different scopes. For example, getting information about customers requires the `api://contoso.com/customer.read` scope, while updating customers requires the `api://contoso.com/customer.write` scope.

## Validate tokens

Dev Proxy allows you to simulate a CRUD API secured with Microsoft Entra, and check that you're using a valid token. Validating token is convenient when you have an app registration in Microsoft Entra, but the team is still building the API. It allows you to more accurately test your app.

If you want Dev Proxy to validate the access token, to the `entraAuthConfig` property add the `validateSigningKey` property and set it to `true`:

**File:** `customers-api.json` (with token validation)

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/crudapiplugin.apifile.schema.json",
  "baseUrl": "https://api.contoso.com/v1/customers",
  "dataFile": "customers-data.json",
  "auth": "entra",
  "entraAuthConfig": {
    "audience": "https://api.contoso.com",
    "issuer": "https://login.microsoftonline.com/contoso.com",
    "scopes": ["api://contoso.com/user_impersonation"],
    "validateSigningKey": true
  },
  "actions": [
    {
      "action": "getAll"
    },
    {
      "action": "getOne",
      "url": "/{customer-id}",
      "query": "$.[?(@.id == {customer-id})]"
    },
    {
      "action": "create"
    },
    {
      "action": "merge",
      "url": "/{customer-id}",
      "query": "$.[?(@.id == {customer-id})]"
    },
    {
      "action": "delete",
      "url": "/{customer-id}",
      "query": "$.[?(@.id == {customer-id})]"
    }
  ]
}
```

If you try to call the API with a self-crafted token, you get a `401 Unauthorized` response. Dev Proxy allows only requests with a valid token issued by Microsoft Entra.

## Next step

Learn more about the CrudApiPlugin.

> [!div class="nextstepaction"]
> [CrudApiPlugin](../technical-reference/crudapiplugin.md#entraauthconfig-properties)

## Samples

See also the related Dev Proxy samples:

- [CRUD APIs with Northwind database data](https://adoption.microsoft.com/sample-solution-gallery/sample/pnp-devproxy-northwinddb/)
