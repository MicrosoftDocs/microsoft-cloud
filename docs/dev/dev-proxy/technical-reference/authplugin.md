---
title: AuthPlugin
description: AuthPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 04/30/2025
---

# AuthPlugin

Simulates authentication and authorization using API keys or OAuth2.

:::image type="content" source="../media/auth-plugin.png" alt-text="Screenshot of a command prompt with Dev Proxy simulating authentication using API key on an Azure Function running locally." lightbox="../media/auth-plugin.png":::

## Plugin instance definition

```json
{
  "name": "AuthPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
  "configSection": "auth"
}
```

## Configuration example: API key

```json
{
  "auth": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.0/authplugin.schema.json",
    "type": "apiKey",
    "apiKey": {
      "parameters": [
        {
          "in": "header",
          "name": "x-api-key"
        },
        {
          "in": "query",
          "name": "code"
        }
      ],
      "allowedKeys": [
        "1234"
      ]
    }
  }
}
```

## Configuration example: OAuth2

```json
{
  "auth": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.0/authplugin.schema.json",
    "type": "oauth2",
    "oauth2": {
      "metadataUrl": "https://login.microsoftonline.com/organizations/v2.0/.well-known/openid-configuration",
      "allowedApplications": [
        "00000000-0000-0000-0000-000000000000"
      ],
      "allowedAudiences": [
        "00000000-0000-0000-0000-000000000000"
      ],
      "allowedPrincipals": [
        "00000000-0000-0000-0000-000000000000"
      ],
      "allowedTenants":[
        "00000000-0000-0000-0000-000000000000"
      ],
      "issuer": "https://login.microsoftonline.com/ffffffff-eeee-dddd-cccc-bbbbbbbbbbb0/v2.0",
      "scopes": [
        "Posts.Read"
      ],
      "validateLifetime": true,
      "validateSigningKey": true
    }
  }
}
```

## Configuration properties

| Property | Description | Required |
|----------|-------------|:--------:|
| `type` | Type of authentication and authorization that Dev Proxy should use. Allowed values: `apiKey`, `oauth2` | Yes |
| `apiKey` | Configuration for API key authentication and authorization. | Yes, when `type` is `apiKey` |
| `oauth2` | Configuration for OAuth2 authentication and authorization. | Yes, when `type` is `oauth2` |

### API key configuration properties

| Property | Description | Required |
|----------|-------------|:--------:|
| `allowedKeys` | List of allowed API keys. | Yes |
| `parameters` | List of parameters that contain the API key. | Yes |

#### Parameter configuration properties

| Property | Description | Required |
|----------|-------------|:--------:|
| `in` | Where the parameter is expected to be found. Allowed values: `header`, `query`, `cookie` | Yes |
| `name` | Name of the parameter. | Yes |

### OAuth2 configuration properties

| Property | Description | Required |
|----------|-------------|:--------:|
| `metadataUrl` | URL to the OpenID Connect metadata document. | Yes |
| `allowedApplications` | List of allowed application IDs. Leave empty to not validate the application (`appid` or `azp` claim) for which the token is issued. | No |
| `allowedAudiences` | List of allowed audiences. Leave empty to not validate the audience (`aud` claim) for which the token is issued. | No |
| `allowedPrincipals` | List of allowed principals. Leave empty to not validate the principal (`oid` claim) for which the token is issued. | No |
| `allowedTenants` | List of allowed tenants. Leave empty to not validate the tenant (`tid` claim) for which the token is issued. | No |
| `issuer` | Allowed token issuer. Leave empty to not validate the token issuer. | No |
| `roles` | List of allowed roles. Leave empty to not validate the roles (`roles` claim) on the token. | No |
| `scopes` | List of allowed scopes. Leave empty to not validate the scopes (`scp` claim) on the token. | No |
| `validateLifetime` | Set to `false` to disable validating the token lifetime. Default `true`. | No |
| `validateSigningKey` | Set to `false` to disable validating the token signature. Default `true` | No |

## Command line options

None
