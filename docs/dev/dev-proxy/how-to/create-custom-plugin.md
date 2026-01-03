---
title: Create a custom plugin
description: How to create a custom plugin for Dev Proxy
author: estruyf
ms.author: wmastyka
ms.date: 01/03/2026
---

<!-- INTENT: Build a custom Dev Proxy plugin -->
<!-- SOLUTION: Create .NET class implementing IProxyPlugin interface -->
<!-- RESULT: Custom plugin loaded by Dev Proxy for custom behaviors -->
<!-- PLUGINS: custom -->
<!-- JOB: extend-proxy -->
<!-- TIME: 30 minutes -->

# Create a custom plugin

> **At a glance**  
> **Goal:** Build a custom Dev Proxy plugin  
> **Time:** 30 minutes  
> **Plugins:** Custom plugin  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md), [.NET 9 SDK](https://dotnet.microsoft.com/download)

In this article, you learn how to create a custom plugin for the Dev Proxy. By creating plugins for Dev Proxy, you can extend its functionality and add custom features to fit your needs.

## Prerequisites

Before you start creating a custom plugin, make sure you have the following prerequisites:

- [.NET v9 Core SDK](https://dotnet.microsoft.com/download)
- The latest version of the Dev Proxy Abstractions DLL, which you can find on the [Dev Proxy GitHub releases](https://github.com/dotnet/dev-proxy/releases) page

## Create a new plugin

Follow the next steps to create a new project:

1. Create a new class library project using the `dotnet new classlib` command.

    ```console
    dotnet new classlib -n MyCustomPlugin
    ```

1. Open the newly created project in Visual Studio Code.

    ```console
    code MyCustomPlugin
    ```

1. Add the Dev Proxy Abstractions DLL (`DevProxy.Abstractions.dll`) to the project folder.
1. Add the `DevProxy.Abstractions.dll` as a reference to your project `DevProxyCustomPlugin.csproj` file.

    ```xml
    <ItemGroup>
      <Reference Include="DevProxy.Abstractions">
        <HintPath>.\DevProxy.Abstractions.dll</HintPath>
        <Private>false</Private>
        <ExcludeAssets>runtime</ExcludeAssets>
      </Reference>
    </ItemGroup>
    ```

1. Add the NuGet packages required for your project.

    ```console
    dotnet add package Microsoft.Extensions.Configuration
    dotnet add package Microsoft.Extensions.Configuration.Binder
    dotnet add package Microsoft.Extensions.Logging.Abstractions
    dotnet add package Unobtanium.Web.Proxy
    ```

1. Exclude the dependency DLLs from the build output by adding a `ExcludeAssets` tag per `PackageReference` in the `DevProxyCustomPlugin.csproj` file.

    ```xml
    <ExcludeAssets>runtime</ExcludeAssets>
    ```

1. Create a new class that inherits from the `BaseProxy` class.

    ```csharp
    using DevProxy.Abstractions.Plugins;
    using DevProxy.Abstractions.Proxy;
    using Microsoft.Extensions.Logging;

    namespace MyCustomPlugin;

    public sealed class CatchApiCallsPlugin(
        ILogger<CatchApiCallsPlugin> logger,
        ISet<UrlToWatch> urlsToWatch) : BasePlugin(logger, urlsToWatch)
    {
        public override string Name => nameof(CatchApiCallsPlugin);

        public override Task BeforeRequestAsync(ProxyRequestArgs e)
        {
            Logger.LogTrace("{Method} called", nameof(BeforeRequestAsync));

            ArgumentNullException.ThrowIfNull(e);

            if (!e.HasRequestUrlMatch(UrlsToWatch))
            {
                Logger.LogRequest("URL not matched", MessageType.Skipped, new(e.Session));
                return Task.CompletedTask;
            }

            var headers = e.Session.HttpClient.Request.Headers;
            var header = headers.Where(h => h.Name == "Authorization").FirstOrDefault();
            if (header is null)
            {
                Logger.LogRequest($"Does not contain the Authorization header", MessageType.Warning, new LoggingContext(e.Session));
                return Task.CompletedTask;
            }

            Logger.LogTrace("Left {Name}", nameof(BeforeRequestAsync));
            return Task.CompletedTask;
        }
    }
    ```

1. Build your project.

    ```console
    dotnet build
    ```

## Use your custom plugin

To use your custom plugin, you need to add it to the Dev Proxy configuration file:

1. Add the new plugin configuration in the `devproxyrc.json` file.

    **File:** devproxyrc.json

    ```json
    {
      "plugins": [{
        "name": "CatchApiCallsPlugin",
        "enabled": true,
        "pluginPath": "./bin/Debug/net9.0/MyCustomPlugin.dll",
      }]
    }
    ```

1. Run the Dev Proxy.

    ```console
    devproxy
    ```

The example plugin checks all matching URLs for the required `Authorization` header. If the header isn't present, it shows a warning message.

## Adding custom configuration to your plugin (optional)

You can extend your plugin's logic by adding custom configuration:

1. Inherit from the `BasePlugin<TConfiguration>` class. Dev Proxy exposes at runtime parsed plugin configuration through the `Configuration` property.

    ```csharp
    using DevProxy.Abstractions.Plugins;
    using DevProxy.Abstractions.Proxy;
    using Microsoft.Extensions.Configuration;
    using Microsoft.Extensions.Logging;

    namespace MyCustomPlugin;

    public sealed class CatchApiCallsConfiguration
    {
        public string? RequiredHeader { get; set; }
    }

    public sealed class CatchApiCallsPlugin(
        ILogger<CatchApiCallsPlugin> logger,
        ISet<UrlToWatch> urlsToWatch,
        IProxyConfiguration proxyConfiguration,
        IConfigurationSection pluginConfigurationSection) :
        BasePlugin<CatchApiCallsConfiguration>(
            logger,
            urlsToWatch,
            proxyConfiguration,
            pluginConfigurationSection)
    {
        public override string Name => nameof(CatchApiCallsPlugin);

        public override Task BeforeRequestAsync(ProxyRequestArgs e)
        {
            Logger.LogTrace("{Method} called", nameof(BeforeRequestAsync));

            ArgumentNullException.ThrowIfNull(e);

            if (!e.HasRequestUrlMatch(UrlsToWatch))
            {
                Logger.LogRequest("URL not matched", MessageType.Skipped, new(e.Session));
                return Task.CompletedTask;
            }

            // Start using your custom configuration
            var requiredHeader = Configuration.RequiredHeader ?? string.Empty;
            if (string.IsNullOrEmpty(requiredHeader))
            {
                // Required header is not set, so we don't need to do anything
                Logger.LogRequest("Required header not set", MessageType.Skipped, new LoggingContext(e.Session));
                return Task.CompletedTask;
            }

            var headers = e.Session.HttpClient.Request.Headers;
            var header = headers.Where(h => h.Name == requiredHeader).FirstOrDefault();
            if (header is null)
            {
                Logger.LogRequest($"Does not contain the {requiredHeader} header", MessageType.Warning, new LoggingContext(e.Session));
                return Task.CompletedTask;
            }

            Logger.LogTrace("Left {Name}", nameof(BeforeRequestAsync));
            return Task.CompletedTask;
        }
    }
    ```

1. Build your project.

    ```console
    dotnet build
    ```
  
1. Update your `devproxyrc.json` file to include the new configuration.

    **File:** devproxyrc.json

    ```json
    {
      "plugins": [{
        "name": "CatchApiCallsPlugin",
        "enabled": true,
        "pluginPath": "./bin/Debug/net9.0/MyCustomPlugin.dll",
        "configSection": "catchApiCalls"
      }],
      "catchApiCalls": {
        "requiredHeader": "Authorization" // Example configuration
      }
    }
    ```

1. Run the Dev Proxy.

    ```console
    devproxy
    ```

## See also

- [Plugin architecture](../technical-reference/plugin-architecture.md)
- [Use preset configurations](./use-preset-configurations.md)
- [Configure Dev Proxy](../get-started/configure.md)
