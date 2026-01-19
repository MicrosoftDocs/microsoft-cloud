---
title: Dev Proxy Toolkit diagnostics
description: Reference for diagnostic codes shown by the Dev Proxy Toolkit VS Code extension
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/19/2026
---

<!-- INTENT: Help users and coding agents fix Dev Proxy configuration issues identified by the VS Code extension -->
<!-- USE-WHEN: A diagnostic code appears in VS Code Problems panel or as a squiggly underline in a Dev Proxy config file -->

# Dev Proxy Toolkit diagnostics

The Dev Proxy Toolkit VS Code extension analyzes your Dev Proxy configuration files and highlights potential issues. Each diagnostic has a code that links to this page for resolution guidance.

## Diagnostic codes reference

| Code | Severity | Description |
|------|----------|-------------|
| [invalidSchema](#invalidschema) | Warning | Schema version mismatch with installed Dev Proxy |
| [invalidConfigSection](#invalidconfigsection) | Warning | Orphaned configuration section |
| [deprecatedPluginPath](#deprecatedpluginpath) | Error | Using old plugin DLL path |
| [missingLanguageModel](#missinglanguagemodel) | Warning | Language model required but not enabled |
| [noEnabledPlugins](#noenabledplugins) | Warning | No plugins are enabled |
| [reporterPosition](#reporterposition) | Warning | Reporter plugin not at end of list |
| [summaryWithoutReporter](#summarywithoutreporter) | Warning | Summary plugin without reporter |
| [apiCenterPluginOrder](#apicenterpluginorder) | Warning | Incorrect plugin order for API Center |
| [emptyUrlsToWatch](#emptyurlstowatch) | Information | No URLs configured to intercept |
| [pluginConfigRequired](#pluginconfigrequired) | Error/Warning | Plugin requires configuration |
| [pluginConfigMissing](#pluginconfigmissing) | Error/Warning | Referenced config section missing |
| [pluginConfigNotRequired](#pluginconfignotrequired) | Error/Warning | Unnecessary config section |

## invalidSchema

**Severity:** Warning  
**Quick fix available:** Yes

The `$schema` property doesn't match the installed Dev Proxy version.

### Cause

Your configuration references a different schema version than your installed Dev Proxy. This can cause validation issues or missing features.

### Resolution

Update the `$schema` property to match your installed version. Use the quick fix (light bulb icon or `Ctrl+.` / `Cmd+.`) to update automatically.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json"
}
```

## invalidConfigSection

**Severity:** Warning  
**Quick fix available:** No

A configuration section exists but no plugin references it.

### Cause

A top-level object exists that isn't used as a `configSection` by any plugin. This often happens after removing a plugin but forgetting to remove its configuration.

### Resolution

Either remove the orphaned section or add a plugin that references it:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "genericRandomErrorPlugin"
    }
  ],
  "urlsToWatch": [
    "https://api.example.com/*"
  ],
  "genericRandomErrorPlugin": {
    "rate": 50
  }
}
```

## deprecatedPluginPath

**Severity:** Error  
**Quick fix available:** Yes

The `pluginPath` uses a deprecated path format.

### Cause

In Dev Proxy v0.29, the plugin DLL was renamed from `dev-proxy-plugins.dll` to `DevProxy.Plugins.dll`.

### Resolution

Update the `pluginPath` to use the new format. Use the quick fix to update automatically.

```json
{
  "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
}
```

## missingLanguageModel

**Severity:** Warning  
**Quick fix available:** Yes

A plugin requires the language model but `languageModel.enabled` isn't `true`.

### Cause

Plugins like `LanguageModelFailurePlugin` and `LanguageModelRateLimitingPlugin` require a language model connection.

### Resolution

Add or update the `languageModel` configuration. Use the quick fix to add automatically.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "LanguageModelFailurePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "languageModelFailurePlugin"
    }
  ],
  "urlsToWatch": [
    "https://api.openai.com/*"
  ],
  "languageModel": {
    "enabled": true
  },
  "languageModelFailurePlugin": {
    "rate": 50
  }
}
```

## noEnabledPlugins

**Severity:** Warning  
**Quick fix available:** No

The `plugins` array is empty or all plugins are disabled.

### Cause

Dev Proxy requires at least one enabled plugin to process requests.

### Resolution

Add at least one plugin with `enabled: true`:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "MockResponsePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "mockResponsePlugin"
    }
  ],
  "urlsToWatch": [
    "https://api.example.com/*"
  ],
  "mockResponsePlugin": {
    "mocksFile": "mocks.json"
  }
}
```

## reporterPosition

**Severity:** Warning  
**Quick fix available:** No

A reporter plugin is placed before other nonreporter plugins.

### Cause

Reporter plugins collect output from other plugins. Placing them before other plugins means they won't capture all data.

### Resolution

Move reporter plugins to the end of the `plugins` array:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "MockResponsePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    },
    {
      "name": "LatencyPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    },
    {
      "name": "PlainTextReporterPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ],
  "urlsToWatch": [
    "https://api.example.com/*"
  ]
}
```

## summaryWithoutReporter

**Severity:** Warning  
**Quick fix available:** No

A summary plugin is enabled without a reporter plugin.

### Cause

Plugins like `ExecutionSummaryPlugin` and `UrlDiscoveryPlugin` generate data that requires a reporter plugin to output.

### Resolution

Add a reporter plugin after the summary plugin:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "ExecutionSummaryPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    },
    {
      "name": "PlainTextReporterPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ],
  "urlsToWatch": [
    "https://api.example.com/*"
  ]
}
```

## apiCenterPluginOrder

**Severity:** Warning  
**Quick fix available:** No

`OpenApiSpecGeneratorPlugin` is placed after `ApiCenterOnboardingPlugin`.

### Cause

`OpenApiSpecGeneratorPlugin` must generate the OpenAPI specification before `ApiCenterOnboardingPlugin` can use it.

### Resolution

Place `OpenApiSpecGeneratorPlugin` before `ApiCenterOnboardingPlugin`:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "OpenApiSpecGeneratorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    },
    {
      "name": "ApiCenterOnboardingPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "apiCenterOnboardingPlugin"
    }
  ],
  "urlsToWatch": [
    "https://api.example.com/*"
  ],
  "apiCenterOnboardingPlugin": {
    "subscriptionId": "your-subscription-id",
    "resourceGroupName": "your-resource-group",
    "serviceName": "your-api-center",
    "workspaceName": "default"
  }
}
```

## emptyUrlsToWatch

**Severity:** Information  
**Quick fix available:** No

The `urlsToWatch` array is empty.

### Cause

Dev Proxy uses `urlsToWatch` to determine which requests to intercept. An empty array means no requests will be processed.

### Resolution

Add URL patterns to intercept:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "MockResponsePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ],
  "urlsToWatch": [
    "https://api.example.com/*",
    "https://graph.microsoft.com/v1.0/*"
  ]
}
```

## pluginConfigRequired

**Severity:** Error (enabled plugin) / Warning (disabled plugin)  
**Quick fix available:** No

A plugin requires a `configSection` but none is specified.

### Cause

Some plugins require configuration to function. The plugin definition is missing the `configSection` property.

### Resolution

Add a `configSection` property and create the configuration object. Use the snippet name shown in the diagnostic message.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "GenericRandomErrorPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "genericRandomErrorPlugin"
    }
  ],
  "urlsToWatch": [
    "https://api.example.com/*"
  ],
  "genericRandomErrorPlugin": {
    "errorsFile": "errors.json",
    "rate": 50
  }
}
```

## pluginConfigMissing

**Severity:** Error (enabled plugin) / Warning (disabled plugin)  
**Quick fix available:** No

A plugin references a `configSection` that doesn't exist.

### Cause

The plugin has a `configSection` property but the corresponding configuration object is missing from the file.

### Resolution

Add the missing configuration section. Use the snippet name shown in the diagnostic message.

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "MockResponsePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "mockResponsePlugin"
    }
  ],
  "urlsToWatch": [
    "https://api.example.com/*"
  ],
  "mockResponsePlugin": {
    "mocksFile": "mocks.json"
  }
}
```

## pluginConfigNotRequired

**Severity:** Error (enabled plugin) / Warning (disabled plugin)  
**Quick fix available:** No

A plugin has a `configSection` but doesn't support configuration.

### Cause

Not all plugins support custom configuration. Adding a `configSection` to a plugin that doesn't need one has no effect.

### Resolution

Remove the `configSection` property:

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.0.0/rc.schema.json",
  "plugins": [
    {
      "name": "RetryAfterPlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
    }
  ],
  "urlsToWatch": [
    "https://api.example.com/*"
  ]
}
```
