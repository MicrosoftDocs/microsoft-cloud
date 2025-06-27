---
title: How to find shadow APIs
description: How to check if all APIs used in my app are registered in Azure API Center
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/05/2025
---

# How to find shadow APIs

Using Azure API Center you can catalog APIs used in your organization. At any time, you can tell which APIs you use in your organization. You can also tell where the API is in its lifecycle and who to contact if there are issues. In short, having an up-to-date catalog of APIs helps you improve the governance-, compliance-, and security posture.

When building your app, especially if you're integrating new scenarios, you might be using APIs that aren't registered in Azure API Center. These APIs are called shadow APIs. Shadow APIs are APIs that aren't registered in your organization. They might be APIs that aren't yet registered, or they might be APIs that aren't meant to be used in your organization.

To check if your app is using shadow APIs, you can use the [`ApiCenterOnboardingPlugin`](../technical-reference/apicenteronboardingplugin.md) plugin. This plugin analyzes API requests from your app and reports on any APIs that aren't registered in API Center. Additionally, it can directly onboard new APIs to Azure API Center.

:::image type="content" source="../media/api-center-onboarding-plugin.png" alt-text="Screenshot of a command prompt showing Dev Proxy checking if the recorded API requests are registered in Azure API Center." lightbox="../media/api-center-onboarding-plugin.png":::

## Before you start

To detect shadow APIs, you need to have an [Azure API Center](/azure/api-center/) instance with information about the APIs that you use in your organization.

### Create an Azure API Center instance

Before you start, create an Azure API Center instance and register APIs that you use in your organization.

:::image type="content" source="../media/api-center-registered-apis.png" alt-text="Screenshot of Azure API Center with several APIs that are used in the organization." lightbox="../media/api-center-registered-apis.png":::

The `ApiCenterOnboardingPlugin` uses this information to check if the APIs, that your app is using, are registered in your organization.

### Copy API Center information

From the Azure API Center instance Overview page, copy the **name** of the API Center instance, the name of the **resource group** and the **subscription ID**. You need this information to configure the `ApiCenterOnboardingPlugin` so that it can connect to your Azure API Center instance.

:::image type="content" source="../media/api-center-overview.png" alt-text="Screenshot of Azure API Center overview page with several properties highlighted." lightbox="../media/api-center-overview.png":::

## Configure Dev Proxy

To check if your app is using shadow APIs, you need to enable the `ApiCenterOnboardingPlugin` in the Dev Proxy configuration file. To create a report of APIs that your app uses, add a reporter.

### Enable the `ApiCenterOnboardingPlugin`

In the `devproxyrc.json` file, add the following configuration:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/rc.schema.json",
  "plugins": [
    {
      "name": "ApiCenterOnboardingPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "apiCenterOnboardingPlugin"
    }
  ],
  "urlsToWatch": [
    "https://jsonplaceholder.typicode.com/*"
  ],
  "apiCenterOnboardingPlugin": {
    "subscriptionId": "aaaa0a0a-bb1b-cc2c-dd3d-eeeeee4e4e4e",
    "resourceGroupName": "demo",
    "serviceName": "contoso-api-center",
    "workspaceName": "default",
    "createApicEntryForNewApis": false
  }
}
```

In the `subscriptionId`, `resourceGroupName`, and `serviceName` properties, provide the information about your Azure API Center instance.

In the `urlsToWatch` property, specify the URLs that your app uses.

> [!TIP]
> Use the [Dev Proxy Toolkit](https://aka.ms/devproxy/toolkit) Visual Studio Code extension to easily manage Dev Proxy configuration.

### Add a reporter

The `ApiCenterOnboardingPlugin` produces a report of APIs that your app is using. To view this report, add a reporter to your Dev Proxy configuration file. Dev Proxy offers several [reporters](../technical-reference/overview.md#reporters). In this example, you use the [plain-text reporter](../technical-reference/plaintextreporter.md).

Update your `devproxyrc.json` file with a reference to the plain-text reporter:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/rc.schema.json",
  "plugins": [
    {
      "name": "ApiCenterOnboardingPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "apiCenterOnboardingPlugin"
    },
    {
      "name": "PlainTextReporter",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ],
  "urlsToWatch": [
    "https://jsonplaceholder.typicode.com/*"
  ],
  "apiCenterOnboardingPlugin": {
    "subscriptionId": "aaaa0a0a-bb1b-cc2c-dd3d-eeeeee4e4e4e",
    "resourceGroupName": "demo",
    "serviceName": "contoso-api-center",
    "workspaceName": "default",
    "createApicEntryForNewApis": false
  }
}
```

## Check if your app is using shadow APIs

To check if your app is using shadow APIs, connect to your Azure subscription, run Dev Proxy, and let it intercept API requests from your app. Dev Proxy then compares the information about the API requests with the information from Azure API Center and reports on any APIs that aren't registered in API Center.

### Connect to your Azure subscription

Dev Proxy uses information from Azure API Center to determine if your app is using shadow APIs. To get this information, it needs a connection to your Azure subscription. You can connect to your Azure subscription in [several ways](../technical-reference/apicenterproductionversionplugin.md#remarks).

### Run Dev Proxy

After connecting to your Azure subscription, start Dev Proxy. If you start Dev Proxy from the same folder where your `devproxyrc.json` file is located, it automatically loads the configuration. Otherwise, specify the path to the configuration file using the `--config-file` option.

When Dev Proxy starts, it checks that it can connect to your Azure subscription. When the connection is successful, you see a message similar to:

```text
 info    Plugin ApiCenterOnboardingPlugin connecting to Azure...
 info    Listening on 127.0.0.1:8000...

Hotkeys: issue (w)eb request, (r)ecord, (s)top recording, (c)lear screen
Press CTRL+C to stop Dev Proxy
```

Press <kbd>r</kbd> to start recording API requests from your app.

### Use your app

Use your app as you would normally do. Dev Proxy intercepts the API requests and stores information about them in memory. In the command line where Dev Proxy runs, you should see information about API requests that your app makes.

```text
 info    Plugin ApiCenterOnboardingPlugin connecting to Azure...
 info    Listening on 127.0.0.1:8000...

Hotkeys: issue (w)eb request, (r)ecord, (s)top recording, (c)lear screen
Press CTRL+C to stop Dev Proxy

◉ Recording... 

 req   ╭ GET https://jsonplaceholder.typicode.com/posts
 api   ╰ Passed through

 req   ╭ DELETE https://jsonplaceholder.typicode.com/posts/1
 api   ╰ Passed through
```

### Check shadow APIs

Stop the recording by pressing <kbd>s</kbd>. Dev Proxy connects to the API Center instance and compares the information about requests with the information from API Center.

```text
 info    Plugin ApiCenterOnboardingPlugin connecting to Azure...
 info    Listening on 127.0.0.1:8000...

Hotkeys: issue (w)eb request, (r)ecord, (s)top recording, (c)lear screen
Press CTRL+C to stop Dev Proxy

◉ Recording... 

 req   ╭ GET https://jsonplaceholder.typicode.com/posts
 api   ╰ Passed through

 req   ╭ DELETE https://jsonplaceholder.typicode.com/posts/1
 api   ╰ Passed through
○ Stopped recording
 info    Checking if recorded API requests belong to APIs in API Center...
 info    Loading APIs from API Center...
 info    Loading API definitions from API Center...
```

When Dev Proxy finishes its analysis, it creates a report in a file named `ApiCenterOnboardingPlugin_PlainTextReporter.txt` with the following contents:

```text
New APIs that aren't registered in Azure API Center:

https://jsonplaceholder.typicode.com:
  DELETE https://jsonplaceholder.typicode.com/posts/1

APIs that are already registered in Azure API Center:

GET https://jsonplaceholder.typicode.com/posts
```

### Automatically onboard shadow APIs

The `ApiCenterOnboardingPlugin` can not only detect shadow APIs, but also automatically onboard them to API Center. To automatically onboard shadow APIs, in the Dev Proxy configuration file, update the `createApicEntryForNewApis` to `true`.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/rc.schema.json",
  "plugins": [
    {
      "name": "ApiCenterOnboardingPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "apiCenterOnboardingPlugin"
    },
    {
      "name": "PlainTextReporter",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ],
  "urlsToWatch": [
    "https://jsonplaceholder.typicode.com/*"
  ],
  "apiCenterOnboardingPlugin": {
    "subscriptionId": "aaaa0a0a-bb1b-cc2c-dd3d-eeeeee4e4e4e",
    "resourceGroupName": "demo",
    "serviceName": "contoso-api-center",
    "workspaceName": "default",
    "createApicEntryForNewApis": true
  }
}
```

When you run Dev Proxy with `createApicEntryForNewApis` set to `true`, it automatically creates new API entries in Azure API Center for the shadow APIs that it detects.

:::image type="content" source="../media/api-center-onboarding-new-api.png" alt-text="Screenshot of API Center showing a newly onboarded API." lightbox="../media/api-center-onboarding-new-api.png":::

### Automatically onboard shadow APIs with OpenAPI spec

When you choose to automatically onboard, shadow APIs to API Center, you can have Dev Proxy generate the OpenAPI spec for the API. Onboarding APIs with OpenAPI specs speeds up onboarding of missing endpoints and provide you with the necessary information about the API. When the `ApiCenterOnboardingPlugin` detects, that Dev Proxy created a new OpenAPI spec, it associates it with the corresponding onboarded API in API Center.

To automatically generate OpenAPI specs for onboarded APIs, update Dev Proxy configuration to include the [`OpenApiSpecGeneratorPlugin`](../technical-reference/openapispecgeneratorplugin.md).

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/rc.schema.json",
  "plugins": [
    {
      "name": "OpenApiSpecGeneratorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    },
    {
      "name": "ApiCenterOnboardingPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "apiCenterOnboardingPlugin"
    },
    {
      "name": "PlainTextReporter",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ],
  "urlsToWatch": [
    "https://jsonplaceholder.typicode.com/*"
  ],
  "apiCenterOnboardingPlugin": {
    "subscriptionId": "aaaa0a0a-bb1b-cc2c-dd3d-eeeeee4e4e4e",
    "resourceGroupName": "demo",
    "serviceName": "contoso-api-center",
    "workspaceName": "default",
    "createApicEntryForNewApis": true
  }
}
```

> [!IMPORTANT]
> Dev Proxy executes plugins in the order they're registered in the configuration. You need to register the `OpenApiSpecGeneratorPlugin` first so that it can create OpenAPI specs before the `ApiCenterOnboardingPlugin` onboards new APIs.

When you run Dev Proxy with this configuration, it automatically creates new API entries in Azure API Center for the shadow APIs that it detects. For each new API, Dev Proxy generates an OpenAPI spec and associates it with the corresponding onboarded API in API Center.

```text
 info    Plugin ApiCenterOnboardingPlugin connecting to Azure...
 info    Listening on 127.0.0.1:8000...

Hotkeys: issue (w)eb request, (r)ecord, (s)top recording, (c)lear screen
Press CTRL+C to stop Dev Proxy

◉ Recording... 

 req   ╭ GET https://jsonplaceholder.typicode.com/posts
 api   ╰ Passed through

 req   ╭ DELETE https://jsonplaceholder.typicode.com/posts/1
 api   ╰ Passed through
○ Stopped recording
 info    Creating OpenAPI spec from recorded requests...
 info    Created OpenAPI spec file jsonplaceholder.typicode.com-20240614104931.json
 info    Checking if recorded API requests belong to APIs in API Center...
 info    Loading APIs from API Center...
 info    Loading API definitions from API Center...
 info    New APIs that aren't registered in Azure API Center:

https://jsonplaceholder.typicode.com:
  DELETE https://jsonplaceholder.typicode.com/posts/1
 info    Creating new API entries in API Center...
 info      Creating API new-jsonplaceholder-typicode-com-1718354977 for https://jsonplaceholder.typicode.com...
 info    DONE
```

:::image type="content" source="../media/api-center-onboarding-new-api-openapi-spec.png" alt-text="Screenshot of Azure API Center showing a newly onboarded API with an OpenAPI spec." lightbox="../media/api-center-onboarding-new-api-openapi-spec.png":::

## Summary

Using the `ApiCenterOnboardingPlugin`, you can check if your app is using shadow APIs. The plugin analyzes API requests from your app and reports on any API requests that aren't registered in Azure API Center. The plugin allows you to easily onboard missing APIs to API Center. By combining the `ApiCenterOnboardingPlugin` plugin with the `OpenApiSpecGeneratorPlugin`, you can automatically generate OpenAPI specs for the newly onboarded APIs. You can run this check manually or integrate with your CI/CD pipeline to ensure that your app is using registered APIs before releasing it to production.

## More information

- [Use Dev Proxy in CI/CD scenarios](./use-dev-proxy-in-ci-cd-overview.md)
- [ApiCenterOnboardingPlugin](../technical-reference/apicenteronboardingplugin.md)
- [Azure API Center](/azure/api-center/)
