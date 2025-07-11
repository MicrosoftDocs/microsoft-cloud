---
title: LanguageModelFailurePlugin
description: LanguageModelFailurePlugin reference
author: waldekmastykarz
ms.author: wmastyka
ms.date: 07/11/2025
---

# LanguageModelFailurePlugin

Simulates various large language model (LLM) failure scenarios to test resilience of language model-dependent applications.

:::image type="content" source="../media/language-model-failure-plugin.png" alt-text="Screenshot of a command prompt with the Dev Proxy simulating a language model failure response for an LLM API request." lightbox="../media/language-model-failure-plugin.png":::

## Plugin instance definition

```json
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
```

## Configuration example

```json
{
  "languageModelFailurePlugin": {
    "$schema": "https://raw.githubusercontent.com/dotnet/dev-proxy/main/schemas/v1.0.0/languagemodelfailureplugin.schema.json",
    "failures": [
      "Hallucination",
      "PlausibleIncorrect"
    ]
  }
}
```

## Configuration properties

| Property | Description | Default |
|----------|-------------|:-------:|
| `failures` | Array of specific failure types to simulate. When not specified, plugin randomly selects from all available failure types. | All available failures |

## Available failure types

The plugin supports the following failure types that simulate common LLM behaviors:

| Failure Type | Description |
|--------------|-------------|
| `AmbiguityVagueness` | Provides ambiguous or vague responses |
| `BiasStereotyping` | Introduces bias or stereotyping in responses |
| `CircularReasoning` | Uses circular reasoning in explanations |
| `ContradictoryInformation` | Provides contradictory information |
| `FailureDisclaimHedge` | Uses excessive disclaimers or hedging |
| `FailureFollowInstructions` | Fails to follow specific instructions |
| `Hallucination` | Generates false or made-up information |
| `IncorrectFormatStyle` | Provides responses in incorrect format or style |
| `Misinterpretation` | Misinterprets the user's request |
| `OutdatedInformation` | Provides outdated or obsolete information |
| `OverSpecification` | Provides unnecessarily detailed responses |
| `OverconfidenceUncertainty` | Shows overconfidence about uncertain information |
| `Overgeneralization` | Makes overly broad generalizations |
| `OverreliancePriorConversation` | Over-relies on previous conversation context |
| `PlausibleIncorrect` | Provides plausible but incorrect information |

## Custom failure types

You can add custom failure types by creating `.prompty` files in the `~appFolder/prompts` directory. The file must be named `lmfailure_<failure>.prompty` where `<failure>` is written in kebab-case (for example, `my-failure`). In the plugin configuration, reference it using PascalCase (for example, `MyFailure`).

## Usage scenarios

The LanguageModelFailurePlugin is designed to help developers test their applications against various LLM failure modes:

- **Hallucination testing**: Verify your app handles false information appropriately
- **Bias detection**: Test responses to biased or stereotypical content
- **Format validation**: Ensure your app handles incorrectly formatted responses
- **Instruction following**: Test resilience when the LLM doesn't follow instructions
- **Uncertainty handling**: Verify your app manages overconfident incorrect responses

## Next step

> [!div class="nextstepaction"]
> [Test my app with language model failures](../how-to/test-my-app-with-language-model-failures.md)
