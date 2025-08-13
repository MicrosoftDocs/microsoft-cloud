---
title: MinimalPermissionsPlugin
description: MinimalPermissionsPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 04/30/2025
---

# MinimalPermissionsPlugin

Detects the minimal permissions needed to perform the specified API operations. Uses API information from the specified local folder.

:::image type="content" source="../media/minimal-permissions-plugin.png" alt-text="Screenshot of a command line showing Dev Proxy checking if the recorded API requests use tokens minimal API permissions." lightbox="../media/minimal-permissions-plugin.png":::

## Plugin instance definition

```json
{
  "name": "MinimalPermissionsPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
  "configSection": "minimalPermissionsPlugin"
}
```

## Configuration example

```json
{
  "minimalPermissionsPlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/minimalpermissionsplugin.schema.json",
    "apiSpecsFolderPath": "./api-specs"
  }
}
```

## Configuration properties

| Property | Description | Default |
|----------|-------------|:-------:|
| `apiSpecsFolderPath` | Relative or absolute path to the folder with API specs | None |

## Command line options

None

## Remarks

The `MinimalPermissionsPlugin` plugin checks if the app uses minimal permissions to call APIs. To check permissions, the plugin uses information about APIs located in the specified local folder.

### Define API permissions

The `MinimalPermissionsPlugin` plugin supports checking OAuth permissions for APIs secured with OAuth. The plugin computes the minimal permissions required to call the APIs used in the app using the information from the provided API specifications.

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
        // [...] trimmed for brevity
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
    "toolVersion": "0.22.0"
  }
}
```

The relevant part is the `securitySchemes` section, where you define the OAuth scopes that the API uses. Then, for each operation, you include the required scopes in the `security` section.

### Replace variables in API specs

Some API specs might contain variables in server URLs. Using variables is a common practice to accommodate different environments (for example, development, staging, production), API versions, or tenants. A URL with a variable looks like this:

```yml
openapi: 3.0.4
info:
  title: SharePoint REST API
  description: SharePoint REST API
  version: v1.0
servers:
  - url: https://{tenant}.sharepoint.com
    variables:
      tenant:
        default: contoso
```

The `MinimalPermissionsPlugin` plugin supports replacing variables in the API specs contents. To replace a variable, start Dev Proxy with the `--env` option and specify the variable name and value. For example, to replace the `tenant` variable with `contoso`, use the following command:

```bash
devproxy --env tenant=northwind
```

This command replaces the `tenant` variable in the API specs with the value `northwind`. The plugin uses the replaced URL to check if the app uses minimal permissions to call APIs.

## More information

- [OpenAPI Authentication and Authorization](https://swagger.io/docs/specification/authentication/)
- [OpenAPI specification Security Scheme Object](https://swagger.io/specification/v3/#security-scheme-object)
