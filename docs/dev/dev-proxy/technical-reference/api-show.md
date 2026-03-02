---
title: api show
description: api show command reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/27/2026
ms.topic: reference
---

<!-- INTENT: Reference for devproxy api show command -->
<!-- COMMAND: devproxy api show -->

# api show

Displays information about the Dev Proxy REST API endpoints.

## Synopsis

```text
devproxy api show [options]

Options:
  --output <format>   Output format: text|json (default: text)
  -h, --help          Show help
```

## Usage

```console
devproxy api show
```

## Options

| Name | Description | Allowed values | Default |
| ---- | ----------- | -------------- | ------- |
| `--output <format>` | Output format | `text`, `json` | `text` |
| `-?, -h, --help` | Show help and usage information | n/a | n/a |

## Examples

### Show Dev Proxy API information

```console
devproxy api show
```

### Get API information as JSON

```console
devproxy api show --output json
```

## Remarks

Dev Proxy exposes a REST API at `http://127.0.0.1:{api-port}` (default port `8897`) with Swagger documentation at `http://127.0.0.1:{api-port}/swagger`.

The `api show` command lists the available REST API endpoints. The API is available while Dev Proxy is running. Use the CLI for lifecycle operations (start, stop, configure), and use the REST API for runtime operations (proxy state, recording, mock management).

The following endpoints are available:

| Method | Endpoint | Description |
| ------ | -------- | ----------- |
| `GET` | `/proxy` | Get proxy status |
| `POST` | `/proxy` | Start or stop the proxy |
| `POST` | `/proxy/mockRequest` | Send a mock request |
| `POST` | `/proxy/stopProxy` | Stop the proxy |
| `GET` | `/proxy/jwtToken` | Get a JSON Web Token (JWT) |
| `GET` | `/proxy/rootCertificate` | Get the root certificate |
| `GET` | `/proxy/logs` | Get recorded request logs |

For the full API reference, see [Proxy API](./proxy-api.md).
