---
title: Technical reference
description: Technical reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 10/28/2025
ms.topic: overview
---

# Technical reference for Dev Proxy

This section contains technical reference for Dev Proxy plugins and configuration options.

## Plugins

List of plugins that work with any API.

Name | Description
--- | ---
[AuthPlugin](./authplugin.md)|Simulates authentication and authorization using API keys or OAuth2.
[DevToolsPlugin](./devtoolsplugin.md)|Exposes Dev Proxy messages, and information about intercepted requests and responses in Chrome DevTools.
[CachingGuidancePlugin](./cachingguidanceplugin.md)|Shows a warning when Dev Proxy intercepted the same request within the specified period of time.
[CrudApiPlugin](./crudapiplugin.md)|Simulates a CRUD API with an in-memory data store.
[ExecutionSummaryPlugin](./executionsummaryplugin.md)|Generates a summary report of the requests that pass through the proxy.
[GenericRandomErrorPlugin](./genericrandomerrorplugin.md)|Fails requests with a random selected error from file containing mocked errors.
[HarGeneratorPlugin](./hargeneratorplugin.md)|Generates HTTP Archive (HAR) files from the intercepted requests and responses.
[HttpFileGeneratorPlugin](./httpfilegeneratorplugin.md)|Generates HTTP file from the intercepted requests and responses.
[LanguageModelFailurePlugin](./languagemodelfailureplugin.md)|Simulates various language model failure scenarios to test resilience of language model-dependent applications.
[LanguageModelRateLimitingPlugin](./languagemodelratelimitingplugin.md)|Simulates token-based rate limiting for language model APIs by tracking prompt and completion token consumption within configurable time windows.
[LatencyPlugin](./latencyplugin.md)|Delays responses by a random number of milliseconds from the configured range.
[MinimalCsomPermissionsPlugin](./minimalcsompermissionsplugin.md)|Detects the minimal permissions needed to call the recorded SharePoint Client-Side Object Model (CSOM) API requests.
[MinimalPermissionsPlugin](./minimalpermissionsplugin.md)|Checks if the app uses minimal permissions to call APIs. Uses API information from the specified local folder.
[MinimalPermissionsGuidancePlugin](./minimalpermissionsguidanceplugin.md)|Compares the permissions used in the JWT token sent to APIs against the minimum required scopes needed for requests that proxy recorded and shows the difference.
[MockGeneratorPlugin](./mockgeneratorplugin.md)|Generates Dev Proxy mocks based on the intercepted requests.
[MockRequestPlugin](./mockrequestplugin.md)|Allows you to issue web requests using Dev Proxy.
[MockResponsePlugin](./mockresponseplugin.md)|Simulates responses.
[ODataPagingGuidancePlugin](./odatapagingguidanceplugin.md)|Shows a warning when proxy intercepts an OData paging request using a URL that hasn't been previously returned in one of the intercepted responses.
[OpenAIMockResponsePlugin](./openaimockresponseplugin.md)|Simulates responses from Azure OpenAI and OpenAI using a local language model.
[OpenAITelemetryPlugin](./openaitelemetryplugin.md)|Generates telemetry data for OpenAI-compatible API requests.
[OpenApiSpecGeneratorPlugin](./openapispecgeneratorplugin.md)|Generates OpenAPI spec from the intercepted requests and responses.
[RateLimitingPlugin](./ratelimitingplugin.md)|Simulates rate-limit behaviors.
[RetryAfterPlugin](./retryafterplugin.md)|Simulates the `Retry-After` header sent by an API after throttling a request.
[RewritePlugin](./rewriteplugin.md)|Rewrites requests.
[TypeSpecGeneratorPlugin](./typespecgeneratorplugin.md)|Generates TypeSpec files from the intercepted requests and responses.
[UrlDiscoveryPlugin](./urldiscoveryplugin.md)|Creates a list of URLs that the proxy has intercepted.

## Azure API Center plugins

List of plugins that work with Azure API Center.

Name | Description
--- | ---
[ApiCenterMinimalPermissionsPlugin](./apicenterminimalpermissionsplugin.md)|Checks if the app uses minimal permissions to call APIs. Uses API information from the specified Azure API Center instance.
[ApiCenterOnboardingPlugin](./apicenteronboardingplugin.md)|Checks if the APIs used in an app are registered in the specified Azure API Center instance.
[ApiCenterProductionVersionPlugin](./apicenterproductionversionplugin.md)|Checks if the APIs used in an app are production version of the APIs registered in the specified Azure API Center instance.

## Microsoft Entra plugins

List of plugins that work with Microsoft Entra API.

Name | Description
--- | ---
[EntraMockResponsePlugin](./entramockresponseplugin.md)|Mocks responses to Microsoft Entra.

## Microsoft Graph plugins

List of plugins that work with Microsoft Graph API.

Name | Description
--- | ---
[GraphBetaSupportGuidancePlugin](./graphbetasupportguidanceplugin.md)|Shows a warning when proxy detects a request to Microsoft Graph beta endpoint.
[GraphClientRequestIdGuidancePlugin](./graphclientrequestidguidanceplugin.md)|Shows a tip when a request to Microsoft Graph API doesn't include the `client-request-id` header.
[GraphConnectorGuidancePlugin](./graphconnectorguidanceplugin.md)|Shows contextual guidance for working with Microsoft Graph connectors.
[GraphMinimalPermissionsPlugin](./graphminimalpermissionsplugin.md)|Returns a list of the minimal permissions required for Microsoft Graph requests that proxy recorded.
[GraphMinimalPermissionsGuidancePlugin](./graphminimalpermissionsguidanceplugin.md)|Compares the permissions used in the JWT token sent to Microsoft Graph against the minimum required scopes needed for requests that proxy recorded and shows the difference.
[GraphMockResponsePlugin](./graphmockresponseplugin.md)|Mocks responses to Microsoft Graph APIs.
[GraphRandomErrorPlugin](./graphrandomerrorplugin.md)|Fails requests made to Microsoft Graph with random errors.
[GraphSdkGuidancePlugin](./graphsdkguidanceplugin.md)|Shows a tip when proxy intercepts a request to Microsoft Graph that hasn't been issued by a Microsoft Graph SDK.
[GraphSelectGuidancePlugin](./graphselectguidanceplugin.md)|Shows a warning when proxy intercepts a request to Microsoft Graph APIs that doesn't include the `$select` query string parameter.
[ODSPSearchGuidancePlugin](./odspsearchguidanceplugin.md)|Shows a warning when Dev Proxy detects a request to OneDrive and SharePoint search APIs.

## Reporters

List of reporters that generate reports in different formats.

Name | Description
--- | ---
[JsonReporter](./jsonreporter.md)|Generates reports in JSON format.
[MarkdownReporter](./markdownreporter.md)|Generates reports in Markdown format.
[PlainTextReporter](./plaintextreporter.md)|Generates reports in plain-text format.

## Configuration

Reference of Dev Proxy configuration options.

Name | Description
--- | ---
[devproxyrc](./devproxyrc.md)|Configuration file for Dev Proxy.
[Proxy API](./proxy-API.md)|API for interacting with Dev Proxy programmatically.
[Proxy settings](./proxy-settings.md)|Configuration options for proxy settings.
