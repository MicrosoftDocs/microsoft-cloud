Simulates rate-limit API behaviors.

## Plugin instance definition

```json
{
  "name": "MockResponsePlugin",
  "enabled": true,
  "pluginPath": "plugins\\m365-developer-proxy-plugins.dll",
  "configSection": "mocksPlugin"
}
```

## Configuration example

```json
{
  "mocksPlugin": {
    "mocksFile": "responses.json"
  }
}
```

## Configuration properties

| Property              | Description                                                        |     Default      |
| --------------------- | ------------------------------------------------------------------ | :--------------: |
| MocksFile             | Path to the file containing mock responses                         | `responses.json` |
| BlockUnmockedRequests | Return `502 Bad Gateway` response for requests that aren't mocked |     `false`      |

## Command line options

| Name             | Description                                | Default |
| ---------------- | ------------------------------------------ | :-----: |
| `-n, --no-mocks` | Disable loading mock requests              |    -    |
| `--mocks-file`   | Path to the file containing mock responses |    -    |
