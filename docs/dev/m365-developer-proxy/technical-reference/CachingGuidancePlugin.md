Provides caching guidance when the same API calls are made repeatedly within a given timeframe.

## Plugin instance definition

```json
{
  "name": "CachingGuidancePlugin",
  "enabled": true,
  "pluginPath": "plugins\\m365-developer-proxy-plugins.dll",
  "configSection": "cachingGuidance"
}
```

## Configuration example

```json
{
  "cachingGuidance": {
    "cacheThresholdSeconds": 5
  }
}
```

## Configuration properties

| Property | Description | Default |
|----------|-------------|:-------:|
| CacheThresholdSeconds | Determines the number of seconds between the same request that triggers the guidance warning. | 5 |

## Command line options

None