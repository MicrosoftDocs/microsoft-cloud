---
title: ApiCenterProductionVersionPlugin
description: ApiCenterProductionVersionPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 04/30/2025
---

# ApiCenterProductionVersionPlugin

Checks if the APIs used in an app are production version of the APIs registered in the specified Azure API Center instance.

:::image type="content" source="../media/api-center-production-version-plugin.png" alt-text="Screenshot of a command prompt showing Dev Proxy checking if the recorded API requests match production version APIs registered in Azure API Center." lightbox="../media/api-center-production-version-plugin.png":::

## Plugin instance definition

```json
{
  "name": "ApiCenterProductionVersionPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
  "configSection": "apiCenterProductionVersionPlugin"
}
```

## Configuration example

```json
{
  "apiCenterProductionVersionPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.27.0/apicenterproductionversionplugin.schema.json",
    "subscriptionId": "aaaa0a0a-bb1b-cc2c-dd3d-eeeeee4e4e4e",
    "resourceGroupName": "resource-group-name",
    "serviceName": "apic-instance",
    "workspaceName": "default"
  }
}
```

## Configuration properties

| Property | Description | Default |
|----------|-------------|:-------:|
| `resourceGroupName` | Name of the resource group where the Azure API Center is located. | None |
| `serviceName` | Name of the Azure API Center instance that Dev Proxy should use to check if the APIs used in the app are registered. | None |
| `subscriptionId` | ID of the Azure subscription where the Azure API Center instance is located. | None |
| `workspace` | Name of the Azure API Center workspace to use. | `default` |

## Command line options

None

## Remarks

The `ApiCenterProductionVersionPlugin` plugin checks if the APIs used in an app are production version of the APIs registered in the specified Azure API Center instance. If the APIs match nonproduction versions, the plugin shows a warning.

To connect to Azure API Center, the plugin uses Azure credentials (in this order):

- Environment
- Workload Identity
- Managed Identity
- Visual Studio
- Visual Studio Code
- Azure CLI
- Azure PowerShell
- Azure Developer CLI

If the plugin fails to get an access token to access Azure, it shows an error and Dev Proxy disables it. Sign in to Azure using either of these tools, and restart Dev Proxy to use the `ApiCenterProductionVersionPlugin` plugin.

If you use Dev Proxy in CI/CD pipelines, you can pass values for the `subscriptionId`, `resourceGroupName`, `serviceName`, and `workspaceName` properties as environment variables. To use environment variables, prepend the name of the value with a `@`, for example:

```json
{
  "apiCenterProductionVersionPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.27.0/apicenterproductionversionplugin.schema.json",
    "subscriptionId": "@AZURE_SUBSCRIPTION_ID",
    "resourceGroupName": "@AZURE_RESOURCE_GROUP_NAME",
    "serviceName": "@AZURE_APIC_INSTANCE_NAME",
    "workspaceName": "@AZURE_APIC_WORKSPACE_NAME"
  }
}
```

In this example, the `ApiCenterProductionVersionPlugin` plugin sets `subscriptionId`, `resourceGroupName`, `serviceName`, and `workspaceName` properties to the values of the `AZURE_SUBSCRIPTION_ID`, `AZURE_RESOURCE_GROUP_NAME`, `AZURE_APIC_INSTANCE_NAME`, and `AZURE_APIC_WORKSPACE_NAME` environment variables, respectively.

## Next step

> [!div class="nextstepaction"]
> [How to check if my app is using production-level APIs](../how-to/check-production-level-apis.md)
