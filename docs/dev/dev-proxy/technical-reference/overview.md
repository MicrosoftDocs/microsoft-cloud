---
title: Technical reference
description: Technical reference
author: garrytrinder
ms.author: garrytrinder
ms.date: 02/14/2024
ms.topic: overview
---

# Technical reference for Dev Proxy

This section contains technical reference for Dev Proxy plugins and configuration options.

## Plugins

List of plugins that work with any API.

Name | Description
--- | ---
[DevToolsPlugin](./devtoolsplugin.md)|Exposes Dev Proxy messages, and information about intercepted requests and responses in Chrome DevTools.
[CachingGuidancePlugin](./cachingguidanceplugin.md)|Shows a warning when Dev Proxy intercepted the same request within the specified period of time.
[CrudApiPlugin](./crudapiplugin.md)|Simulates a CRUD API with an in-memory data store.
[ExecutionSummaryPlugin](./executionsummaryplugin.md)|Generates a summary report of the requests that pass through the proxy.
[GenericRandomErrorPlugin](./genericrandomerrorplugin.md)|Fails requests with a random selected error from file containing mocked errors.
[LatencyPlugin](./latencyplugin.md)|Delays responses by a random number of milliseconds from the configured range.
[MockGeneratorPlugin](./mockgeneratorplugin.md)|Generates Dev Proxy mocks based on the intercepted requests.
[MockResponsePlugin](./mockresponseplugin.md)|Simulates responses.
[ODataPagingGuidancePlugin](./odatapagingguidanceplugin.md)|Shows a warning when proxy intercepts an OData paging request using a URL that hasn't been previously returned in one of the intercepted responses.
[OpenApiSpecGeneratorPlugin](./openapispecgeneratorplugin.md)|Generates OpenAPI spec in JSON format from the intercepted requests and responses.
[RateLimitingPlugin](./ratelimitingplugin.md)|Simulates rate-limit behaviors.
[RetryAfterPlugin](./retryafterplugin.md)|Simulates the `Retry-After` header sent by an API after throttling a request.

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
[GraphMockResponsePlugin](./graphmockresponseplugin.md)|Mocks responses to Microsoft Graph APIs.
[GraphRandomErrorPlugin](./graphrandomerrorplugin.md)|Fails requests made to Microsoft Graph with random errors.
[GraphSdkGuidancePlugin](./graphsdkguidanceplugin.md)|Shows a tip when proxy intercepts a request to Microsoft Graph that hasn't been issued by a Microsoft Graph SDK.
[GraphSelectGuidancePlugin](./graphselectguidanceplugin.md)|Shows a warning when proxy intercepts a request to Microsoft Graph APIs that doesn't include the `$select` query string parameter.
[MinimalPermissionsPlugin](./minimalpermissionsplugin.md)|Returns a list of the minimal permissions required for Microsoft Graph requests that proxy recorded.
[MinimalPermissionsGuidancePlugin](./minimalpermissionsguidanceplugin.md)|Compares the permissions used in the JWT token sent to Microsoft Graph against the minimum required scopes needed for requests that proxy recorded and shows the difference.
[ODSPSearchGuidancePlugin](./odspsearchguidanceplugin.md)|Shows a warning when Dev Proxy detects a request to OneDrive and SharePoint search APIs.

## Configuration

Reference of Dev Proxy configuration options.

Name | Description
--- | ---
[devproxyrc](./devproxyrc.md)|Configuration file for Dev Proxy.
[Proxy settings](./proxy-settings.md)|Configuration options for proxy settings.
