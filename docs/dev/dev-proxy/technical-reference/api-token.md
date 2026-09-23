---
title: api token
description: api token command reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 09/23/2026
ms.topic: reference
---

<!-- INTENT: Reference for devproxy api token command -->
<!-- COMMAND: devproxy api token -->

# api token

Prints the API bearer token of a running Dev Proxy instance.

## Synopsis

```text
devproxy api token [options]

Options:
  --pid <pid>          Retrieve the token of a specific instance
  --output <format>    Output format: text|json (default: text)
  -h, --help           Show help
```

## Usage

```console
devproxy api token
```

## Arguments

None

## Options

| Name | Description | Allowed values | Default |
| ---- | ----------- | -------------- | ------- |
| `--pid <pid>` | Retrieve the token of a specific Dev Proxy instance by process ID | integer | n/a |
| `--output <format>` | Output format | `text`, `json` | `text` |
| `-?, -h, --help` | Show help and usage information | n/a | n/a |

## Examples

### Get the token of the running instance

```console
devproxy api token
```

### Get the token of a specific instance

```console
devproxy api token --pid 12345
```

### Get token information as JSON

```console
devproxy api token --output json
```

The JSON output contains the `pid`, `apiUrl`, and `token` properties.

## Remarks

Dev Proxy must already be running. When one instance is running, the command selects it automatically. When multiple instances are running, specify an instance using `--pid`. To list running instances and their process IDs, use `devproxy status`.

The command reads the credential for the current user and prints the secret to standard output, including when you redirect the output. Store and handle the token securely. Send it to the Dev Proxy API using the `Authorization: Bearer <token>` request header.

## Exit codes

| Code | Meaning |
| ---- | ------- |
| `0` | Success |
| `1` | The instance or credential is unavailable |
| `2` | Invalid input or usage |

## See also

- [api show](api-show.md)
- [status](status-command.md)
- [Proxy API](proxy-api.md)
