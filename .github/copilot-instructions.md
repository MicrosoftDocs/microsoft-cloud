---
applyTo: docs/dev/dev-proxy/**
description: Guidelines for contributing to Dev Proxy documentation
---
# Dev Proxy Documentation Guidelines

This repository contains documentation for Dev Proxy, a command-line API simulator for testing app resilience. When working with this codebase, follow these guidelines:

## Architecture Overview

Dev Proxy uses a **plugin-based architecture** with three main plugin types:
- **Intercepting plugins**: Process requests/responses (inherit from `BaseProxyPlugin`)
- **Reporting plugins**: Analyze recorded data (inherit from `BaseReportingPlugin`) 
- **Reporters**: Convert reports to readable formats (inherit from `BaseReporter`)

## Configuration Patterns

### Standard Configuration Structure
All configuration files follow this pattern:
```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/rc.schema.json",
  "plugins": [
    {
      "name": "PluginName",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "myPluginConfig"
    }
  ],
  "urlsToWatch": ["https://api.example.com/*"],
  "myPluginConfig": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.2/pluginname.schema.json"
  }
}
```

### Plugin Ordering Matters
Plugins execute in the order listed in the configuration file. This is critical for proper behavior.

### Environment Variable Pattern
Use `@VARIABLE_NAME` syntax for environment variables in configurations:
```json
"subscriptionId": "@AZURE_SUBSCRIPTION_ID"
```

## Documentation Conventions

### File Naming
- Plugin docs: `technical-reference/pluginname.md` (lowercase)
- How-to guides: `how-to/action-description.md` (kebab-case)
- Concepts: `concepts/what-is-concept.md` or `concepts/how-to-verb.md`

### Metadata Requirements
Every documentation file must include:
```yaml
---
title: Descriptive Title
description: Brief description for SEO/navigation
author: githubusername
ms.author: microsoftusername
ms.date: MM/DD/YYYY
ms.topic: overview|get-started|how-to|reference|concept
---
```

### Plugin Documentation Structure
Each plugin reference must include:
1. **Plugin instance definition** (JSON block)
2. **Configuration example** (if configurable)
3. **Configuration properties** table
4. **Command line options** section
5. **Remarks** (if applicable)

### Code Block Standards
- Use `json` for configuration examples
- Use `console` for command-line examples
- Use `text` for Dev Proxy output examples
- Include schema references in JSON examples

## Content Patterns

### Cross-References
- Plugin architecture details: `docs/dev/dev-proxy/technical-reference/plugin-architecture.md`
- Complete plugin reference: `docs/dev/dev-proxy/technical-reference/overview.md`
- Preset configurations: `docs/dev/dev-proxy/how-to/use-preset-configurations.md`

### Common Scenarios Covered
- **Error simulation**: Random errors, specific API failures
- **Behavior simulation**: Throttling, rate limiting, latency
- **API mocking**: CRUD APIs, Microsoft Graph, OpenAI
- **Guidance**: Minimal permissions, best practices
- **Generation**: OpenAPI specs, HTTP files, TypeSpec

### Installation Instructions
Use pivot zones for OS-specific instructions:
```markdown
::: zone pivot="client-operating-system-windows"
::: zone-end
```

## Development Workflows

### Testing Documentation
- Local development uses `devproxy` command
- Configuration files typically named `devproxyrc.json` for auto-discovery
- Use `--config-file` parameter for custom configurations
- Always test with "first run" certificate installation steps

### VS Code Integration
Reference the Dev Proxy Toolkit extension for enhanced development experience with IntelliSense and snippets.

## Key File Locations

- **Main TOC**: `docs/dev/dev-proxy/TOC.yml` - hierarchical navigation structure
- **Plugin reference**: `docs/dev/dev-proxy/technical-reference/` - one file per plugin
- **How-to guides**: `docs/dev/dev-proxy/how-to/` - task-oriented documentation  
- **Concepts**: `docs/dev/dev-proxy/concepts/` - explanatory content
- **Getting started**: `docs/dev/dev-proxy/get-started/` - setup and configuration

When adding new plugins or features, ensure proper categorization in the TOC and maintain consistent cross-referencing between related concepts.
