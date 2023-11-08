Returns a list of the minimal permission scopes required for requests sent to Microsoft Graph that proxy recorded.

> NOTE: This plugin requires [ExecutionSummaryPlugin](./ExecutionSummaryPlugin.md) to be enabled. This plugin contains the functionality to record proxy activity.

## Plugin instance definition

```json
{
  "name": "MinimalPermissionsPlugin",
  "enabled": false,
  "pluginPath": "plugins\\m365-developer-proxy-plugins.dll",
  "configSection": "minimalPermissionsPlugin"
}
```

## Configuration example

```json
{
  "minimalPermissionsPlugin": {
    "type": "delegated"
  }
}
```

## Configuration properties

| Property | Description | Default |
|----------|-------------|:-------:|
| Type | Determines which type of permission scopes to return. Can be `Delegated` or `Application` | `Delegated` | 

## Command line options

None
