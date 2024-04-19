---
title: ApiCenterOnboardingPlugin
description: ApiCenterOnboardingPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 04/19/2024
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
| `createApicEntryForNewApis` | Set to `true` to have Dev Proxy create new API entries for the APIs that it detected and which aren't yet registered in API Center. When set to `false` Dev Proxy only lists the unregistered APIs in the command prompt. | `true` |
| `excludeDevCredentials` | Set to `true` for Dev Proxy not to use Azure dev tools credentials to connect to Azure API Center. | `false` |
| `excludeProdCredentials` | Set to `true` for Dev Proxy not to use Azure production credentials to connect to Azure API Center. | `true` |
| `resourceGroupName` | Name of the resource group where the Azure API Center is located. | None |
| `serviceName` | Name of the Azure API Center instance that Dev Proxy should use to check if the APIs used in the app are registered. | None |
| `subscriptionId` | ID of the Azure subscription where the Azure API Center instance is located. | None |
| `workspace` | Name of the Azure API Center workspace to use. | `default` |

## Command line options

None

## Remarks

The `ApiCenterOnboardingPlugin` plugin checks if the APIs used in an app are registered in the specified Azure API Center instance. If the APIs aren't registered, the plugin can create new API entries in the API Center instance.

To connect to Azure API Center, the plugin uses Azure credentials. If you configure the `excludeDevCredentials` property to `false` (default), the plugin uses the following credentials (in this order):

- Shared Token Cache
- Visual Studio
- Visual Studio Code
- Azure CLI
- Azure PowerShell
- Azure Developer CLI

If you configure the `excludeProdCredentials` property to `false`, the plugin uses the following credentials (in this order):

- Environment
- Workload Identity
- Managed Identity

For local use we recommend to configure the `excludeDevCredentials` property to `false` and the `excludeProdCredentials` property to `true` to use the development credentials. For use in CI/CD environments, configure the `excludeDevCredentials` property to `true` and the `excludeProdCredentials` property to `false` to use the production credentials.

If the plugin fails to get an access token to access Azure, it shows an error and Dev Proxy disables it. Sign in to Azure using either of these tools, and restart Dev Proxy to use the `ApiCenterOnboardingPlugin` plugin.
