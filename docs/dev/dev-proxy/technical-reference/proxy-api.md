---
title: Proxy API
description: Overview of the Dev Proxy API
author: waldekmastykarz
ms.author: wmastyka
ms.date: 08/23/2024
---

# Proxy API

Dev Proxy comes with a web API that allows you to interact with the proxy programmatically. The API is available on the port specified in the [proxy settings](./proxy-settings.md).

## Swagger

The API is documented using Swagger. You can access the Swagger UI by navigating to `http://localhost:<apiPort>/swagger` in your browser.

## Operations

The following list shows available API operations.

### `GET /proxy`

Returns an instance of `ProxyInfo` with information about the currently running Dev Proxy instance.

#### Example: Get information about the currently running Dev Proxy instance

Request:

```http
GET http://localhost:8897/proxy
```

Response:

```text
200 OK

{
  "recording": false,
  "configFile": "/Users/user/dev-proxy/devproxyrc.json"
}
```

### `POST /proxy`

Controls the currently running Dev Proxy instance.

#### Example: start recording

Request:

```http
POST http://localhost:8897/proxy
content-type: application/json

{
  "recording": true
}
```

Response:

```text
200 OK

{
  "recording": true,
  "configFile": "/Users/user/dev-proxy/devproxyrc.json"
}
```

#### Example: stop recording

Request:

```http
POST http://localhost:8897/proxy
content-type: application/json

{
  "recording": false
}
```

Response:

```text
200 OK

{
  "recording": false,
  "configFile": "/Users/user/dev-proxy/devproxyrc.json"
}
```

### `POST /proxy/raisemockrequest`

Raises a mock request. Equivalent of pressing <kbd>w</kbd> in the console where Dev Proxy is running.

Request:

```http
POST http://localhost:8897/proxy/raisemockrequest
```

Response:

```text
202 Accepted
```

### `POST /proxy/stopproxy`

Gracefully shuts down Dev Proxy.

Request:

```http
POST http://localhost:8897/proxy/stopproxy
```

Response:

```text
202 Accepted
```

## Models

### `ProxyInfo`

Information about the currently running Dev Proxy instance.

| Property | Type | Description |
|----------|------|-------------|
| `recording` | `boolean` | Whether the proxy is currently recording requests |
| `configFile` | `string` | Path to the configuration file that Dev Proxy is using (read-only) |
