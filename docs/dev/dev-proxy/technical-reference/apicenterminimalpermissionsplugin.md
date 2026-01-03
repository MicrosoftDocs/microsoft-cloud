---
title: ApiCenterMinimalPermissionsPlugin
description: ApiCenterMinimalPermissionsPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/06/2026
---

<!-- INTENT: Check API permissions against Azure API Center specs -->
<!-- PLUGIN-TYPE: Reporting -->
<!-- WORKS-WITH: ApiCenterOnboardingPlugin, ApiCenterProductionVersionPlugin, MarkdownReporter -->
<!-- USE-WHEN: Auditing API permissions using Azure API Center governance -->

# ApiCenterMinimalPermissionsPlugin

Checks if the app uses minimal permissions to call APIs. Uses API information from the specified Azure API Center instance.

:::image type="content" source="../media/api-center-minimal-permissions-plugin.png" alt-text="Screenshot of a command prompt showing Dev Proxy checking if the recorded API requests use tokens minimal API permissions." lightbox="../media/api-center-minimal-permissions-plugin.png":::

## Configuration example

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "ApiCenterMinimalPermissionsPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "apiCenterMinimalPermissionsPlugin"
    }
  ],
  "apiCenterMinimalPermissionsPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/apicenterminimalpermissionsplugin.schema.json",
    "subscriptionId": "aaaa0a0a-bb1b-cc2c-dd3d-eeeeee4e4e4e",
    "resourceGroupName": "resource-group-name",
    "serviceName": "apic-instance",
    "workspace": "default"
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
| `schemeName` | Name of the security scheme definition used to determine minimal permissions. If not specified, the plugin reads information from the first oauth2 scheme in the spec | None |

## Command line options

None

## Remarks

The `ApiCenterMinimalPermissionsPlugin` plugin checks if the app uses minimal permissions to call APIs. To check permissions, the plugin uses information about APIs that are registered in the specified Azure API Center instance.

### Connect to Azure API Center

To connect to Azure API Center, the plugin uses Azure credentials (in this order):

- Environment
- Workload Identity
- Managed Identity
- Visual Studio
- Visual Studio Code
- Azure CLI
- Azure PowerShell
- Azure Developer CLI

If the plugin fails to get an access token to access Azure, it shows an error and Dev Proxy disables it. Sign in to Azure using either of these tools, and restart Dev Proxy to use the `ApiCenterMinimalPermissionsPlugin` plugin.

If you use Dev Proxy in CI/CD pipelines, you can pass values for the `subscriptionId`, `resourceGroupName`, `serviceName`, and `workspace` properties as environment variables. To use environment variables, prepend the name of the value with a `@`, for example:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "ApiCenterMinimalPermissionsPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "apiCenterMinimalPermissionsPlugin"
    }
  ],
  "apiCenterMinimalPermissionsPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/apicenterminimalpermissionsplugin.schema.json",
    "subscriptionId": "@AZURE_SUBSCRIPTION_ID",
    "resourceGroupName": "@AZURE_RESOURCE_GROUP_NAME",
    "serviceName": "@AZURE_APIC_INSTANCE_NAME",
    "workspace": "@AZURE_APIC_WORKSPACE_NAME"
  }
}
```

In this example, the `ApiCenterMinimalPermissionsPlugin` plugin sets `subscriptionId`, `resourceGroupName`, `serviceName`, and `workspace` properties to the values of the `AZURE_SUBSCRIPTION_ID`, `AZURE_RESOURCE_GROUP_NAME`, `AZURE_APIC_INSTANCE_NAME`, and `AZURE_APIC_WORKSPACE_NAME` environment variables, respectively.

### Define API permissions

The `ApiCenterMinimalPermissionsPlugin` plugin supports checking OAuth permissions for APIs secured with OAuth registered in Azure API Center. The plugin computes the minimal permissions required to call the APIs used in the app using the information from API Center. Then, the plugin compares the permissions used in the JSON Web Token (JWT) token against the minimum required scopes needed for the requests that Dev Proxy recorded.

To define permissions for your APIs, include them in the OpenAPI definition of your API. The following example shows how to define permissions for an API in an OpenAPI definition:

```json
{
  "openapi": "3.0.1",
  "info": {
    "title": "Northwind API",
    "description": "Northwind API",
    "version": "v1.0"
  },
  "servers": [
    {
      "url": "https://api.northwind.com"
    }
  ],
  "components": {
    "securitySchemes": {
      "OAuth2": {
        "type": "oauth2",
        "flows": {
          "authorizationCode": {
            "authorizationUrl": "https://login.microsoftonline.com/common/oauth2/authorize",
            "tokenUrl": "https://login.microsoftonline.com/common/oauth2/token",
            "scopes": {
              "customer.read": "Grants access to ready customer info",
              "customer.readwrite": "Grants access to read and write customer info"
            }
          }
        }
      }
    },
    "schemas": {
      "Customer": {
        "type": "object",
        "properties": {}
      }
    }
  },
  "paths": {
    "/customers/{customers-id}": {
      "description": "Provides operations to manage a customer",
      "get": {
        "summary": "Get customer by ID",
        "operationId": "getCustomerById",
        "security": [
          {
            "OAuth2": [
              "customer.read"
            ]
          },
          {
            "OAuth2": [
              "customer.readwrite"
            ]
          }
        ],
        "responses": {
          "200": {
            "description": "OK",
            "content": {
              "application/json; charset=utf-8": {
                "schema": {
                  "$ref": "#/components/schemas/Customer"
                }
              }
            }
          }
        }
      },
      "patch": {
        "summary": "Update customer by ID",
        "operationId": "updateCustomerById",
        "security": [
          {
            "OAuth2": [
              "customer.readwrite"
            ]
          }
        ],
        "requestBody": {
          "required": true,
          "content": {
            "application/json": {
              "schema": {
                "$ref": "#/components/schemas/Customer"
              }
            }
          }
        },
        "responses": {
          "204": {
            "description": "No Content"
          }
        }
      },
      "parameters": [
        {
          "name": "customers-id",
          "in": "path",
          "required": true,
          "schema": {
            "type": "string"
          }
        }
      ]
    }
  },
  "x-ms-generated-by": {
    "toolName": "Dev Proxy",
    "toolVersion": "0.27.0"
  }
}
```

The relevant part is the `securitySchemes` section, where you define the OAuth scopes that the API uses. Then, for each operation, you include the required scopes in the `security` section.

## More information

- [OpenAPI Authentication and Authorization](https://swagger.io/docs/specification/authentication/)
- [OpenAPI specification Security Scheme Object](https://swagger.io/specification/v3/#security-scheme-object)

## Next step

> [!div class="nextstepaction"]
> [How to check if my app is calling APIs with minimal permissions](../how-to/check-minimal-api-permissions.md)
