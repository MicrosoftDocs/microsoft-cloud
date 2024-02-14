---
title: EntraMockResponsePlugin
description: EntraMockResponsePlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/14/2024
---

# EntraMockResponsePlugin

Mocks responses to Microsoft Entra. Includes all functionality of the [MockResponsePlugin](./MockResponsePlugin.md) and adds support for mocking auth flow API requests.

:::image type="content" source="../media/entra-mock-response-plugin.png" alt-text="Screenshot of a terminal with Dev Proxy mocking a response to a Microsoft Entra API." lightbox="../media/entra-mock-response-plugin.png":::

When the plugin simulates auth flow API responses, it updates the state and nonce to match the API request. In the mocked response body, the plugin searches for the following tokens and replaces them with the actual values from the intercepted API requests.

| Token | Description |
| ----- | ----------- |
| `state=@dynamic` | The state token in the request. Dev Proxy replaces the `@dynamic` token with the value of the `state` query string parameter |
| `"id_token": "@dynamic.eyJ0eXAiOiJKV1QiL..."` | Mocked ID token. Dev Proxy removes the `@dynamic.` token and updates the value of the `nonce` claim in the mocked ID token. |

## Plugin instance definition

```json
{
  "name": "EntraMockResponsePlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
  "configSection": "mocksPlugin"
}
```

## Configuration example

See [MockResponsePlugin](./MockResponsePlugin.md)

## Configuration properties

See [MockResponsePlugin](./MockResponsePlugin.md)

## Command line options

See [MockResponsePlugin](./MockResponsePlugin.md)
