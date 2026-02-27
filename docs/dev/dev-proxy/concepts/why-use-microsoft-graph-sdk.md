---
title: Why use Microsoft Graph SDK
description: Learn why Dev Proxy recommends using Microsoft Graph SDKs instead of raw HTTP calls
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/26/2026
---

<!-- INTENT: Explain why Dev Proxy recommends using Microsoft Graph SDKs -->

# Why use a Microsoft Graph SDK

When Dev Proxy detects that your application makes Microsoft Graph API requests without using an SDK, it suggests migrating to an SDK. This guidance exists because Microsoft Graph SDKs significantly simplify working with Microsoft Graph APIs and help you build more robust applications.

## Benefits of Microsoft Graph SDKs

Microsoft Graph SDKs provide numerous advantages over making raw HTTP calls.

### Built-in authentication handling

SDKs integrate with authentication libraries like Microsoft Authentication Library (MSAL), handling token acquisition, refresh, and management automatically. You don't need to implement the OAuth 2.0 flow manually or worry about token expiration.

### Automatic retry logic for throttling

Microsoft 365 services implement [throttling](./what-is-throttling.md) to ensure service reliability. When your application receives a `429 Too Many Requests` response, the SDK automatically handles retries with appropriate backoff delays, respecting the `Retry-After` header that Microsoft Graph returns.

### Proper pagination handling

Many Microsoft Graph endpoints return paginated results. SDKs provide built-in iterators that transparently handle fetching subsequent pages, so you don't need to manually track `@odata.nextLink` values and make other requests.

### Type safety and IntelliSense

SDKs provide strongly-typed models for requests and responses, giving you IntelliSense support in your IDE. Type models reduce errors and speed up development by catching issues at compile time rather than runtime.

### Batching support

SDKs support [batching multiple requests](/graph/sdks/batch-requests) into a single HTTP call, reducing network overhead and improving performance. Batching also helps you stay within throttling limits by reducing the number of individual requests.

### Delta queries

SDKs simplify implementing [delta queries](/graph/delta-query-overview) that let you efficiently sync data changes since your last request. Using delta queries is crucial for building responsive applications that need to stay synchronized with Microsoft 365 data.

### Middleware architecture for customization

SDKs use a customizable middleware pipeline to add logging, modify headers, implement custom retry policies, or inject other behaviors without changing your application code.

### Consistent API patterns

SDKs provide consistent patterns across different platforms and languages, making it easier to share knowledge between team members and across projects.

## What raw HTTP calls miss

When you make raw HTTP calls to Microsoft Graph, you need to implement several critical features yourself.

### Manual retry logic implementation

Without an SDK, you must implement retry logic for handling `429 responses`, including:

- Parsing the `Retry-After` header values
- Implementing exponential backoff
- Handling different error scenarios
- Avoiding retry storms under heavy load

### Manual pagination handling

You need to:

- Check every response for `@odata.nextLink`
- Make more requests for each page
- Aggregate results across pages
- Handle partial failures during pagination

### Error handling boilerplate

Microsoft Graph can return various error responses with different structures. Without an SDK, you need to:

- Parse error response bodies
- Handle different HTTP status codes appropriately
- Extract meaningful error messages for logging
- Implement fallback behavior for unexpected errors

### Authentication complexity

Managing authentication manually requires:

- Implementing OAuth 2.0 flows
- Storing and securing tokens
- Handling token refresh before expiration
- Managing different authentication scenarios (delegated vs. application permissions)

## Available SDKs

Microsoft provides official SDKs for the following platforms:

| Platform | SDK | Documentation |
|----------|-----|---------------|
| .NET | Microsoft.Graph | [.NET SDK documentation](/graph/sdks/sdks-overview#microsoft-graph-net-sdk) |
| JavaScript/TypeScript | @microsoft/microsoft-graph-client | [JavaScript SDK documentation](/graph/sdks/sdks-overview#microsoft-graph-javascript-sdk) |
| Java | microsoft-graph | [Java SDK documentation](/graph/sdks/sdks-overview#microsoft-graph-java-sdk) |
| Python | msgraph-sdk-python | [Python SDK documentation](/graph/sdks/sdks-overview#microsoft-graph-python-sdk) |
| Go | msgraph-sdk-go | [Go SDK documentation](/graph/sdks/sdks-overview#microsoft-graph-go-sdk) |
| PHP | microsoft-graph | [PHP SDK documentation](/graph/sdks/sdks-overview#microsoft-graph-php-sdk) |
| PowerShell | Microsoft.Graph | [PowerShell SDK documentation](/graph/sdks/sdks-overview#microsoft-graph-powershell-sdk) |

## Alternatives to using an SDK

While SDKs are recommended for most scenarios, there are cases where other approaches might be a better fit:

### Kiota-generated client

If your application only uses a small subset of Microsoft Graph APIs, consider [generating a client with Kiota](/graph/sdks/generate-with-kiota) instead of using the full SDK. Kiota generates a lightweight, strongly-typed client tailored to the specific APIs you need, reducing your dependency footprint while still giving you type safety and request building.

### Minimal API usage

If you're writing a quick script or tool that makes only a few API calls, the overhead of setting up an SDK might not be justified. In these cases, direct HTTP calls can be simpler. However, you still need to handle [authentication](#authentication-complexity) and [error responses](#error-handling-boilerplate) yourself, which can quickly add complexity even for simple scripts.

### Unsupported language

If you're using a programming language that doesn't have an official SDK, you need to make direct HTTP calls. You might also consider using Kiota to [generate a client](/graph/sdks/generate-with-kiota) for your language if Kiota supports it.

### When the SDK doesn't support a specific feature yet

Microsoft Graph continuously adds new capabilities. Occasionally, a new API or feature might not have a fluent API in the SDK immediately. In most cases, you can still use the SDK to make arbitrary requests using a [path string](/graph/sdks/create-requests), which gives you the benefits of the SDK's middleware (authentication, retries, logging) without needing typed request builders. If the SDK doesn't support the request at all, you can fall back to direct HTTP calls until SDK support is added.

### Performance-sensitive environments

In scenarios like serverless functions or edge computing where cold start time and package size matter, a lighter approach using Kiota-generated clients or direct HTTP calls might be more appropriate.

## Next steps

Learn how to update your application to use a Microsoft Graph SDK:

> [!div class="nextstepaction"]
> [Update your application to use Microsoft Graph JavaScript SDK](../how-to/update-my-application-code-to-use-Microsoft-Graph-JavaScript-SDK.md)

## Related content

- [Microsoft Graph SDK overview](/graph/sdks/sdks-overview)
- [Choose a Microsoft Graph authentication provider](/graph/sdks/choose-authentication-providers)
- [Install a Microsoft Graph SDK](/graph/sdks/sdk-installation)
- [Best practices for working with Microsoft Graph](/graph/best-practices-concept)
