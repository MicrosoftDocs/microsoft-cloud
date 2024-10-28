---
title: MinimalPermissionsPlugin
description: MinimalPermissionsPlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/28/2024
---

# MinimalPermissionsPlugin

Checks if the app uses minimal permissions to call APIs. Uses API information from the specified local folder.

:::image type="content" source="../media/minimal-permissions-plugin.png" alt-text="Screenshot of a command line showing Dev Proxy checking if the recorded API requests use tokens minimal API permissions." lightbox="../media/minimal-permissions-plugin.png":::

## Plugin instance definition

```json
{
  "name": "MinimalPermissionsPlugin",
  "enabled": true,
  "pluginPath": "~appFolder/plugins/dev-proxy-plugins.dll",
  "configSection": "minimalPermissionsPlugin"
}
```

## Configuration example

```json
{
  "minimalPermissionsPlugin": {
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

The `MinimalPermissionsPlugin` plugin supports checking OAuth permissions for APIs secured with OAuth. The plugin computes the minimal permissions required to call the APIs used in the app using the information from the provided API specifications. Then, the plugin compares the permissions used in the JSON Web Token (JWT) token against the minimum required scopes needed for the requests that Dev Proxy recorded.

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

## More information

- [OpenAPI Authentication and Authorization](https://swagger.io/docs/specification/authentication/)
- [OpenAPI specification Security Scheme Object](https://swagger.io/specification/v3/#security-scheme-object)
