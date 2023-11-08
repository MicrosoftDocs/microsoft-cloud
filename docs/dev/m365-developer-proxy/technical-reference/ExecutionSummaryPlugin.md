Records activity of requests that pass through proxy and generate a summary report from that activity.

## Plugin instance definition

```json
{
  "name": "ExecutionSummaryPlugin",
  "enabled": false,
  "pluginPath": "plugins\\m365-developer-proxy-plugins.dll",
  "configSection": "executionSummaryPlugin"
}
```

## Configuration example

```json
{
  "executionSummaryPlugin": {
    "groupBy": "url"
  }
}
```

## Configuration properties

| Property | Description | Default |
|----------|-------------|:-------:|
| FilePath | Path to the file where the summary should be saved. If not specified, the summary is printed to the console. Path can be absolute or relative to the current working directory. | No default | 
| GroupBy | Specifies how the information should be grouped in the summary. Available options: `url`, `messageType` | `url` |

## Command line options

| Name | Description | Default |
|----------|-------------|:-------:|
| `--summary-file-path` | Path to the file where the summary should be saved. If not specified, the summary is printed to the console. Path can be absolute or relative to the current working directory. | No default |
| `--summary-group-by` | Specifies how the information should be grouped in the summary. Available options: `url` (default), `messageType`. | `url` |
