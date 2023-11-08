Fails requests made to Microsoft Graph with random errors.

## Plugin instance definition

```json
{
  "name": "GraphRandomErrorPlugin",
  "enabled": false,
  "pluginPath": "plugins\\m365-developer-proxy-plugins.dll",
  "configSection": "graphRandomErrorsPlugin"
}
```

## Configuration example

```json
{
  "graphRandomErrorsPlugin": {
    "allowedErrors": [ 429, 500, 502, 503, 504, 507 ]
  }
}
```

## Configuration properties

| Property | Description | Default |
|----------|-------------|:-------:|
| AllowedErrors | List of [supported HTTP status codes](./Supported-HTTP-error-status-codes.md) that developer proxy might produce. |  |

## Command line options

| Name | Description | Default |
|----------|-------------|:-------:|
| `--allowed-errors` | List of [supported HTTP status codes](./Supported-HTTP-error-status-codes.md) that developer proxy might produce. |  |
