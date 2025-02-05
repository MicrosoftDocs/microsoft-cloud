---
title: GraphConnectorNotificationPlugin
description: GraphConnectorNotificationPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/05/2025
---

# GraphConnectorNotificationPlugin

Simulates the notification when enabling or disabling a Microsoft Graph connector in Teams Admin Center (TAC). Validates requests for creating and deleting the external connection.

To issue the configured notification request, press `w` in the command prompt session where Dev Proxy is running.

:::image type="content" source="../media/graph-connector-notification-plugin.png" alt-text="Screenshot of a command prompt split in two. The top part is showing Dev Proxy issuing a simulated Teams Admin Center notification. The bottom part is showing a Microsoft Graph connector that receives the notification." lightbox="../media/graph-connector-notification-plugin.png":::

## Issuing notification requests

The `GraphConnectorNotificationPlugin` extends the `MockRequestPlugins` with extra functionality to simulate notifications from TAC.

| Token | Description |
| ----- | ----------- |
| `@dynamic.validationToken` | JWT token to validate the authenticity of the notification. Dev Proxy replaces it with a valid JWT token, signed by the `Dev Proxy CA` certificate. |

## Validating handling notifications and issuing Graph connector requests

Next to simulating the notification requests, the `GraphConnectorNotificationPlugin` validates if the notification API, properly handles notifications and issues correct Microsoft Graph requests.

For handling TAC notifications, the plugin checks if the API sends a 202 Accepted response without a body. Additionally, the plugin inspects POST and DELETE requests to the `/external/connections/*` Microsoft Graph endpoint. It checks if the request contains the `GraphConnectors-Ticket` header with the ticket specified in the notification. If either of the checks fails, the plugin logs an error.

## Plugin instance definition

```json
{
  "name": "GraphConnectorNotificationPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
  "configSection": "graphConnectorNotificationEnabled"
}
```

## Configuration example

```json
{
  "graphConnectorNotificationEnabled": {
    "mockFile": "graph-connector-notification-enabled.json",
    "tenant": "fa15d692-e9c7-4460-a743-29f29522229",
    "audience": "00001111-aaaa-2222-bbbb-3333cccc4444"
  }
}
```

## Configuration properties

| Property | Description |     Default      | Required |
| -------- | ------------| :--------------: | :------: |
| `audience` | The Microsoft Entra app registration ID that the Microsoft Graph connector uses to authenticate the notification request | empty | Yes |
| `mockFile` | Path to the file containing the mock request | `mock-request.json` | Yes |
| `tenant` | The tenant ID where the Microsoft Graph connector creates the external connection | empty | Yes |

## Command line options

None

## Mock request file example

Following are several examples of API files that define a CRUD API for information about customers.

### Enable a Microsoft Graph connector TAC notification

Following is an example of a notification that Teams Admin Center sends, when a user enables a Microsoft Graph connector.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.24.0/mockrequestplugin.schema.json",
  "request": {
    "url": "http://localhost:3000/api/notification",
    "method": "POST",
    "body": {
      "value": [
        {
          "changeType": "updated",
          "subscriptionId": "aaaa0a0a-bb1b-cc2c-dd3d-eeeeee4e4e4e",
          "resource": "external",
          "clientState": null,
          "resourceData": {
            "@odata.type": "#Microsoft.Graph.connector",
            "@odata.id": "external",
            "id": "35177924-33fc-444d-bd51-f059ce385ec2",
            "state": "enabled",
            "connectorsTicket":"eyJhbGciOiJIUzI1"
          },
          "subscriptionExpirationDateTime": "2021-06-26T12:40:26.4436785-07:00",
          "tenantId": "fa15d692-e9c7-4460-a743-29f29522229"
        }
      ],
      "validationTokens": [ "@dynamic.validationToken" ]
    }
  }
}
```
