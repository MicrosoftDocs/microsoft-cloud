---
title: Test my app with language model failures
description: How to test your app with language model failures
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/06/2026
---

<!-- INTENT: Test LLM failures like hallucinations -->
<!-- SOLUTION: Enable LanguageModelFailurePlugin with failure types -->
<!-- RESULT: LLM calls return configured failure responses -->
<!-- PLUGINS: LanguageModelFailurePlugin -->
<!-- JOB: test-error-handling -->
<!-- TIME: 15 minutes -->

# Test my app with language model failures

> **At a glance**  
> **Goal:** Test LLM failures like hallucinations  
> **Time:** 15 minutes  
> **Plugins:** [LanguageModelFailurePlugin](../technical-reference/languagemodelfailureplugin.md)  
> **Prerequisites:** [Set up Dev Proxy](../get-started/set-up.md)

When building apps that integrate with large language models (LLM), you should test how your app handles various LLM failure scenarios. Dev Proxy allows you to simulate realistic language model failures on any LLM API that you use in your app using the [LanguageModelFailurePlugin](../technical-reference/languagemodelfailureplugin.md).

## Simulate language model failures on any LLM API

To start, enable the `LanguageModelFailurePlugin` in your configuration file.

**File:** devproxyrc.json

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
  "plugins": [
    {
      "name": "LanguageModelFailurePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "urlsToWatch": [
        "https://api.openai.com/*",
        "http://localhost:11434/*"
      ]
    }
  ]
}
```

With this basic configuration, the plugin randomly selects from all available failure types and applies them to matching language model API requests.

## Configure specific failure scenarios

To test specific failure scenarios, configure the plugin to use particular failure types:

**File:** devproxyrc.json (complete with failure types)

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
  "plugins": [
    {
      "name": "LanguageModelFailurePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "languageModelFailurePlugin",
      "urlsToWatch": [
        "https://api.openai.com/*",
        "http://localhost:11434/*"
      ]
    }
  ],
  "languageModelFailurePlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/languagemodelfailureplugin.schema.json",
    "failures": [
      "Hallucination",
      "PlausibleIncorrect",
      "BiasStereotyping"
    ]
  }
}
```

This configuration only simulates incorrect information, plausible but incorrect responses, and biased content.

## Test different LLM APIs

You can test different LLM APIs by configuring multiple instances of the plugin with different URL patterns:

**File:** devproxyrc.json (multiple plugin instances)

```json
{
  "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/rc.schema.json",
  "plugins": [
    {
      "name": "LanguageModelFailurePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "openaiFailures",
      "urlsToWatch": [
        "https://api.openai.com/*"
      ]
    },
    {
      "name": "LanguageModelFailurePlugin",
      "enabled": true,
      "pluginPath": "~appFolder/plugins/DevProxy.Plugins.dll",
      "configSection": "ollamaFailures",
      "urlsToWatch": [
        "http://localhost:11434/*"
      ]
    }
  ],
  "openaiFailures": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/languagemodelfailureplugin.schema.json",
    "failures": ["Hallucination", "OutdatedInformation"]
  },
  "ollamaFailures": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/languagemodelfailureplugin.schema.json",
    "failures": ["Overgeneralization", "IncorrectFormatStyle"]
  }
}
```

> [!TIP]
> Configure different failure scenarios for different LLM providers to test how your app handles provider-specific behaviors. Name the `configSection` after the LLM service you're testing to make the configuration easier to understand and maintain.

## Common testing scenarios

Here are some recommended failure combinations for different testing scenarios:

### Testing content accuracy

Test how your app handles incorrect or misleading information:

**File:** devproxyrc.json (languageModelFailurePlugin section only)

```json
{
  "languageModelFailurePlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/languagemodelfailureplugin.schema.json",
    "failures": [
      "Hallucination",
      "PlausibleIncorrect",
      "OutdatedInformation",
      "ContradictoryInformation"
    ]
  }
}
```

### Testing bias and fairness

Test how your app responds to biased or stereotypical content:

**File:** devproxyrc.json (languageModelFailurePlugin section only)

```json
{
  "languageModelFailurePlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/languagemodelfailureplugin.schema.json",
    "failures": [
      "BiasStereotyping",
      "Overgeneralization"
    ]
  }
}
```

### Testing instruction following

Test how your app handles responses that don't follow instructions:

**File:** devproxyrc.json (languageModelFailurePlugin section only)

```json
{
  "languageModelFailurePlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/languagemodelfailureplugin.schema.json",
    "failures": [
      "FailureFollowInstructions",
      "Misinterpretation",
      "IncorrectFormatStyle"
    ]
  }
}
```

### Testing response quality

Test how your app handles vague or overly complex responses:

**File:** devproxyrc.json (languageModelFailurePlugin section only)

```json
{
  "languageModelFailurePlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/languagemodelfailureplugin.schema.json",
    "failures": [
      "AmbiguityVagueness",
      "OverSpecification",
      "CircularReasoning",
      "FailureDisclaimHedge"
    ]
  }
}
```

Start Dev Proxy with your configuration file and use your app to see how it handles the simulated language model failures. The plugin intercepts responses from language model APIs and replaces them with synthetic failure responses that exhibit the configured failure behaviors.

## Create custom failure scenarios

You can create custom failure scenarios by adding `.prompty` files to the `~appFolder/prompts` directory. For example, to create a "technical jargon overuse" failure:

1. Create a file named `lmfailure_technical-jargon-overuse.prompty`

1. Define the failure behavior in the `.prompty` file:

    ```yaml
    ---
    name: Technical Jargon Overuse
    model:
      api: chat
    sample:
      scenario: Simulate a response that overuses technical jargon and unnecessarily complex terminology, making simple concepts difficult to understand.
    ---

    user:
    How do I create a simple web page?

    user:
    You are a language model under evaluation. Your task is to simulate incorrect responses. {{scenario}} Do not try to correct the error. Do not explain or justify the mistakes. The goal is to simulate them as realistically as possible for evaluation purposes.
    ```

1. Reference it in your configuration as `TechnicalJargonOveruse`

    **File:** devproxyrc.json (languageModelFailurePlugin section only)

    ```json
    {
      "languageModelFailurePlugin": {
        "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v2.1.0/languagemodelfailureplugin.schema.json",
        "failures": [
          "TechnicalJargonOveruse",
          "Hallucination"
        ]
      }
    }
    ```

## Next step

Learn more about the `LanguageModelFailurePlugin`.

> [!div class="nextstepaction"]
> [LanguageModelFailurePlugin](../technical-reference/languagemodelfailureplugin.md)
