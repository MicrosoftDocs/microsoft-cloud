---
title: Proxy API
description: Overview of the Dev Proxy API
author: waldekmastykarz
ms.author: wmastyka
ms.date: 04/02/2025
---

<!-- INTENT: Control Dev Proxy programmatically via its REST API -->

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

### `POST /proxy/jwtToken`

Generates a JSON Web Token (JWT).

Request:

```http
POST http://localhost:8897/proxy/jwtToken
Content-Type: application/json

{
  "name": "Dev Proxy",
  "audiences": [
    "https://myserver.com"
  ],
  "issuer": "dev-proxy",
  "roles": [
    "admin"
  ],
  "scopes": [
    "Post.Read",
    "Post.Write"
  ],
  "claims": {
    "claim1": "value",
    "claim2": "value"
  },
  "validFor": 60
}
```

> [!NOTE]
> Registered claims (for example, `iss`, `sub`, `aud`, `exp`, `nbf`, `iat`, `jti`) are automatically added to the token. If you specify any of these claims in the request, the API ignores the values you provide.

Response:

```text
200 OK

{
  "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1bmlxdWVfbmFtZSI6IkRldiBQcm94eSIsInN1YiI6IkRldiBQcm94eSIsImp0aSI6IjkyZjM5YzciLCJzY3AiOlsiUG9zdC5SZWFkIiwiUG9zdC5Xcml0ZSJdLCJyb2xlcyI6ImFkbWluIiwiY2xhaW0xIjoidmFsdWUiLCJjbGFpbTIiOiJ2YWx1ZSIsImF1ZCI6Imh0dHBzOi8vbXlzZXJ2ZXIuY29tIiwibmJmIjoxNzI3MTk4MjgyLCJleHAiOjE3MjcyMDE4ODIsImlhdCI6MTcyNzE5ODI4MiwiaXNzIjoiZGV2LXByb3h5In0.E_Gj9E58OrAh9uHgc-TW8DYfq8YHFrhaUTpKA4yXEIg"
}
```

### `POST /proxy/mockrequest`

Raises a mock request. Equivalent of pressing <kbd>w</kbd> in the console where Dev Proxy is running.

Request:

```http
POST http://localhost:8897/proxy/mockrequest
```

Response:

```text
202 Accepted
```

### `GET /proxy/rootCertificate?format=crt`

Downloads the public key of the root certificate in PEM (Privacy Enhanced Mail) format that Dev Proxy uses to decrypt HTTPS requests. This API is helpful when you want to trust the root certificate on your host while running Dev Proxy in a Docker container.

At this moment, the only supported format is `crt`. The API returns a 400 Bad Request error if you specify any other format or don't specify a format at all.

Request:

```http
GET http://localhost:8897/proxy/rootCertificate?format=crt
```

Response:

```text
content-type: application/x-x509-ca-cert

-----BEGIN CERTIFICATE-----
[base64 encoded certificate]
-----END CERTIFICATE-----
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

## `JwtOptions`

Options for generating a JWT token.

| Property | Type | Description |
|----------|------|-------------|
| `name` | `string` | The name of the user to create the token for |
| `audience` | `string[]` | The audiences to create the token for |
| `issuer` | `string[]` | The issuer of the token |
| `roles` | `string[]` | A role claim to add to the token |
| `scopes` | `string[]` | A scope claim to add to the token |
| `claims` | `KeyValuePair` | Claims to add to the token |
| `validFor` | `number` | The duration (in minutes) for which the token is valid |
| `signingKey` | `string` | The key to use to sign the token. Must be at least 32 characters long. If not specified, uses a randomly generated key. |

## `JwtInfo`

Information about a JWT token.

| Property | Type | Description |
|----------|------|-------------|
| `token` | `string` | The JWT token |
