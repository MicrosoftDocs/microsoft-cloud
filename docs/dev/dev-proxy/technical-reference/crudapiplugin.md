---
title: CrudApiPlugin
description: CrudApiPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 04/30/2025
---

# CrudApiPlugin

Simulates a CRUD API with an in-memory data store. Sends JSON responses. Supports CORS for cross-domain usage from client-side applications. Optionally, simulates CRUD APIs secured with Microsoft Entra.

:::image type="content" source="../media/crud-api-plugin.png" alt-text="Screenshot of a command prompt with Dev Proxy simulating a CRUD API." lightbox="../media/crud-api-plugin.png":::

## Plugin instance definition

```json
{
  "name": "CrudApiPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
  "configSection": "customersApi"
}
```

## Configuration example

```json
{
  "customersApi": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/crudapiplugin.schema.json",
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

Following are several examples of API files that define a CRUD API for information about customers.

### Anonymous CRUD API

Following is an example of an API file that defines an anonymous CRUD API for information about customers.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/crudapiplugin.schema.json",
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

### CRUD API secured with Microsoft Entra using a single scope

Following is an example of an API file that defines a CRUD API for information about customers secured with Microsoft Entra. All actions are secured with one scope. CrudApiPlugin validates the token audience, issuer, and the scope.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/crudapiplugin.schema.json",
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

### CRUD API secured with Microsoft Entra using specific scopes

Following is an example of an API file that defines a CRUD API for information about customers secured with Microsoft Entra. Actions are secured with specific scopes. CrudApiPlugin validates the token audience, issuer, and the scope.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/crudapiplugin.schema.json",
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
      "action": "update",
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

## API file properties

| Property | Description | Required |
|----------|-------------|----------|
| `actions` | List of actions that the API supports. | Yes |
| `auth` | Determines if the API is secured or not. Allowed values: `none`, `entra`. Default `none` | No |
| `baseUrl` | Base URL where Dev Proxy exposes the URL. Dev Proxy prepends the base URL to URLs you define in actions. | Yes |
| `dataFile` | Path to the file that contains the data for the API. | Yes |
| `entraAuthConfig` | Configuration for Microsoft Entra authentication. | Yes, when you configure `auth` to `entra` |

You can refer to the `dataFile` using an absolute- or a relative path. Dev Proxy resolves relative paths relatively to the API definition file.

The `dataFile` must define a JSON array. The array can be empty or it can contain an initial set of objects.

### EntraAuthConfig properties

When you configure the `auth` property to `entra`, you must define the `entraAuthConfig` property. If you don't define it, CrudApiPlugin shows a warning and the API is available anonymously.

You can define `entraAuthConfig` on the API file and on each API action. When you define it on the API file, it applies to all actions. When you define it on an action, it overrides the API file configuration for this specific action.

The `entraAuthConfig` property has the following properties.

| Property | Description | Required | Default |
|----------|-------------|----------|---------|
| `audience` | Specify the valid audience for the token. When specified, CrudApiPlugin compares the audience from the token with this audience. If they're different, CrudApiPlugin returns a 401 Unauthorized response. | No | None |
| `issuer` | Specify the valid token issuer. When specified, CrudApiPlugin compares the issuer from the token with this issuer. If they're different, CrudApiPlugin returns a 401 Unauthorized response. | No | None |
| `scopes` | Specify the array of valid scopes. When specified, CrudApiPlugin controls if either of the scopes is present on the token. If neither of the scopes is present, CrudApiPlugin returns a 401 Unauthorized response. | No | None |
| `roles` | Specify the array of valid roles. When specified, CrudApiPlugin controls if either of the roles is present on the token. If neither of the roles is present, CrudApiPlugin returns a 401 Unauthorized response. | No | None |
| `validateLifetime` | Set to `true` for the CrudApiPlugin to validate if the token hasn't expired. When CrudApiPlugin detects an expired token, it returns a 401 Unauthorized response. | No | `false` |
| `validateSigningKey` | Set to `true` for the CrudApiPlugin to validate if the token is authentic. When CrudApiPlugin detects a token with an invalid signature (for example, because you modified the token manually), it returns a 401 Unauthorized response. | No | `false` |

### Action properties

Each action in the `actions` list has the following properties.

| Property | Description | Required | Default |
|----------|-------------|----------|---------|
| `action` | Defines how Dev Proxy interacts with the data. Possible values: `getAll`, `getOne`, `getMany`, `create`, `merge`, `update`, `delete`. | Yes | None |
| `auth` | Determines if the action is secured or not. Allowed values: `none`, `entra`. | No | `none` |
| `entraAuthConfig` | Configuration for Microsoft Entra authentication. | Yes, when you configure `auth` to `entra` | None |
| `method` | HTTP method that Dev Proxy uses to expose the action. | No | Depends on the action |
| `query` | Newtonsoft [JSONPath](https://www.newtonsoft.com/json/help/html/QueryJsonSelectTokenJsonPath.htm) query that Dev Proxy uses to find the data in the data file. | No | Empty |
| `url` | URL where Dev Proxy exposes the action on. Dev Proxy appends the URL to the base URL. | No | Empty |

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
| `getOne` | Returns a single item from the data file. Fails when the query matches multiple items. | `GET` |
| `getMany` | Returns multiple items from the data file. Returns an empty array if the query doesn't match any items. | `GET` |
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

> [!div class="nextstepaction"]
> [Simulate a CRUD API](../how-to/simulate-crud-api.md)
