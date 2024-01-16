---
title: Simulate a CRUD API
description: How to simulate a CRUD API and speed up development with Dev Proxy
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/16/2024
---

# Simulate a CRUD API

When building apps, you often interact with backend APIs. Sometimes, these APIs aren't yet available, or other teams are updating them to meet the latest requirements. To avoid waiting, you typically create a mock API that returns the data you need. While this approach unblocks you, it requires you to spend time on building an API that you eventually replace with the real one. To avoid wasting time, you can use Dev Proxy to simulate a CRUD API and speed up development.

Using the [`CrudApiPlugin`](../technical-reference/crudapiplugin.md), you can simulate a CRUD (Create, Read, Update, Delete) API with an in-memory data store. Using a simple configuration file, you can define which URLs your mock API supports and what data it returns. The plugin also supports CORS for cross-domain usage from client-side applications.

Where the [`MockResponsePlugin`](../technical-reference/mockresponseplugin.md) allows you to define static mock responses, the CrudApiPlugin allows you to define a dynamic mock API that you can use to interact with data and see your changes reflected in the mock data set.

## Scenario

Say, you're building an app that allows users to manage customers. To get the data, you need to call the `/customers` endpoint of the backend API. To avoid waiting for the backend team to finish their work, you decide to use Dev Proxy to simulate the API and return the data you need.

You start with enabling the `CrudApiPlugin` and configuring it to use the `customers-api.json` file.

```json
{
  "name": "CrudApiPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
  "configSection": "customersApi"
}
```

```json
{
  "customersApi": {
    "apiFile": "customers-api.json"
  }
}
```

In the `customers-api.json` file, you define the mock customers API.

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v1.0/crudapiplugin.schema.json",
  "baseUrl": "https://api.contoso.com/v1/customers",
  "dataFile": "customers-data.json",
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

In the `baseUrl` property, you define the base URL of the mock API. In the `dataFile` property, you define the file that contains mock customer data. In the `actions` property, you define the supported actions and how they map to the HTTP methods and URLs. You want to use your API to:

- get all customers, by calling `GET /v1/customers`
- get a single customer, by calling `GET /v1/customers/{customer-id}`
- add a new customer, by calling `POST /v1/customers`,
- update a customer, by calling `PATCH /v1/customers/{customer-id}`,
- delete a customer, by calling `DELETE /v1/customers/{customer-id}`

In your URLs, you use the `{customer-id}` parameter, which the plugin replaces with the actual customer ID from the URL. The plugin also uses the `{customer-id}` parameter in a JSONPath query to look up the customer in the data file.

In the `customers-data.json` file, you define the mock customer data.

```json
[
  {
    "id": 1,
    "name": "Contoso",
    "address": "4567 Main St Buffalo, NY 98052"
  },
  {
    "id": 2,
    "name": "Fabrikam",
    "address": "4567 Main St Buffalo, NY 98052"
  }
]
```

You start Dev Proxy and call the `https://api.contoso.com/v1/customers` endpoint. Dev Proxy intercepts the request and returns the mock customer data.

```json
[
  {
    "id": 1,
    "name": "Contoso",
    "address": "4567 Main St Buffalo, NY 98052"
  },
  {
    "id": 2,
    "name": "Fabrikam",
    "address": "4567 Main St Buffalo, NY 98052"
  }
]
```

## Next step

Learn more about the CrudApiPlugin.

> [!div class="nextstepaction"]
> [CrudApiPlugin](../technical-reference/crudapiplugin.md)
