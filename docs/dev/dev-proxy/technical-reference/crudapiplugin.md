---
title: CrudApiPlugin
description: CrudApiPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 12/22/2023
---

# CrudApiPlugin

Simulates a CRUD API with an in-memory data store. Sends JSON responses. Supports CORS for cross-domain usage from client-side applications.

:::image type="content" source="../media/crud-api-plugin.png" alt-text="Screenshot of a terminal with Dev Proxy simulating a CRUD API." lightbox="../media/crud-api-plugin.png":::

## Plugin instance definition

```json
{
  "name": "CrudApiPlugin",
  "enabled": true,
  "pluginPath": "plugins\\dev-proxy-plugins.dll",
  "configSection": "customersApi"
}
```

## Configuration example

```json
{
  "customersApi": {
    "apiFile": "customers-api.json"
  }
}
```

## Configuration properties

| Property | Description |
|----------|-------------|
| `apiFile` | Path to the file that contains the definition of the CRUD API |

## Command line options

None

## API file example

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
      "action": "update",
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

## API file properties

| Property | Description | Required |
|----------|-------------|----------|
| `baseUrl` | Base URL where Dev Proxy exposes the URL. Dev Proxy prepends the base URL to URLs you define in actions. | Yes |
| `dataFile` | Path to the file that contains the data for the API. | Yes |
| `actions` | List of actions that the API supports. | Yes |

You can refer to the `dataFile` using an absolute- or a relative path. Dev Proxy resolves relative paths relatively to the API definition file.

The `dataFile` must define a JSON array. The array can be empty or it can contain an initial set of objects.

### Action properties

Each action in the `actions` list has the following properties.

| Property | Description | Required | Default |
|----------|-------------|----------|---------|
| `action` | Defines how Dev Proxy interacts with the data. Possible values: `getAll`, `getOne`, `create`, `merge`, `update`, `delete`. | Yes | None |
| `url` | URL where Dev Proxy exposes the action on. Dev Proxy appends the URL to the base URL. | No | Empty |
| `method` | HTTP method that Dev Proxy uses to expose the action. | No | Depends on the action |
| `query` | Newtonsoft [JSONPath](https://www.newtonsoft.com/json/help/html/QueryJsonSelectTokenJsonPath.htm) query that Dev Proxy uses to find the data in the data file. | No | Empty |

The URL specified in the `url` property can contain parameters. You define parameters by wrapping the parameter name in curly braces, for example, `{customer-id}`. When routing the request, Dev Proxy replaces the parameter with the value from the request URL.

You can use the same parameter in the query. For example, if you define the `url` as `/customers/{customer-id}` and the `query` as `$.[?(@.id == {customer-id})]`, Dev Proxy replaces the `{customer-id}` parameter in the query with the value from the request URL.

> [!IMPORTANT]
> Dev Proxy implements JSONPath in the `query` property using Newtonsoft.Json. There are some limitations to using it such as, it supports only single quotes. Before submitting an issue, be sure to validate your query.

When the plugin can't find the data in the data file using the query, it returns a `404 Not Found` response.

Each action type has a default HTTP method. You can override the default by specifying the `method` property. For example, the `get` action type has a default method of `GET`. If you want to use `POST` instead, specify the `method` property as `POST`.

The `actions` array defined a collection of actions that you want to mock. You can define multiple actions for the same HTTP method and action type. For example, you can define two `getOne` actions, one that retrieves a customer by their ID and the other by their email address. Be sure to define unique URLs for each action.

#### Actions

Dev Proxy supports the following actions for CRUD APIs.

| Action | Description | Default method |
|--------|-------------|----------------|
| `getAll` | Returns all items from the data file. | `GET` |
| `getOne` | Returns a single item from the data file. | `GET` |
| `create` | Adds a new item to the data collection. | `POST` |
| `merge` | Merges the data from the request with the data from the data file. | `PATCH` |
| `update` | Replaces the data in the data file with the data from the request. | `PUT` |
| `delete` | Deletes the item from the data file. | `DELETE` |

When you create a new item using a `create` action, the plugin doesn't validate its shape and adds it to the data collection as-is.

### Data file example

```json
[
  {
    "id": 1,
    "firstName": "Ted",
    "lastName": "James",
    "gender": "male",
    "address": "1234 Anywhere St.",
    "city": "Phoenix",
    "state": {
      "abbreviation": "AZ",
      "name": "Arizona"
    },
    "orders": [
      { "productName": "Basketball", "itemCost": 7.99 },
      { "productName": "Shoes", "itemCost": 199.99 }
    ],
    "latitude": 33.299,
    "longitude": -111.963
  },
  {
    "id": 2,
    "firstName": "Michelle",
    "lastName": "Thompson",
    "gender": "female",
    "address": "345 Cedar Point Ave.",
    "city": "Encinitas",
    "state": {
      "abbreviation": "CA",
      "name": "California"
    },
    "orders": [
      { "productName": "Frisbee", "itemCost": 2.99 },
      { "productName": "Hat", "itemCost": 5.99 }
    ],
    "latitude": 33.037,
    "longitude": -117.291
  }
]
```
