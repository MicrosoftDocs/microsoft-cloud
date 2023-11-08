Fails requests with a random selected error from file containing mocked errors.

## Plugin instance definition

```json
{
  "name": "GenericRandomErrorPlugin",
  "enabled": true,
  "pluginPath": "plugins\\m365-developer-proxy-plugins.dll",
  "configSection": "genericRandomErrorPlugin",
  "urlsToWatch": [
    "https://api.openai.com/*"
  ]
}
```

## Configuration example

```json
{
  "genericRandomErrorPlugin": {
    "rate": 90,
    "errorsFile": "errors.json"
  },
}
```

## Configuration properties

| Property | Description | Default |
|----------|-------------|:-------:|
| Rate | Determines the percentage chance for a request to fail with an error. | No default |
| ErrorsFile | Path to the file that contains error responses. | No default |

## Command line options

None
