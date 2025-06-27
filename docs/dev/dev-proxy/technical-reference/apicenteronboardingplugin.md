---
title: ApiCenterOnboardingPlugin
description: ApiCenterOnboardingPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 04/30/2025
---

# ApiCenterOnboardingPlugin

Checks if the APIs used in an app are registered in the specified Azure API Center instance.

:::image type="content" source="../media/api-center-onboarding-plugin.png" alt-text="Screenshot of a command prompt showing Dev Proxy checking if the recorded API requests are registered in Azure API Center." lightbox="../media/api-center-onboarding-plugin.png":::

## Plugin instance definition

```json
{
  "name": "ApiCenterOnboardingPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
  "configSection": "apiCenterOnboardingPlugin"
}
```

## Configuration example

```json
{
  "apiCenterOnboardingPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/apicenteronboardingplugin.schema.json",
    "subscriptionId": "aaaa0a0a-bb1b-cc2c-dd3d-eeeeee4e4e4e",
    "resourceGroupName": "resource-group-name",
    "serviceName": "apic-instance",
    "workspaceName": "default",
    "createApicEntryForNewApis": true
  }
}
```

## Configuration properties

| Property | Description | Default |
|----------|-------------|:-------:|
| `createApicEntryForNewApis` | Set to `true` to have Dev Proxy create new API entries for the APIs that it detected and which aren't yet registered in API Center. When set to `false` Dev Proxy only lists the unregistered APIs in the command prompt. | `true` |
| `resourceGroupName` | Name of the resource group where the Azure API Center is located. | None |
| `serviceName` | Name of the Azure API Center instance that Dev Proxy should use to check if the APIs used in the app are registered. | None |
| `subscriptionId` | ID of the Azure subscription where the Azure API Center instance is located. | None |
| `workspace` | Name of the Azure API Center workspace to use. | `default` |

## Command line options

None

## Remarks

The `ApiCenterOnboardingPlugin` plugin checks if the APIs used in an app are registered in the specified Azure API Center instance. If the APIs aren't registered, the plugin can create new API entries in the API Center instance.

To connect to Azure API Center, the plugin uses Azure credentials (in this order):

- Environment
- Workload Identity
- Managed Identity
- Visual Studio
- Visual Studio Code
- Azure CLI
- Azure PowerShell
- Azure Developer CLI

If the plugin fails to get an access token to access Azure, it shows an error and Dev Proxy disables it. Sign in to Azure using either of these tools, and restart Dev Proxy to use the `ApiCenterOnboardingPlugin` plugin.

If you use Dev Proxy in CI/CD pipelines, you can pass values for the `subscriptionId`, `resourceGroupName`, `serviceName`, and `workspaceName` properties as environment variables. To use environment variables, prepend the name of the value with a `@`, for example:

```json
{
  "apiCenterOnboardingPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/apicenteronboardingplugin.schema.json",
    "subscriptionId": "@AZURE_SUBSCRIPTION_ID",
    "resourceGroupName": "@AZURE_RESOURCE_GROUP_NAME",
    "serviceName": "@AZURE_APIC_INSTANCE_NAME",
    "workspaceName": "@AZURE_APIC_WORKSPACE_NAME",
    "createApicEntryForNewApis": true
  }
}
```

In this example, the `ApiCenterOnboardingPlugin` plugin sets `subscriptionId`, `resourceGroupName`, `serviceName`, and `workspaceName` properties to the values of the `AZURE_SUBSCRIPTION_ID`, `AZURE_RESOURCE_GROUP_NAME`, `AZURE_APIC_INSTANCE_NAME`, and `AZURE_APIC_WORKSPACE_NAME` environment variables, respectively.

## Next step

> [!div class="nextstepaction"]
> [How to find shadow APIs](../how-to/check-shadow-apis.md)
