---
title: Generate an HTTP file
description: How to generate an HTTP file from the intercepted API requests and responses
author: waldekmastykarz
ms.author: wmastyka
ms.date: 06/25/2025
---

# Generate an HTTP file

Dev Proxy allows you to generate an HTTP file from intercepted API requests and responses. Using HTTP files is especially useful for developers who want to simulate API behavior or share reproducible API interactions. The HTTP file includes all relevant request and response details, with sensitive information replaced by variables for security and reusability.

To generate an HTTP file using Dev Proxy:

1. In the configuration file, enable the `HttpFileGeneratorPlugin`:

   ```json
   {
     "plugins": [
       {
         "name": "HttpFileGeneratorPlugin",
         "enabled": true,
         "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
         "configSection": "httpFileGeneratorPlugin"
       }
     ]
     // [...] shortened for brevity
   }
   ```

1. Optionally, configure the plugin:

   ```json
   {
     "httpFileGeneratorPlugin": {
       "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v0.29.1/httpfilegeneratorplugin.schema.json",
       "includeOptionsRequests": false
     }
     // [...] shortened for brevity
   }
   ```

   - `includeOptionsRequests`: Determines whether to include `OPTIONS` requests in the generated HTTP file. Default is `false`.

1. In the configuration file, to the list of URLs to watch, add the URL of the API for which you want to generate an HTTP file:

   ```json
   {
     "urlsToWatch": [
       "https://api.example.com/*"
     ]
     // [...] shortened for brevity
   }
   ```

1. Start Dev Proxy:

   ```console
   devproxy
   ```

1. Start recording requests by pressing `r`.

1. Perform the API requests you want to include in the HTTP file.

1. Stop recording by pressing `s`.

1. Dev Proxy generates an HTTP file and saves it in the current directory. The file includes all captured requests and responses, with sensitive data like bearer tokens and API keys replaced by variables. For example:

   ```http
   @jsonplaceholder_typicode_com_api_key = api-key
   ###
   # @name getPosts
   GET https://jsonplaceholder.typicode.com/posts?api-key={{jsonplaceholder_typicode_com_api_key}}
   Host: jsonplaceholder.typicode.com
   User-Agent: curl/8.6.0
   Accept: */*
   Via: 1.1 dev-proxy/0.29.0
   ```

   The plugin automatically creates variables for each combination of hostname and sensitive parameter, reusing them across requests when applicable.

:::image type="content" source="../media/http-file-generator-plugin.png" alt-text="Screenshot of two command prompt windows. One shows Dev Proxy recording API requests. The other shows the generated HTTP file." lightbox="../media/http-file-generator-plugin.png":::

## Next steps

Learn more about the HttpFileGeneratorPlugin.

> [!div class="nextstepaction"]
> [HttpFileGeneratorPlugin](../technical-reference/httpfilegeneratorplugin.md)