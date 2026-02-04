---
title: MockRequestPlugin
description: MockRequestPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/06/2026
---

<!-- INTENT: Issue web requests from Dev Proxy (e.g., webhooks) -->
<!-- PLUGIN-TYPE: Intercepting -->
<!-- WORKS-WITH: MockResponsePlugin, CrudApiPlugin -->
<!-- USE-WHEN: Simulating webhook notifications or external triggers -->

# MockRequestPlugin

Allows you to issue web requests using Dev Proxy. This plugin is convenient for simulating requests such as webhook notifications.

To issue the configured request, press `w` in the command prompt session where Dev Proxy is running.

:::image type="content" source="../media/mock-request-plugin.png" alt-text="Screenshot of a command prompt split in two. The top part is showing Dev Proxy issuing a web request. The bottom part is showing an API that receives the request and prints the request body." lightbox="../media/mock-request-plugin.png":::

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
  "plugins": [
    {
      "name": "MockRequestPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "contosoNotification"
    }
  ],
  "contosoNotification": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/mockrequestplugin.schema.json",
    "mockFile": "mock-request.json"
  }
}
```

## Configuration properties

| Property | Description |     Default      |
| -------- | ------------| :--------------: |
| `mockFile` | Path to the file containing the mock request | `mock-request.json` |

## Command line options

None

## Mock request file example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/mockrequestplugin.mockfile.schema.json",
  "request": {
    "url": "http://localhost:3000/api/notification",
    "method": "POST",
    "body": {
      "property1": "value1",
      "property2": "value2"
    }
  }
}
```

## Mock request file properties

| Property | Description | Required |
|----------|-------------|----------|
| `request` | Defines the request that Dev Proxy should issue. | Yes |

### Mock request properties

| Property | Description | Required | Default |
|----------|-------------|----------|---------|
| `url` | URL that Dev Proxy should call. | Yes | empty |
| `method` | HTTP method that Dev Proxy should use. | No | `POST` |
| `body` | Body of the request that Dev Proxy should send. | No | empty |
| `headers` | Array of request headers that Dev Proxy should send with the request. | No | empty |

You can configure `body` to a string or a JSON object.

### Mock request headers

| Property | Description | Required |
|----------|-------------|----------|
| `name` | Request header name. | Yes |
| `value` | Request header value. | Yes |
