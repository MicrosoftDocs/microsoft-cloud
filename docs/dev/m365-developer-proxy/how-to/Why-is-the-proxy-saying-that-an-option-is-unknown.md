The proxy uses a plugin architecture, as such, proxy features can be added, removed, enabled or disabled through changing the configuration in the [m365proxyrc.json](./m365proxyrc.json) file. 

Some plugins provide command line options, when the plugin is disabled or removed from the configuration, the proxy is not able to interpret the command line options that the plugin provides.

By default, we ship with many plugins that are configured but disabled. 

For example, the [ExecutiveSummaryPlugin](./ExecutiveSummaryPlugin) provides the ability to record proxy activity and exposes command line options, such as `--summary-file-path`. As the plugin is disabled, the `--summary-file-path` option is an unknown option and so the proxy will return an error.

To enable a plugin and make its options available for use, open the [m365proxyrc.json](./m365proxyrc.json) file, locate the plugin and set `enabled` to `true`.

```json
{
  "name": "ExecutionSummaryPlugin",
  "enabled": true,
  "pluginPath": "plugins\\m365-developer-proxy-plugins.dll",
  "configSection": "executionSummaryPlugin"
}
```
