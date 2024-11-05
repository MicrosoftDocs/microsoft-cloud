---
title: Mock Teams Admin Center notification for Microsoft Graph connectors
description: How to test that your Microsoft Graph connector properly handles notifications from the Teams Admin Center.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 04/08/2024
---

# Mock Teams Admin Center notification for Microsoft Graph connectors

Microsoft Graph connectors allow you to bring your organizational content to Microsoft 365. Using Microsoft Graph connectors you're able to find your content from one place, no matter where you store it. What's more, it gives Microsoft Copilot for Microsoft 365 access to the content, so that it can help you get more relevant answers.

When deploying Graph connectors in your organization, you should consider packaging them as Microsoft Teams app. That way, they're deployed to the Teams Admin Center, from which admins can control them in a familiar way. To package a Graph connector as a Teams app, you need to extend it with an API that receives the webhook from Teams Admin Center.

Dev Proxy allows you to test how your Microsoft Graph connector handles notifications from the Teams Admin Center. You can mock the notification for enabling and disabling the Graph connector and check if your connector processes it correctly. Using Dev Proxy, you can validate configuring your connector end to end: from validating the token, to running the initial content ingestion. Dev Proxy allows you to test your connector locally without deploying it to the Teams Admin Center.

## Before you begin

Before you start mocking Teams Admin Center notifications, complete the following steps.

### Download the Teams Admin Center notifications for Microsoft Graph connectors Dev Proxy preset

Start, by downloading the Dev Proxy preset for simulating Teams Admin Center notifications for Microsoft Graph connectors. In the command prompt, run the following command:

```console
devproxy preset get microsoft-graph-connector-notification
```

Dev Proxy downloads the preset and saves it in the **presets** folder in your Dev Proxy installation directory.

### Configure the preset to send the notification to your API

In a code editor, open the `~appFolder/presets/microsoft-graph-connector-notification/graph-connector-notification-enabled.json` file, where `~appFolder` refers to the Dev Proxy installation folder. Update the `request.url` property with the URL of your API that receives the notification from the Teams Admin Center.

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v0.16.0/mockrequestplugin.schema.json",
  "request": {
    "url": "http://localhost:3000/api/notification",
    "method": "POST",
    // [...] trimmed for brevity
  }
}
```

Follow the same steps for the `graph-connector-notification-disabled.json` file.

### Configure tenant ID and Entra app for your Microsoft Graph connector

Dev Proxy simulates validating the token from the Teams Admin Center notification and issuing an access token for Microsoft Graph for your connector. Dev Proxy uses a simulated Microsoft 365 tenant ID and an Entra app. To intercept requests from your Graph connector, update the tenant ID to `fa15d692-e9c7-4460-a743-29f29522229` and Entra app ID to `00001111-aaaa-2222-bbbb-3333cccc4444`. If you want to use your own IDs, update the values in all preset files.

## Mock the Teams Admin Center notification for enabling the Microsoft Graph connector

Start your API that receives the notification from the Teams Admin Center. Ensure, that it proxies its requests through Dev Proxy.

Next, in a command prompt, start Dev Proxy with the preset for simulating the Teams Admin Center notification for enabling the Microsoft Graph connector.

```console
devproxy --config-file "~appFolder/presets/microsoft-graph-connector-notification/devproxyrc.json"
```

After Dev Proxy starts, press <kbd>w</kbd> to simulate the webhook from the Teams Admin Center for enabling the Microsoft Graph connector. Dev Proxy sends the notification to your API, which should process it as if it came from the Teams Admin Center.

:::image type="content" source="../media/graph-connector-notification-plugin.png" alt-text="Screenshot of a command prompt split in two. Top: Dev Proxy issuing a simulated notification. Bottom: a Microsoft Graph connector that receives it." lightbox="../media/graph-connector-notification-plugin.png":::

When ready, to stop Dev Proxy, press <kbd>Ctrl</kbd>+<kbd>c</kbd>.

## Mock the Teams Admin Center notification for disabling the Microsoft Graph connector

In a code editor, open the `~appFolder/presets/microsoft-graph-connector-notification/devproxyrc.json` file, where `~appFolder` refers to the Dev Proxy installation folder. Locate the instance of the `GraphConnectorNotificationPlugin` for the enabled notification, and change the `enabled` property to `false`. Locate the instance of the `GraphConnectorNotificationPlugin` for the disabled notification, and change the `enabled` property to `true`. The config file should be similar to:

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v0.16.0/rc.schema.json",
  "plugins": [
    {
      "name": "GraphConnectorGuidancePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll"
    },
    {
      "name": "GraphConnectorNotificationPlugin",
      "enabled": false,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
      "configSection": "graphConnectorNotificationEnabled"
    },
    {
      "name": "GraphConnectorNotificationPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
      "configSection": "graphConnectorNotificationDisabled"
    },
    // [...] trimmed for brevity
  ]
  // [...] trimmed for brevity
}
```

Save your changes.

Follow the same steps as described previously when testing the notification for enabling the Microsoft Graph connector.

When ready, to stop Dev Proxy, press <kbd>Ctrl</kbd>+<kbd>c</kbd>.

## Next step

Learn more about the GraphConnectorNotificationPlugin.

> [!div class="nextstepaction"]
> [GraphConnectorNotificationPlugin](../technical-reference/graphconnectornotificationplugin.md)

## More information

- [Microsoft Graph connectors overview](/graph/connecting-external-content-connectors-overview)
- [Enable the simplified admin experience for your Microsoft Graph connector in the Teams admin center](/graph/connecting-external-content-deploy-teams)
