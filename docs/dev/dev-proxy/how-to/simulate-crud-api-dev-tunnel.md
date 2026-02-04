---
title: Simulate a CRUD API across the internet
description: How to simulate a CRUD API across the internet.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/06/2026
---

<!-- INTENT: Expose CRUD API via dev tunnels -->
<!-- SOLUTION: Combine CrudApiPlugin with dev tunnel port forwarding -->
<!-- RESULT: CRUD API accessible from internet -->
<!-- PLUGINS: CrudApiPlugin -->
<!-- JOB: mock-api -->
<!-- TIME: 15 minutes -->

# Simulate a CRUD API across the internet

> **At a glance**  
> **Goal:** Expose CRUD API via dev tunnels  
> **Time:** 15 minutes  
> **Plugins:** [CrudApiPlugin](../technical-reference/crudapiplugin.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md), [Dev Tunnels](/azure/developer/dev-tunnels/get-started)

Dev Proxy allows you to [simulate CRUD APIs](./simulate-crud-api.md) without having to build them. Simulating APIs using Dev Proxy allows you to save time and speed up development. When you integrate your API with cloud services, you need to expose your API across the internet so that the cloud service can access it. To expose a CRUD API simulated by Dev Proxy across the internet, use [Dev Tunnels](/azure/developer/dev-tunnels/). This article explains how to configure a CRUD API to be exposed across the internet using Dev Tunnels.

> [!TIP]
> The CRUD API in this article is based on the [Northwind database Dev Proxy sample](https://adoption.microsoft.com/sample-solution-gallery/sample/pnp-devproxy-northwinddb).

## Configure CRUD API to be exposed across the internet

To expose a CRUD API simulated by Dev Proxy across the internet, start by configuring the CRUD API.

> [!IMPORTANT]
> At this moment, you can only expose HTTP CRUD APIs across the internet using Dev Tunnels.

### Define the CRUD API data

Create a data file, named `orders-data.json`, that backs the CRUD API, for example:

**File:** orders-data.json

```json
[
  {
    "OrderID": 10248,
    "CustomerID": "VINET",
    "EmployeeID": 5,
    "OrderDate": "1996-07-04T00:00:00",
    "RequiredDate": "1996-08-01T00:00:00",
    "ShippedDate": "1996-07-16T00:00:00",
    "ShipVia": 3,
    "Freight": 32.38,
    "ShipName": "Vins et alcools Chevalier",
    "ShipAddress": "59 rue de l'Abbaye",
    "ShipCity": "Reims",
    "ShipPostalCode": "51100",
    "ShipCountry": "France"
  },
  {
    "OrderID": 10249,
    "CustomerID": "TOMSP",
    "EmployeeID": 6,
    "OrderDate": "1996-07-05T00:00:00",
    "RequiredDate": "1996-08-16T00:00:00",
    "ShippedDate": "1996-07-10T00:00:00",
    "ShipVia": 1,
    "Freight": 11.61,
    "ShipName": "Toms Spezialitäten",
    "ShipAddress": "Luisenstr. 48",
    "ShipCity": "Münster",
    "ShipPostalCode": "44087",
    "ShipCountry": "Germany"
  }
]
```

### Configure the CRUD API

Next, create the API configuration file named `orders-api.json`, where you specify the CRUD API URL, its operations, and data file. Be sure to specify an HTTP URL in the `baseUrl` property:

**File:** orders-api.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/crudapiplugin.apifile.schema.json",
  "baseUrl": "http://api.northwind.com/orders",
  "auth": "none",
  "dataFile": "orders-data.json",
  "actions": [
    {
      "action": "getAll"
    },
    {
      "action": "getOne",
      "url": "/{order-id}",
      "query": "$.[?(@.OrderID == {order-id})]"
    },
    {
      "action": "create"
    },
    {
      "action": "merge",
      "url": "/{order-id}",
      "query": "$.[?(@.OrderID == {order-id})]"
    },
    {
      "action": "delete",
      "url": "/{order-id}",
      "query": "$.[?(@.OrderID == {order-id})]"
    }
  ]
}
```

### Define Dev Proxy configuration

Next, create a Dev Proxy configuration file named `devproxyrc.json` with the `CrudApiPlugin` enabled. Configure Dev Proxy to listen to the URL that you configured for your CRUD API:

**File:** devproxyrc.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
  "plugins": [
    {
      "name": "CrudApiPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "ordersApi"
    }
  ],
  "urlsToWatch": [
    "http://api.northwind.com/*"
  ],
  "ordersApi": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/crudapiplugin.schema.json",
    "apiFile": "orders-api.json"
  }
}
```

### Verify configuration

Verify that the CRUD API is working correctly by running Dev Proxy and sending requests to the CRUD API.

Start Dev Proxy, assuming you saved the Dev Proxy configuration in a file named `devproxyrc.json` in the current working directory:

```console
devproxy
```

Call the CRUD API using curl:

```console
$ curl -x http://127.0.0.1:8000 http://api.northwind.com/orders

[
  {
    "OrderID": 10248,
    "CustomerID": "VINET",
    "EmployeeID": 5,
    "OrderDate": "1996-07-04T00:00:00",
    "RequiredDate": "1996-08-01T00:00:00",
    "ShippedDate": "1996-07-16T00:00:00",
    "ShipVia": 3,
    "Freight": 32.38,
    "ShipName": "Vins et alcools Chevalier",
    "ShipAddress": "59 rue de l'Abbaye",
    "ShipCity": "Reims",
    "ShipPostalCode": "51100",
    "ShipCountry": "France"
  },
  {
    "OrderID": 10249,
    "CustomerID": "TOMSP",
    "EmployeeID": 6,
    "OrderDate": "1996-07-05T00:00:00",
    "RequiredDate": "1996-08-16T00:00:00",
    "ShippedDate": "1996-07-10T00:00:00",
    "ShipVia": 1,
    "Freight": 11.61,
    "ShipName": "Toms Spezialitäten",
    "ShipAddress": "Luisenstr. 48",
    "ShipCity": "Münster",
    "ShipPostalCode": "44087",
    "ShipCountry": "Germany"
  }
]
```

## Expose the CRUD API across the internet

To expose the CRUD API across the internet, start a dev tunnel mapped to the Dev Proxy port. Configure the tunnel to use the host name configured for the CRUD API.

> [!WARNING]
> Allowing anonymous access to a dev tunnel means anyone on the internet is able to connect to your local server, if they can guess the dev tunnel ID.

```console
$ devtunnel host -p 8000 -a --host-header api.northwind.com

Hosting port: 8000
Connect via browser: https://vpfm55qw.euw.devtunnels.ms:8000, https://vpfm55qw-8000.euw.devtunnels.ms
Inspect network activity: https://vpfm55qw-8000-inspect.euw.devtunnels.ms

Ready to accept connections for tunnel: vpfm55qw
```

Call the CRUD API that Dev Proxy simulates via dev tunnel using curl:

```console
$ curl https://vpfm55qw-8000.euw.devtunnels.ms/orders

[
  {
    "OrderID": 10248,
    "CustomerID": "VINET",
    "EmployeeID": 5,
    "OrderDate": "1996-07-04T00:00:00",
    "RequiredDate": "1996-08-01T00:00:00",
    "ShippedDate": "1996-07-16T00:00:00",
    "ShipVia": 3,
    "Freight": 32.38,
    "ShipName": "Vins et alcools Chevalier",
    "ShipAddress": "59 rue de l'Abbaye",
    "ShipCity": "Reims",
    "ShipPostalCode": "51100",
    "ShipCountry": "France"
  },
  {
    "OrderID": 10249,
    "CustomerID": "TOMSP",
    "EmployeeID": 6,
    "OrderDate": "1996-07-05T00:00:00",
    "RequiredDate": "1996-08-16T00:00:00",
    "ShippedDate": "1996-07-10T00:00:00",
    "ShipVia": 1,
    "Freight": 11.61,
    "ShipName": "Toms Spezialitäten",
    "ShipAddress": "Luisenstr. 48",
    "ShipCity": "Münster",
    "ShipPostalCode": "44087",
    "ShipCountry": "Germany"
  }
]
```

## See also

- [Simulate a CRUD API](./simulate-crud-api.md)
- [CrudApiPlugin](../technical-reference/crudapiplugin.md)
- [Dev Tunnels documentation](/azure/developer/dev-tunnels/)
