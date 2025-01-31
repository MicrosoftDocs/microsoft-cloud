---
title: Configure Dev Proxy
description: Learn how to configure Dev Proxy to your needs.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/31/2025
ms.topic: get-started
---

# Configure Dev Proxy

Dev Proxy is highly configurable. It uses [plugins](./technical-reference/plugin-architecture.md) to implement functionality. You can combine any of the [standard plugins](./technical-reference/overview.md#plugins) and [build your own](./how-to/create-custom-plugin.md). By using plugins and custom configurations, you can tailor Dev Proxy to your specific needs. Dev Proxy includes a default configuration file, named `devproxyrc.json`. The file is located in Dev Proxy's installation folder.

> [!TIP]
> We recommend that you create custom configuration files. By using custom configuration files, you can easily switch between different configurations and can include them in your source control system along with your project's code. Storing your configuration with your project also makes it easier to share it with your team.
>
> If you name your configuration file `devproxyrc.json` or `devproxyrc.jsonc`, Dev Proxy automatically loads it from the current directory when you start it. For other names, specify the file path in the `--config-file` argument when starting Dev Proxy.

## Configuration file structure

The following code snippet shows the default Dev Proxy configuration file:

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v0.24.0/rc.schema.json",
  "plugins": [
    {
      "name": "RetryAfterPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll"
    },
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
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

The configuration file consists of two sections:

1. The list of plugins that Dev Proxy uses, defined in the `plugins` array.
1. General [configuration settings](./technical-reference/proxy-settings.md).

> [!IMPORTANT]
> Dev Proxy applies plugins in the order they're listed in the configuration file. Be mindful of the order to get the desired behavior.

Some plugins are configurable and expose their own settings. Each plugin has its own configuration section in the configuration file, referenced by the `configSection` property in the plugin definition. See the reference documentation for each plugin to learn more about its configuration options.

> [!TIP]
> The value of the `configSection` property can be any string. By using different names, you can include multiple instances of the same plugin, each with a different configuration. You might need to reference the same plugin multiple times, for example, when mocking multiple APIs with different error responses and behaviors.

## Dev Proxy Toolkit

[Dev Proxy Toolkit](https://aka.ms/devproxy/toolkit) is a Visual Studio Code extension that significantly simplifies configuring Dev Proxy. Here are some of the features it includes:

- code snippets for common configuration scenarios
- custom editor actions and commands to conveniently start and stop Dev Proxy
- notifications about new versions of Dev Proxy

If you use Visual Studio Code, we highly recommend that you install the Dev Proxy Toolkit extension.

## Example scenarios

The following scenarios show how to configure Dev Proxy for different scenarios.

### Update the URLs to watch

By default, Dev Proxy is configured to intercept any request made to the [JSON Placeholder API](https://jsonplaceholder.typicode.com/). You can configure Dev Proxy to intercept requests to any HTTP API.

- Open the Dev Proxy configuration file by running in command line: `devproxy config`.
- Locate the `urlsToWatch` array.

```json
"urlsToWatch": [
  "https://jsonplaceholder.typicode.com/*"
],
```

The `urlsToWatch` array represents the URLs that Dev Proxy should intercept. When Dev Proxy intercepts a request to a URL that matches a URL from the array, it applies to it plugins as defined in the configuration file. An asterisk (`*`) in the URL indicates a wildcard. If you want to exclude a URL, prepend it with an exclamation mark (`!`).

Let's consider that you don't want Dev Proxy to intercept requests made to a specific endpoint.

- Add a new entry to the `urlsToWatch` array.

```json
"urlsToWatch": [
  "!https://jsonplaceholder.typicode.com/posts/2",
  "https://jsonplaceholder.typicode.com/*"
],
```

The exclamation mark at the beginning of the URL tells Dev Proxy to ignore any requests that match that URL. You can mix and match exclamation marks and asterisks in a URL.

- At the command line, enter `devproxy` and press <kbd>Enter</kbd> to start Dev Proxy.
- Send a request to `https://jsonplaceholder.typicode.com/posts/2` from the command line and view the output.

When an ignored URL is matched to a request, Dev Proxy doesn't process the request, and so no output is displayed.

> [!TIP]
> The order in which the URLs are listed in the `urlsToWatch` array is important. Dev Proxy processes these URLs in order. When a URL matches, it isn't processed again. Therefore, placing the URL first ensures that the request is ignored before the next URL is processed.

### Change failure rate

By default, Dev Proxy is configured to fail requests with a 50% chance to URLs that are being watched. You can increase or decrease the chance of a request returning an error response.

Let's update the failure rate so that every request to the JSON Placeholder API returns an error response.

- Open the Dev Proxy configuration file by running in command line: `devproxy config`.
- Locate the `rate` property and update the value from `50` to `100`.

The `devproxyrc.json` file contains configuration settings that are used when you start Dev Proxy. When changing configuration settings, you should always stop and start Dev Proxy for the changes to be persisted.

- At the command line, enter `devproxy` and press <kbd>Enter</kbd> to start Dev Proxy.
- Send a request to the JSON Placeholder API from the command line and view the output.

Alternatively, you can override configuration settings at runtime by using the `--failure-rate` option when starting Dev Proxy.

```text
devproxy --failure-rate 100
```

- Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to safely stop Dev Proxy.

### Simulate throttling

By default, Dev Proxy returns a range of generic 400 and 500 error responses. You can customize these error responses to your own needs.

By default, Dev Proxy uses the following plugins:

- [GenericRandomErrorPlugin](./technical-reference/genericrandomerrorplugin.md) plugin provides the ability for Dev Proxy to respond with an error response.
- [RetryAfterPlugin](./technical-reference/retryafterplugin.md) plugin provides the ability for Dev Proxy to inject a dynamic value into the Retry-After header in the error response.

Let's change the configuration so that Dev Proxy always returns a `429 Too Many requests` error response to simulate throttling.

First let's locate the location of the file that contains the error definitions.

- Open the Dev Proxy configuration file by running in command line: `devproxy config`.
- In the `plugins` array, locate the entry for the [GenericRandomErrorPlugin](./technical-reference/genericrandomerrorplugin.md) plugin. Note the value of the `configSection` property.
- Further down the file, locate the `genericRandomErrorPlugin` object. Note the value of the `errorsFile` property.

> [!TIP]
> The location of the errors file is also displayed in the output when you start Dev Proxy.

- In the Dev Proxy installation folder, open `devproxy-errors.json` in a text editor.
- Remove all response entries in the `responses` array, except for the `429` response.

```json
{
  "$schema": "https://raw.githubusercontent.com/microsoft/dev-proxy/main/schemas/v0.24.0/genericrandomerrorplugin.schema.json",
  "errors": [
    {
      "request": {
        "url": "https://jsonplaceholder.typicode.com/*"
      },
      "responses": [
        {
          "statusCode": 429,
          "body": {
            "message": "Too Many Requests",
            "details": "The user has sent too many requests in a given amount of time (\"rate limiting\")."
          },
          "headers": {
            "Retry-After": "@dynamic"
          }
        }
      ]
    }
  ]
}
```

- At the command line, enter `devproxy` and press <kbd>Enter</kbd> to start Dev Proxy.
- Send a request to the JSON Placeholder API from the command line and view the output.

```text
 req   ╭ GET https://jsonplaceholder.typicode.com/posts
 skip  │ RetryAfterPlugin: Request not throttled
 oops  ╰ 429 TooManyRequests
```

- Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to safely stop Dev Proxy.

## Next step

Learn how to use Dev Proxy to simulate random errors for your own application.

> [!div class="nextstepaction"]
> [Simulate errors for your own app](./tutorials/simulate-errors-for-your-own-app.md)
