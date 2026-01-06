---
title: How-to guides
description: How-to guides for Dev Proxy
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/03/2026
ms.topic: overview
---

<!-- INTENT: Help users find the right guide for their task -->
<!-- SOLUTION: Browse categorized guides by goal -->
<!-- RESULT: Users find relevant how-to quickly -->
<!-- AUDIENCE: Developers who know what they want to accomplish -->

# How-to guides for Dev Proxy

Find guides organized by what you want to accomplish.

## Test API resilience

Simulate failures and edge cases to see how your app behaves.

- [Test my app with random errors](./Test-my-app-with-random-errors.md) · 5 min
- [Simulate slow API responses](./Simulate-slow-API-responses.md) · 5 min
- [Test that my application handles throttling properly](./test-that-my-application-handles-throttling-properly.md) · 5 min
- [Simulate Rate-Limit API responses](./Simulate-Rate-Limit-API-responses.md) · 10 min
- [Change request failure rate](./Change-request-failure-rate.md) · 2 min

### Microsoft Graph specific

- [Simulate errors from Microsoft Graph APIs](./simulate-errors-microsoft-graph-apis.md) · 5 min
- [Simulate throttling on Microsoft 365 APIs](./simulate-throttling-microsoft-365.md) · 10 min

### Language models (OpenAI, Azure OpenAI)

- [Test my app with language model failures](./test-my-app-with-language-model-failures.md) · 5 min
- [Simulate errors from OpenAI APIs](./simulate-errors-openai-apis.md) · 5 min
- [Test language model token limits](./test-language-model-token-limits.md) · 10 min

## Mock APIs

Create mock responses without building a real API.

- [Mock responses](./Mock-responses.md) · 10 min
- [Mock n-th request](./Mock-nth-request.md) · 5 min
- [Mock multiple responses to the same endpoint](./Mock-multiple-responses-to-the-same-endpoint.md) · 10 min
- [Mock responses that return binary data](./Mock-responses-that-return-binary-data.md) · 10 min
- [Change mocks file](./Change-mocks-file.md) · 2 min

### Dynamic CRUD APIs

- [Simulate a CRUD API](./simulate-crud-api.md) · 15 min
- [Simulate a CRUD API across the internet](./simulate-crud-api-dev-tunnel.md) · 20 min
- [Simulate a CRUD API secured with Microsoft Entra](./simulate-crud-api-entra.md) · 25 min
- [Simulate mock responses across the internet](./simulate-mock-responses-dev-tunnel.md) · 15 min

### Language model APIs

- [Simulate OpenAI API](./simulate-openai.md) · 15 min
- [Simulate Azure OpenAI API](./simulate-azure-openai.md) · 15 min

## Analyze API usage

Understand what APIs your app calls and how.

- [Discover URLs to watch](./discover-urls-watch.md) · 5 min
- [Record and export proxy activity](./Record-and-export-proxy-activity.md) · 10 min
- [Generate an HTTP file](./generate-http-file.md) · 5 min
- [Generate an OpenAPI spec](./generate-openapi-spec.md) · 10 min
- [Generate a TypeSpec file](./generate-typespec-file.md) · 10 min
- [Understand language model usage](./understand-language-model-usage.md) · 10 min

## Check permissions and best practices

Get guidance on using APIs correctly.

### Microsoft Graph

- [Detect minimal Microsoft Graph API permissions](./Detect-minimal-Microsoft-Graph-API-permissions.md) · 10 min
- [Check if you're using excessive Microsoft Graph API permissions](./Check-if-you-are-using-excessive-Microsoft-Graph-API-permissions.md) · 10 min
- [Update my application code to use Microsoft Graph JavaScript SDK](./Update-my-application-code-to-use-Microsoft-Graph-JavaScript-SDK.md) · 15 min

### General APIs

- [Check if my app is using production-level APIs](./check-production-level-apis.md) · 5 min
- [Check if my app is calling APIs with minimal permissions](./check-minimal-api-permissions.md) · 10 min
- [Find shadow APIs](./check-shadow-apis.md) · 10 min

## Intercept requests

Control which requests Dev Proxy intercepts.

- [Intercept requests from specific processes](./Intercept-requests-from-specific-processes.md) · 5 min
- [Intercept requests with specific headers](./intercept-requests-specific-headers.md) · 5 min
- [Intercept requests to localhost](./intercept-localhost-requests.md) · 5 min
- [Exclude a URL](./Exclude-a-URL.md) · 2 min
- [Inspect requests and responses using Chrome DevTool](./inspect-requests-responses-chrome-devtools.md) · 10 min
- [Inspect API requests issued by cloud services](./inspect-api-requests-cloud-services.md) · 15 min

## Use Dev Proxy with your stack

Platform and framework-specific guides.

- [With Node.js applications](./use-dev-proxy-with-nodejs.md) · 10 min
- [With Node.js applications in Docker containers](./use-dev-proxy-with-nodejs-docker.md) · 15 min
- [With JavaScript Azure Functions](./use-dev-proxy-with-javascript-azure-functions.md) · 15 min
- [With .NET applications](./use-dev-proxy-with-dotnet.md) · 10 min
- [With .NET applications in Docker containers](./use-dev-proxy-with-dotnet-docker.md) · 15 min
- [With .NET Azure Functions](./use-dev-proxy-with-dotnet-azure-functions.md) · 15 min
- [With .NET Aspire applications](./use-dev-proxy-with-dotnet-aspire.md) · 15 min
- [With SharePoint Framework (SPFx) solutions](./use-dev-proxy-with-spfx.md) · 10 min
- [In a Docker container](./use-dev-proxy-in-docker-container.md) · 15 min

## Automate in CI/CD

Run Dev Proxy in automated pipelines.

- [In CI/CD scenarios](./use-dev-proxy-in-ci-cd-overview.md) · 20 min

## Customize Dev Proxy

Change settings and extend functionality.

- [Use preset configurations](./Use-preset-configurations.md) · 5 min
- [Change logging level](./Change-logging-level.md) · 2 min
- [Clear the output](./Clear-the-output.md) · 1 min
- [Use local language model with Dev Proxy](./use-language-model.md) · 15 min
- [Refresh local Microsoft Graph database](./Refresh-local-Microsoft-Graph-database.md) · 5 min

## Common problems

Solutions to frequently encountered issues.

- [No requests are intercepted](./no-requests-intercepted.md)
- [No random errors are thrown when using mocks](./Why-are-random-errors-not-thrown-when-using-mocks.md)
- [No internet connection after using the proxy](./Why-is-my-internet-connection-not-working-after-using-the-proxy.md)
- [All requests fail with 429 responses](./Why-do-I-keep-getting-429-responses.md)
- [All requests fail with gateway time-out](./Why-do-all-requests-fail-with-gateway-timeout.md)
- [Binary responses aren't mocked](./Why-is-proxy-not-mocking-my-binary-response.md)
- [No requests are intercepted from my .NET 4.8 app](./why-is-proxy-not-intercepting-requests-from-my-dotnet-4-8-app.md)
- [Options aren't recognized](./Why-is-the-proxy-saying-that-an-option-is-unknown.md)
- [The type initializer for 'Microsoft.Data.Sqlite.SqliteConnection' threw an exception](./type-initializer-microsoft-data-sqlite-sqliteconnection-threw-exception.md)
- [Uninstall](./Uninstall.md)

## Get help

- [Get help and support](./Get-help-and-support.md)
