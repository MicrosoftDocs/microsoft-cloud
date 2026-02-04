---
title: Understand language model usage
description: How to use Dev Proxy to intercept OpenAI-compatible requests and responses to understand how your application uses large language models.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/06/2026
---

<!-- INTENT: Monitor LLM usage with OpenTelemetry -->
<!-- SOLUTION: Enable OpenAITelemetryPlugin with OTLP exporter -->
<!-- RESULT: LLM usage metrics exported to monitoring system -->
<!-- PLUGINS: OpenAITelemetryPlugin -->
<!-- JOB: analyze-usage -->
<!-- TIME: 20 minutes -->

# Understand language model usage

> **At a glance**  
> **Goal:** Monitor LLM usage with OpenTelemetry  
> **Time:** 20 minutes  
> **Plugins:** [OpenAITelemetryPlugin](../technical-reference/openaitelemetryplugin.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md), Docker

Using language models incurs costs. To understand how your application uses large language models, use Dev Proxy to intercept OpenAI-compatible requests and responses. Dev Proxy analyzes the requests and responses and logs telemetry data to help you understand how your application uses large language models. This information allows you to optimize your application and reduce costs.

Dev Proxy logs language model usage data in OpenTelemetry format. You can use any OpenTelemetry-compatible dashboard to visualize the data. For example, you can use the [.NET Aspire dashboard](/dotnet/aspire/fundamentals/dashboard/standalone) or [OpenLIT](https://openlit.io/). The telemetry data includes the number of tokens used in the request and response, the cost of the tokens used, and the total cost of all requests over the course of a session.

## Intercept OpenAI-compatible requests and responses using Dev Proxy

To intercept OpenAI-compatible requests and responses, use the [OpenAITelemetryPlugin](../technical-reference/openaitelemetryplugin.md). This plugin logs telemetry data from the OpenAI-compatible requests and responses that it intercepts and emits OpenTelemetry data.

### Create a Dev Proxy configuration file

1. Create a new Dev Proxy configuration file using the `devproxy config new` command or using the Dev Proxy Toolkit extension.
1. Add the `OpenAITelemetryPlugin` to the configuration file.

   **File:** devproxyrc.json

   ```json
   {
     "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
     "plugins": [
       {
         "name": "OpenAITelemetryPlugin",
         "enabled": true,
         "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
       }
     ],
     "urlsToWatch": [
     ],
     "logLevel": "information",
     "newVersionNotification": "stable",
     "showSkipMessages": true
   }
   ```

1. Configure the `urlsToWatch` property to include the URLs of the OpenAI-compatible requests that you want to intercept. The following example intercepts requests to Azure OpenAI chat completions.

   **File:** devproxyrc.json (with Azure OpenAI URLs)

   ```json
   {
     "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
     "plugins": [
       {
         "name": "OpenAITelemetryPlugin",
         "enabled": true,
         "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll"
       }
     ],
     "urlsToWatch": [
       "https://*.openai.azure.com/openai/deployments/*/chat/completions*",
       "https://*.cognitiveservices.azure.com/openai/deployments/*/chat/completions*"
     ],
     "logLevel": "information",
     "newVersionNotification": "stable",
     "showSkipMessages": true
   }
   ```

1. Save your changes.

### Start OpenTelemetry collector and Dev Proxy

> [!IMPORTANT]
> Both .NET Aspire and OpenLIT require Docker to run. If you don't have Docker installed, follow the instructions in the [Docker documentation](https://docs.docker.com/get-docker/) to install Docker.

1. Start Docker.

1. Start the OpenTelemetry collector.

    # [.NET Aspire](#tab/aspire)
    
    1. Run the following command to start the .NET Aspire OpenTelemetry collector and dashboard:
    
        ```bash
        docker run --rm -it -p 18888:18888 -p 4317:18889 -p 4318:18890 --name aspire-dashboard mcr.microsoft.com/dotnet/aspire-dashboard:latest
        ```
        
        > [!NOTE]
        > When you're finished using the .NET Aspire dashboard, stop the dashboard by pressing <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal where you started the dashboard. Docker automatically removes the container when you stop it.
    
    1. Open the .NET Aspire dashboard in your browser at `http://localhost:18888/login?t=<code>`.
    
    # [OpenLIT](#tab/openlit)
    
    1. To run OpenLIT, run the following command:
    
        ```bash
        # Clone the OpenLIT repository
        git clone git@github.com:openlit/openlit.git
        # Start the OpenLIT OpenTelemetry collector and dashboard
        docker compose up -d
        ```
    
        > [!IMPORTANT]
        > When you're finished using the OpenLIT dashboard, stop the dashboard by running the following command:
        >
        > ```bash
        > docker compose down
        > ```
    
    1. Open the OpenLIT dashboard in your browser at `http://127.0.0.1:3000`. Use the default credentials to sign in:
      - Email: `user@openlit.io`
      - Password: `openlituser`
    
    ---

1. To start Dev Proxy, change the working directory to the folder where you created the Dev Proxy configuration file and run the following command:

   ```bash
   devproxy
   ```

### Use language model and inspect telemetry data

1. Make a request to the OpenAI-compatible endpoint that you configured Dev Proxy to intercept.

1. Verify that Dev Proxy intercepted the request and response. In the console, where Dev Proxy is running, you should see similar information:

    ```text
     info    Dev Proxy API listening on http://127.0.0.1:8897...
     info    Dev Proxy listening on 127.0.0.1:8000...
    
    Hotkeys: issue (w)eb request, (r)ecord, (s)top recording, (c)lear screen
    Press CTRL+C to stop Dev Proxy
    
    
     req   ╭ POST https://some-resource.cognitiveservices.azure.com/openai/deployments/some-deployment/chat/completions?api-version=2025-01-01-preview
     time  │ 19/05/2025 07:53:38 +00:00
     pass  │ Passed through
     proc  ╰ OpenAITelemetryPlugin: OpenTelemetry information emitted
    ```

1. In the web browser, navigate to the OpenTelemetry dashboard.

    # [.NET Aspire](#tab/aspire)
    
    1. From the side menu, select **Traces**.
    1. Select one of the traces named `DevProxy.OpenAI`.
    1. Select the request span.
    1. In the side panel, explore the language model usage data.
    
        :::image type="content" source="../media/openai-telemetry-aspire-trace-basic.png" alt-text="Screenshot of the .NET Aspire dashboard showing OpenAI telemetry data in a span." lightbox="../media/openai-telemetry-aspire-trace-basic.png":::
    
    1. In the side panel, switch to **Metrics**.
    1. From the **Resource** drop-down list, select **DevProxy.OpenAI**.
    1. From the list of metrics, select **gen_ai.client.token.usage** to see a chart showing the number of tokens that your application uses.
    
        :::image type="content" source="../media/openai-telemetry-aspire-metrics-usage.png" alt-text="Screenshot of the .NET Aspire dashboard showing a chart of token usage." lightbox="../media/openai-telemetry-aspire-metrics-usage.png":::
    
    # [OpenLIT](#tab/openlit)
    
    1. On the dashboard, notice the intercepted request and response and the high-level overview.
    
        :::image type="content" source="../media/openai-telemetry-openlit-dashboard-basic.png" alt-text="Screenshot of the OpenLIT dashboard showing basic token usage information." lightbox="../media/openai-telemetry-openlit-dashboard-basic.png":::
    
    1. In the side menu, switch to **Requests**.
    1. To view the details, select the request.
    
        :::image type="content" source="../media/openai-telemetry-openlit-requests-basic.png" alt-text="Screenshot of the OpenLIT dashboard showing information about intercepted requests." lightbox="../media/openai-telemetry-openlit-requests-basic.png":::
    
    ---

1. Stop Dev Proxy by pressing <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal where it's running.

## Understand language model costs

Dev Proxy supports estimating the costs of using language models. To allow Dev Proxy to estimate costs, you need to provide information about prices for the models you use.

### Create a prices file

1. In the same folder where you created the Dev Proxy configuration file, create a new file named `prices.json`.
1. Add the following content to the file:

   **File:** prices.json

   ```json
   {
     "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/openaitelemetryplugin.pricesfile.schema.json",
     "prices": {
       "o4-mini": {
         "input": 0.97,
         "output": 3.87
       }
     }
   }
   ```

   > [!IMPORTANT]
   > The key is the name of the language model. The `input` and `output` properties are the prices per million tokens for input and output tokens. If you use a model for which there's no price information, Dev Proxy doesn't log the cost metrics.

1. Save your changes.
1. In the code editor, open the Dev Proxy configuration file.
1. Extend the `OpenAITelemetryPlugin` reference with a configuration section:

   **File:** devproxyrc.json (add configSection to plugin)

   ```json
   {
     "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
     "plugins": [
       {
         "name": "OpenAITelemetryPlugin",
         "enabled": true,
         "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
         "configSection": "openAITelemetryPlugin"
       }
     ],
     "urlsToWatch": [
       "https://*.openai.azure.com/openai/deployments/*/chat/completions*",
       "https://*.cognitiveservices.azure.com/openai/deployments/*/chat/completions*"
     ],
     "logLevel": "information",
     "newVersionNotification": "stable",
     "showSkipMessages": true
   }
   ```

1. Add the `openAITelemetryPlugin` section to the configuration file:

   **File:** devproxyrc.json (complete with cost tracking)

   ```json
   {
     "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
     "plugins": [
       {
         "name": "OpenAITelemetryPlugin",
         "enabled": true,
         "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
         "configSection": "openAITelemetryPlugin"
       }
     ],
     "urlsToWatch": [
       "https://*.openai.azure.com/openai/deployments/*/chat/completions*",
       "https://*.cognitiveservices.azure.com/openai/deployments/*/chat/completions*"
     ],
     "openAITelemetryPlugin": {
       "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/openaitelemetryplugin.schema.json",
       "includeCosts": true,
       "pricesFile": "prices.json"
     },
     "logLevel": "information",
     "newVersionNotification": "stable",
     "showSkipMessages": true
   }
   ```

   Notice the `includeCosts` property set to `true` and the `pricesFile` property set to the name of the file with prices information.

1. Save your changes.

### View estimated costs

1. Start Dev Proxy.

1. Make a request to the OpenAI-compatible endpoint that you configured Dev Proxy to intercept.

1. In the web browser, navigate to the OpenTelemetry dashboard.

    # [.NET Aspire](#tab/aspire)
    
    1. From the side panel, select **Metrics**.
    1. From the **Resource** drop-down list, select **DevProxy.OpenAI**.
    1. From the list of metrics, select **gen_ai.client.total_cost** to see a chart showing the estimated total cost that your application incurs for using the language models.
    
        :::image type="content" source="../media/openai-telemetry-aspire-metrics-cost.png" alt-text="Screenshot of the .NET Aspire dashboard showing a chart of estimated token cost." lightbox="../media/openai-telemetry-aspire-metrics-cost.png":::
    
    # [OpenLIT](#tab/openlit)
    
    On the dashboard, notice the intercepted request and response and the high-level overview.
    
    :::image type="content" source="../media/openai-telemetry-openlit-dashboard-cost.png" alt-text="Screenshot of the OpenLIT dashboard showing token cost information." lightbox="../media/openai-telemetry-openlit-dashboard-cost.png":::
    
    ---

1. Stop Dev Proxy by pressing <kbd>Ctrl</kbd> + <kbd>C</kbd> in the terminal where it's running.

1. Stop the OpenTelemetry collector.

    # [.NET Aspire](#tab/aspire)
    
    In the terminal where the .NET Aspire dashboard is running, press <kbd>Ctrl</kbd> + <kbd>C</kbd> to stop the dashboard. Docker automatically removes the container when you stop it.
    
    # [OpenLIT](#tab/openlit)
    
    In the terminal where you started OpenLIT, run the following command to stop the dashboard:
    
      ```bash
      docker compose down
      ```
    
    ---

## Next steps

Learn more about the OpenAITelemetryPlugin.

> [!div class="nextstepaction"]
> [OpenAITelemetryPlugin](../technical-reference/openaitelemetryplugin.md)