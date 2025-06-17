---
title: jwt create
description: jwt create command reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 06/02/2025
---

# jwt create

Creates a new JSON Web Token (JWT) for testing purposes.

## Usage

```console
devproxy jwt create [options]
```

## Example

Generates a JWT for a user named `Megan Bowen` with the issuer `my-app` and audience `https://myserver.com`. The token includes the role `admin`, scopes `read` and `write`, and a custom claim `custom:claim`. The token is valid for 120 minutes.

```console
devproxy jwt create --name "Megan Bowen" --issuer "my-app" --audiences "https://myserver.com" --roles "admin" --scopes "read" --scopes "write" --claims "custom:claim" --valid-for 120
```

## Arguments

None

## Options

|Name|Description|Allowed values|Default|
|--|--|--|--|
| `-n, --name` | The name of the user to create the token for. | string | `Dev Proxy` |
| `-i, --issuer` | The issuer of the token. | string | `dev-proxy` |
| `-a, --audiences` | The audiences to create the token for. Specify once for each audience. | string | `https://myserver.com` |
| `-r, --roles` | A role claim to add to the token. Specify once for each role. | string | None |
| `-s, --scopes` | A scope claim to add to the token. Specify once for each scope. | string | None |
| `--claims` | Claims to add to the token. Specify once for each claim in the format `name:value`. | None |
| `-v, --valid-for` | The duration which the token is valid for. Duration is set in minutes. | `60` |
| `--signing-key` | The key to use to sign the token. Must be at least 32 characters long. | Randomly generated |
|`--log-level <loglevel>`|Level of messages to log|`trace`, `debug`, `information`, `warning`, `error`| `information`|

> [!NOTE]
> Dev Proxy automatically adds registered claims (for example, `iss`, `sub`, `aud`, `exp`, `nbf`, `iat`, `jti`) to the token. If you specify any of these claims using the `--claims` option, it takes precedence over the value specified in the dedicated option.
