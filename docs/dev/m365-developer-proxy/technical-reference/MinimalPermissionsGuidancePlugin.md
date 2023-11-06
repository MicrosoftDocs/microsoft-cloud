Provides guidance which compares the permission scopes used in the JWT token sent to Microsoft Graph against the minimum required scopes needed for requests recorded by the proxy.

> NOTE: This plugin requires [ExecutionSummaryPlugin](./ExecutionSummaryPlugin) to be enabled. This plugin contains the functionality to record proxy activity.

## Plugin instance definition

```json
{
  "name": "MinimalPermissionsGuidancePlugin",
  "enabled": false,
  "pluginPath": "plugins\\m365-developer-proxy-plugins.dll"
}
```

## Configuration example

None

## Configuration properties

None

## Command line options

None