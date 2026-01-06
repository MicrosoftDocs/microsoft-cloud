---
title: EntraMockResponsePlugin
description: EntraMockResponsePlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/06/2026
---

<!-- INTENT: Mock Microsoft Entra auth flow responses -->
<!-- PLUGIN-TYPE: Intercepting -->
<!-- WORKS-WITH: MockResponsePlugin, CrudApiPlugin -->
<!-- USE-WHEN: Testing auth flows without real Microsoft Entra -->

# EntraMockResponsePlugin

Mocks responses to Microsoft Entra. Includes all functionality of the [MockResponsePlugin](./MockResponsePlugin.md) and adds support for mocking auth flow API requests.

:::image type="content" source="../media/entra-mock-response-plugin.png" alt-text="Screenshot of a command prompt with Dev Proxy mocking a response to a Microsoft Entra API." lightbox="../media/entra-mock-response-plugin.png":::

When the plugin simulates auth flow API responses, it updates the state and nonce to match the API request. In the mocked response body, the plugin searches for the following tokens and replaces them with the actual values from the intercepted API requests.

| Token | Description |
| ----- | ----------- |
| `@dynamic.devProxyCertificateChain` | The certificate chain of the Dev Proxy CA. Dev Proxy replaces the `@dynamic.devProxyCertificateChain` token with the actual certificate chain of the Dev Proxy CA. This token is useful if you want to validate an access token issued by Dev Proxy. |
| `@dynamic.devProxyKeyId` | The key ID of the Dev Proxy CA. Dev Proxy replaces the `@dynamic.devProxyKeyId` token with the actual key ID of the Dev Proxy CA. This token is useful if you want to validate an access token issued by Dev Proxy. |
| `state=@dynamic` | The state token in the request. Dev Proxy replaces the `@dynamic` token with the value of the `state` query string parameter |
| `"id_token": "@dynamic.eyJ0eXAiOiJKV1QiL..."` | Mocked ID token. Dev Proxy removes the `@dynamic.` token and updates the value of the `nonce` claim in the mocked ID token. |

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "EntraMockResponsePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "mocksPlugin"
    }
  ],
  "mocksPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/mockresponseplugin.schema.json",
    "mocksFile": "mocks.json"
  }
}
```

See [MockResponsePlugin](./MockResponsePlugin.md) for more configuration options.

## Configuration properties

See [MockResponsePlugin](./MockResponsePlugin.md)

## Command line options

See [MockResponsePlugin](./MockResponsePlugin.md)
