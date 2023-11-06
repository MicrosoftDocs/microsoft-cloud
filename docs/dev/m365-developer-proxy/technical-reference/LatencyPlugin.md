Provides the ability to introduce latency into requests.

## Plugin instance definition

```json
{
  "name": "LatencyPlugin",
  "enabled": true,
  "pluginPath": "plugins\\m365-developer-proxy-plugins.dll",
  "configSection": "latencyPlugin"
}
```

## Configuration example

```json
"latencyPlugin": {
  "minMs": 200,
  "maxMs": 10000
},
```

## Configuration properties

| Property | Description | Default |
| -------- | ----------- | :-----: |
| `minMs` | The minimum amount of delay added to a request in milliseconds. |   0 | 
| `maxMs` | The maximum amount of delay added to a request in milliseconds. Max value is 10000 (10s) |  5000  |

## Command line options

None
