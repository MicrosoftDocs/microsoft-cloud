---
title: Simulate a CRUD API secured with an API key
description: How to simulate a CRUD API secured with an API key
author: garrytrinder
ms.author: garrytrinder
ms.date: 05/13/2026
---

<!-- INTENT: Simulate CRUD API with API key auth -->
<!-- SOLUTION: Configure CrudApiPlugin with API key auth settings -->
<!-- RESULT: CRUD API requires valid API key -->
<!-- PLUGINS: CrudApiPlugin -->
<!-- JOB: mock-api -->
<!-- TIME: 10 minutes -->

# Simulate a CRUD API secured with an API key

> **At a glance**  
> **Goal:** Simulate CRUD API with API key auth  
> **Time:** 10 minutes  
> **Plugins:** [CrudApiPlugin](../technical-reference/crudapiplugin.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

When building apps, you often interact with backend APIs. Sometimes, these APIs aren't yet available, or other teams are updating them to meet the latest requirements. To avoid waiting, you typically create a mock API that returns the data you need. While this approach unblocks you, it requires you to spend time on building an API that you eventually replace with the real one. It gets even more complicated, when you need to secure your API with an API key. To avoid wasting time, you can use Dev Proxy to simulate a CRUD API and speed up development.

Using the [`CrudApiPlugin`](../technical-reference/crudapiplugin.md), you can [simulate a CRUD (Create, Read, Update, Delete) API](./simulate-crud-api.md) with an in-memory data store. Using a simple configuration file, you can define which URLs your mock API supports and what data it returns. The plugin also supports CORS for cross-domain usage from client-side applications. The plugin also supports API key authentication, so you can secure your mock API with an API key and test that your app sends the key correctly.

## Scenario

Say, you're building an app that allows users to manage customers. To get the data, you need to call the `/customers` endpoint of the backend API. The API is secured with an API key. To avoid waiting for the backend team to finish their work, you decide to use Dev Proxy to simulate the API and return the data you need.

## Before you begin

You start by [creating a simulated CRUD API with customer data](./simulate-crud-api.md). After confirming that the API works, you can secure it with an API key.

## Example 1: Simulate a CRUD API secured with an API key in a header

In the first example, you secure the whole API with an API key that clients send in an HTTP header.

In the `customers-api.json` file, add information about API key authentication.

**File:** `customers-api.json`

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.0.0/crudapiplugin.apifile.schema.json",
  "baseUrl": "https://api.contoso.com/v1/customers",
  "dataFile": "customers-data.json",
  "auth": "apiKey",
  "apiKeyAuthConfig": {
    "apiKey": "my-secret-key",
    "headerName": "X-API-Key"
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

By setting the `auth` property to `apiKey` you specify, that the API is secured with an API key. In the `apiKeyAuthConfig` property, you specify the configuration details. The `apiKey` property specifies the valid API key, and the `headerName` property specifies the HTTP header where the plugin looks for the key.

If you try to call the API without the `X-API-Key` header set to `my-secret-key`, you get a `401 Unauthorized` response.

## Example 2: Simulate a CRUD API secured with an API key in a query parameter

In some APIs, clients send the API key as a query-string parameter. You can simulate this behavior by configuring the `queryParameterName` property.

Update the `customers-api.json` file as follows:

**File:** `customers-api.json`

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.0.0/crudapiplugin.apifile.schema.json",
  "baseUrl": "https://api.contoso.com/v1/customers",
  "dataFile": "customers-data.json",
  "auth": "apiKey",
  "apiKeyAuthConfig": {
    "apiKey": "my-secret-key",
    "queryParameterName": "api_key"
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

In this example, the plugin looks for the API key in the `api_key` query-string parameter. For example, calling `https://api.contoso.com/v1/customers?api_key=my-secret-key` succeeds, while calling `https://api.contoso.com/v1/customers` returns a `401 Unauthorized` response.

## Example 3: Simulate a CRUD API that accepts an API key from both header and query parameter

You can also configure the plugin to accept the API key from both a header and a query parameter. The plugin checks the header first. If the header doesn't contain the API key, the plugin checks the query parameter.

Update the `customers-api.json` file as follows:

**File:** `customers-api.json`

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v3.0.0/crudapiplugin.apifile.schema.json",
  "baseUrl": "https://api.contoso.com/v1/customers",
  "dataFile": "customers-data.json",
  "auth": "apiKey",
  "apiKeyAuthConfig": {
    "apiKey": "my-secret-key",
    "headerName": "X-API-Key",
    "queryParameterName": "api_key"
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

In this example, requests that include the API key in either the `X-API-Key` header or the `api_key` query parameter are authorized.

## Next step

Learn more about the CrudApiPlugin.

> [!div class="nextstepaction"]
> [CrudApiPlugin](../technical-reference/crudapiplugin.md#apikeyauthconfig-properties)

## Samples

See also the related Dev Proxy samples:

- [CRUD APIs with Northwind database data](https://adoption.microsoft.com/sample-solution-gallery/sample/pnp-devproxy-northwinddb/)
