---
title: MinimalCsomPermissionsPlugin
description: MinimalCsomPermissionsPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/06/2026
---

<!-- INTENT: Detect minimal permissions for SharePoint CSOM APIs -->
<!-- PLUGIN-TYPE: Reporting -->
<!-- WORKS-WITH: MarkdownReporter, JsonReporter -->
<!-- USE-WHEN: Auditing SharePoint CSOM permissions -->

# MinimalCsomPermissionsPlugin

Detects minimal permissions needed to call the recorded SharePoint Client-Side Object Model (CSOM) API requests.

:::image type="content" source="../media/minimal-csom-permissions-plugin.png" alt-text="Screenshot of a command line showing Dev Proxy listing minimal permissions required to call the recorded set of SharePoint CSOM APIs." lightbox="../media/minimal-permissions-plugin.png":::

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "MinimalCsomPermissionsPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "minimalCsomPermissionsPlugin"
    }
  ],
  "minimalCsomPermissionsPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/minimalcsompermissionsplugin.schema.json",
    "typesFilePath": "./api-specs"
  }
}
```

## Configuration properties

| Property | Description | Default |
|----------|-------------|:-------:|
| `typesFilePath` | Path to the file that lists permissions required to call SharePoint CSOM APIs | `~appFolder/config/spo-csom-types.json` |

## Command line options

None

## Remarks

The `MinimalCsomPermissionsPlugin` plugin detects what permissions the client application needs at minimum to call the set of SharePoint CSOM APIs. To detect these minimal permissions, the plugin uses information about permissions for SharePoint CSOM APIs located in the specified SharePoint CSOM types file.

### SharePoint CSOM types file

The `MinimalCsomPermissionsPlugin` uses a CSOM types file to determine what minimal permissions the client application needs to call the specific set of CSOM APIs. The CSOM types file is a JSON file that contains information about SharePoint CSOM types and their permissions. The following example shows an example CSOM types file:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/minimalcsompermissions.types.schema.json",
  "types": {
    "268004ae-ef6b-4e9b-8425-127220d84719": "Microsoft.Online.SharePoint.TenantAdministration.Tenant",
    "3747adcd-a3c3-41b9-bfab-4a64dd2f1e0a": "Microsoft.SharePoint.Client.RequestContext"
  },
  "returnTypes": {
    "Microsoft.SharePoint.Client.RequestContext.Current.Site": "Microsoft.SharePoint.Client.Site"
  },
  "actions": {
    "Microsoft.SharePoint.Client.RequestContext.Current": {
      "delegated": [
        "AllSites.Read",
        "AllSites.Write",
        "AllSites.Manage",
        "AllSites.FullControl"
      ],
      "application": []
    },
    "Microsoft.SharePoint.Client.Site.setProperty": {
      "delegated": [
        "AllSites.FullControl"
      ],
      "application": []
    },
    "Microsoft.Online.SharePoint.TenantAdministration.Tenant.ctor": {
      "delegated": [
        "AllSites.Read",
        "AllSites.Write",
        "AllSites.Manage",
        "AllSites.FullControl"
      ],
      "application": [
      ]
    },
    "Microsoft.Online.SharePoint.TenantAdministration.Tenant.query": {
      "delegated": [
        "AllSites.Write",
        "AllSites.Manage",
        "AllSites.FullControl"
      ],
      "application": [
      ]
    },
    "Microsoft.Online.SharePoint.TenantAdministration.Tenant.GetSitePropertiesFromSharePointByFilters": {
      "delegated": [
        "AllSites.FullControl"
      ],
      "application": [
      ]
    }
  }
}
```

The file consists of three main parts:

- `types`
- `returnTypes`
- `actions`

The `types` section contains a list of SharePoint CSOM types and their IDs. This section is included for readability because it's easier to understand `Microsoft.SharePoint.Client.RequestContext.Current` than `3747adcd-a3c3-41b9-bfab-4a64dd2f1e0a.Current`.

The `returnTypes` section contains a list of return types for the methods in the `actions` section. The plugin uses this information when parsing CSOM requests to traverse the hierarchy of the CSOM APIs.

The `actions` section contains a list of actions that can be performed using SharePoint CSOM APIs. For each action, it lists the delegated and application permissions that a client application can use to perform this action. Permissions are sorted ascending by their privilege, so that the least privileged permissions are listed first.

As of today, the CSOM types file that we include with Dev Proxy, is incomplete. We're working together with our community to document more types. Meanwhile, you can update it to include the types and permissions that you need if the information that you need is missing. You can then use your own types file. We encourage you to contribute your changes back to the community so that we can all benefit from them, by creating a pull request in the [Dev Proxy samples repository](https://github.com/pnp/proxy-samples/tree/main/samples/sharepoint-api-minimal-permissions). We regularly pull the latest changes from the repository to the Dev Proxy repository so that you can benefit from the latest changes without having to update your Dev Proxy installation.
