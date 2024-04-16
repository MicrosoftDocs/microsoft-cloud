---
title: ApiCenterOnboardingPlugin
description: ApiCenterOnboardingPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 04/16/2024
---

# ApiCenterOnboardingPlugin

Checks if the APIs used in an app are registered in the specified Azure API Center instance.

:::image type="content" source="../media/api-center-onboarding-plugin.png" alt-text="Screenshot of a command prompt showing Dev Proxy checking if the recorded API requests are registered in Azure API Center." lightbox="../media/api-center-onboarding-plugin.png":::

## Plugin instance definition

```json
{
  "name": "ApiCenterOnboardingPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
  "configSection": "apiCenterOnboardingPlugin"
}
```

## Configuration example

```json
{
  "apiCenterOnboardingPlugin": {
    "subscriptionId": "cdae2297-7aa6-4195-bbb1-dcd89153cc72",
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
| `subscriptionId` | ID of the Azure subscription where the Azure API Center instance is located. | None |
| `resourceGroupName` | Name of the resource group where the Azure API Center is located. | None |
| `serviceName` | Name of the Azure API Center instance that Dev Proxy should use to check if the APIs used in the app are registered. | None |
| `workspace` | Name of the Azure API Center workspace to use. | `default` |
| `createApicEntryForNewApis` | Set to `true` to have Dev Proxy create new API entries for the APIs that it detected and which aren't yet registered in API Center. When set to `false` Dev Proxy only lists the unregistered APIs in the command prompt. | `true` |

## Command line options

None

## Remarks

The `ApiCenterOnboardingPlugin` plugin checks if the APIs used in an app are registered in the specified Azure API Center instance. If the APIs aren't registered, the plugin can create new API entries in the API Center instance.

To connect to Azure API Center, the plugin uses the following credentials (in this order):

- Visual Studio
- Visual Studio Code
- Azure CLI
- Azure PowerShell
- Azure Developer CLI

If you aren't signed in to Azure with any of these tools, the plugin shows an error and Dev Proxy disables it. Sign in to Azure using either of these tools, and restart Dev Proxy to use the `ApiCenterOnboardingPlugin` plugin.
