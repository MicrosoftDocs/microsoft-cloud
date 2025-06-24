---
title: Configure Dev Proxy
description: Learn how to configure Dev Proxy to your needs.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/05/2025
ms.topic: get-started
---

# Configure Dev Proxy

Dev Proxy is highly configurable. It uses [plugins](../technical-reference/plugin-architecture.md) to implement functionality. You can combine any of the [standard plugins](../technical-reference/overview.md#plugins) and [build your own](../how-to/create-custom-plugin.md). By using plugins and custom configurations, you can tailor Dev Proxy to your specific needs. Dev Proxy includes a default configuration file, named `devproxyrc.json`. The file is located in Dev Proxy's installation folder.

> [!TIP]
> We recommend that you create custom configuration files. By using custom configuration files, you can easily switch between different configurations and can include them in your source control system along with your project's code. Storing your configuration with your project also makes it easier to share it with your team.
>
> If you name your configuration file `devproxyrc.json` or `devproxyrc.jsonc`, Dev Proxy automatically loads it from the current directory when you start it. For other names, specify the file path in the `--config-file` argument when starting Dev Proxy, for example `devproxy --config-file ./my-devproxy-config.json`.

## Configuration file structure

The following code snippet shows the default Dev Proxy configuration file:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.0/rc.schema.json",
  "plugins": [
    {
      "name": "RetryAfterPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    },
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "genericRandomErrorPlugin"
    }
  ],
  "urlsToWatch": [
    "https://jsonplaceholder.typicode.com/*"
  ],
  "genericRandomErrorPlugin": {
    "errorsFile": "devproxy-errors.json"
  },
  "rate": 50,
  "logLevel": "information",
  "newVersionNotification": "stable",
  "showSkipMessages": true,
  "showTimestamps": true
}
```

The configuration file consists of three sections:

- Schema, defined in the `$schema` property. To ensure that your config file is valid, be sure to use the same schema version as the Dev Proxy version you're using.
- The list of plugins that Dev Proxy uses, defined in the `plugins` array.
- General [configuration settings](../technical-reference/proxy-settings.md).

> [!IMPORTANT]
> Dev Proxy applies plugins in the order they're listed in the configuration file. Be mindful of the order to get the desired behavior.

Some plugins are configurable and expose their own settings. Each plugin has its own configuration section in the configuration file, referenced by the `configSection` property in the plugin definition. See the reference documentation for each plugin to learn more about its configuration options.

> [!TIP]
> The value of the `configSection` property can be any string. By using different names, you can include multiple instances of the same plugin, each with a different configuration. You might need to reference the same plugin multiple times, for example, when mocking multiple APIs with different error responses and behaviors.

## Dev Proxy Toolkit

[Dev Proxy Toolkit](https://aka.ms/devproxy/toolkit) is a Visual Studio Code extension that significantly simplifies configuring Dev Proxy. Here are some of the features it includes:

- code snippets for common configuration scenarios
- extended linting and IntelliSense for Dev Proxy configuration files
- custom editor actions and commands to conveniently start and stop Dev Proxy
- notifications about new versions of Dev Proxy

> [!TIP]
> If you use Visual Studio Code, we highly recommend that you [install the Dev Proxy Toolkit](vscode:extension/garrytrinder.dev-proxy-toolkit) extension.

## Next step

If you want to keep learning about using Dev Proxy, consider the following tutorials:

- [Simulate random errors for your own application](../tutorials/simulate-errors-for-your-own-app.md)
- [Test a JavaScript client-side web application that calls Microsoft Graph](../tutorials/test-javascript-client-side-web-app-microsoft-graph.md)
- [Test a JavaScript client-side web application](../tutorials/test-javascript-client-side-web-app.md)

Otherwise, check out our [how to guides](../how-to/overview.md) to learn how to use Dev Proxy for specific scenarios.
