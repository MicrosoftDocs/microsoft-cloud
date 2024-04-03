---
title: Create a custom plugin
description: How to create a custom plugin for Dev Proxy
author: estruyf
ms.author: 
ms.date: 03/28/2024
---

# Create a custom plugin

In this article, you will learn how to create a custom plugin for the Dev Proxy. By creating plugins for Dev Proxy, you can extend its functionality and add custom features to fit your needs.

## Prerequisites

Before you start creating a custom plugin, make sure you have the following prerequisites:

- [.NET Core SDK](https://dotnet.microsoft.com/download)
- The latest version of the Dev Proxy Abstractions DLL, which you can find on the [Dev Proxy GitHub releases](https://github.com/microsoft/dev-proxy/releases) page

## Create a new plugin

Follow the next steps to create a new project:

1. Create a new class library project using the `dotnet new classlib` command.

    ```bash
    dotnet new classlib -n MyCustomPlugin
    ```

1. Open the newly created project in Visual Studio Code.

    ```bash
    code MyCustomPlugin
    ```

1. Add the Dev Proxy Abstractions DLL (`dev-proxy-abstractions.dll`) to the project folder.
1. Add the `dev-proxy-abstractions.dll` as a reference to your project `DevProxyCustomPlugin.csproj` file.

    ```xml
    <ItemGroup>
      <Reference Include="dev-proxy-abstractions">
        <HintPath>.\dev-proxy-abstractions.dll</HintPath>
        <Private>false</Private>
        <ExcludeAssets>runtime</ExcludeAssets>
      </Reference>
    </ItemGroup>
    ```

1. Add the NuGet packages required for your project.

    ```bash
    dotnet add package Microsoft.Extensions.Configuration
    dotnet add package Microsoft.Extensions.Configuration.Binder
    dotnet add package Microsoft.Extensions.Logging.Abstractions
    dotnet add package Titanium.Web.Proxy
    ```

1. Exclude the dependency DLLs from the build output by adding a `ExcludeAssets` tag per `PackageReference` in the `DevProxyCustomPlugin.csproj` file.

    ```xml
    <ExcludeAssets>runtime</ExcludeAssets>
    ```

1. Create a new class that implements the `BaseProxyPlugin` interface.

    ```csharp
    using Microsoft.DevProxy.Abstractions;
    using Microsoft.Extensions.Configuration;

    namespace MyCustomPlugin;

    public class CatchApiCalls : BaseProxyPlugin
    {
      public override string Name => nameof(CatchApiCalls);

      public override void Register(IPluginEvents pluginEvents,
                                    IProxyContext context,
                                    ISet<UrlToWatch> urlsToWatch,
                                    IConfigurationSection? configSection = null)
      {
        base.Register(pluginEvents, context, urlsToWatch, configSection);

        // Register your event handlers
        pluginEvents.BeforeRequest += OnBeforeRequest;
      }

      private Task OnBeforeRequest(object sender, ProxyRequestArgs e)
      {
        if (_urlsToWatch is null ||
          !e.HasRequestUrlMatch(_urlsToWatch))
        {
          // No match for the URL, so we don't need to do anything
          return Task.CompletedTask;
        }

        var headers = e.Session.HttpClient.Request.Headers;
        var header = headers.Where(h => h.Name == "Authorization").FirstOrDefault();
        if (header is null)
        {
          _logger?.LogRequest([$"Does not contain the Authorization header"], MessageType.Warning);
          return Task.CompletedTask;
        }

        return Task.CompletedTask;
      }
    }
    ```

1. Build your project.

    ```bash
    dotnet build
    ```

## Use your custom plugin

To use your custom plugin, you need to add it to the Dev Proxy configuration file. Here is an example of how you can do this:

1. Add the new plugin configuration in the `devproxyrc.json` file.

    ```json
    {
      "plugins": [{
        "name": "CatchApiCalls",
        "enabled": true,
        "pluginPath": "./bin/Debug/net8.0/MyCustomPlugin.dll",
      }]
    }
    ```

1. Run the Dev Proxy.

    ```bash
    devproxy
    ```

The example plugin will check all matching URLs for the required Authorization header. If the header is not present, it will show a warning message.

## Adding custom configuration to your plugin (optional)

You can extend your plugin's logic by adding custom configuration. Here is an example of how you can do this:

1. Add a new `_configuration` object and bind it in the `Register` method.

    ```csharp
    using Microsoft.DevProxy.Abstractions;
    using Microsoft.Extensions.Configuration;

    namespace MyCustomPlugin;

    public class CatchApiCallsConfiguration
    {
      public string? RequiredHeader { get; set; }
    }

    public class CatchApiCalls : BaseProxyPlugin
    {
      public override string Name => nameof(CatchApiCalls);

      // Define you custom configuration
      private readonly CatchApiCallsConfiguration _configuration = new();

      public override void Register(IPluginEvents pluginEvents,
                                    IProxyContext context,
                                    ISet<UrlToWatch> urlsToWatch,
                                    IConfigurationSection? configSection = null)
      {
        base.Register(pluginEvents, context, urlsToWatch, configSection);

        // Bind your plugin configuration
        configSection?.Bind(_configuration);

        // Register your event handlers
        pluginEvents.BeforeRequest += OnBeforeRequest;
      }

      private Task OnBeforeRequest(object sender, ProxyRequestArgs e)
      {
        if (_urlsToWatch is null ||
          !e.HasRequestUrlMatch(_urlsToWatch))
        {
          // No match for the URL, so we don't need to do anything
          return Task.CompletedTask;
        }

        // Start using your custom configuration
        var requiredHeader = _configuration?.RequiredHeader ?? string.Empty;
        if (string.IsNullOrEmpty(requiredHeader))
        {
          // Required header is not set, so we don't need to do anything
          return Task.CompletedTask;
        }

        var headers = e.Session.HttpClient.Request.Headers;
        var header = headers.Where(h => h.Name == requiredHeader).FirstOrDefault();
        if (header is null)
        {
          _logger?.LogRequest([$"Does not contain the {requiredHeader} header"], MessageType.Warning);
          return Task.CompletedTask;
        }

        return Task.CompletedTask;
      }
    }
    ```

1. Build your project.

    ```bash
    dotnet build
    ```
  
1. Update your `devproxyrc.json` file to include the new configuration.

    ```json
    {
      "plugins": [{
        "name": "CatchApiCalls",
        "enabled": true,
        "pluginPath": "./bin/Debug/net8.0/MyCustomPlugin.dll",
        "configSection": "catchApiCalls"
      }],
      "catchApiCalls": {
        "requiredHeader": "Authorization" // Example configuration
      }
    }
    ```

1. Run the Dev Proxy.

    ```bash
    devproxy
    ```
