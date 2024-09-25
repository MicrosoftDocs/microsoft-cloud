---
title: jwt create
description: jwt create command reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 09/24/2024
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
devproxy jwt create --name "Megan Bowen" --issuer "my-app" --audience "https://myserver.com" --roles "admin" --scopes "read" --scopes "write" --claims "custom:claim" --valid-to 120
```

## Arguments

None

## Options

| Name | Description | Default |
| --- | --- | --- |
| `-n, --name` | The name of the user to create the token for. | Dev Proxy |
| `-i, --issuer` | The issuer of the token. | `dev-proxy` |
| `-a, --audience` | The audiences to create the token for. Specify once for each audience. | `https://myserver.com` |
| `-r, --roles` | A role claim to add to the token. Specify once for each role. | None |
| `-s, --scopes` | A scope claim to add to the token. Specify once for each scope. | None |
| `--claims` | Claims to add to the token. Specify once for each claim in the format `name:value`. | None |
| `-v, --valid-for` | The duration which the token is valid for. Duration is set in minutes. | 60 |

> [!NOTE]
> Registered claims (e.g. `iss`, `sub`, `aud`, `exp`, `nbf`, `iat`, `jti`) are automatically added to the token. If you specify any of these claims in the `--claims` option, the values you provide will be ignored.
