---
title: How to Debug Microsoft Graph API Calls
description: This article shares some recommendations for debugging Microsoft Graph API calls.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/26/2024
---

# How to debug Microsoft Graph API calls

You might build an application that uses Microsoft Graph APIs to connect to Microsoft 365 and discover that the API isn't working as you expected. How do you debug it? This article shares some recommendations for debugging Microsoft Graph API calls.

## Check for known issues

Before you start debugging, check if the issue that you're facing is a known issue with the Microsoft Graph API. For more information, see the [Microsoft Graph known issues website](https://developer.microsoft.com/graph/known-issues/?search=).

## Check the changelog

If you're using the [Microsoft Graph beta endpoint](./use-microsoft-graph-beta-production.md), check the [Microsoft Graph changelog](https://developer.microsoft.com/graph/changelog/?search=) for any changes that might affect your application. The changelog contains information about new features, changes, and fixes in the Microsoft Graph API. It also contains information about breaking changes that might affect your application.

## Check the documentation

When the API isn't working as expected, check its documentation. It might contain information that can help you understand the issue that you're facing. For example, the documentation might contain information about known limitations, specific requirements, or other considerations.

## Check the throttling limits

Check the [throttling limits](/graph/throttling-limits) for the API that you're using. If you exceed the [throttling](./what-is-throttling.md) limits, the API might return an HTTP `429 (Too Many Requests)` status code. If you get throttled, [handle it properly](./how-to-handle-api-throttling.md) in your application.

## Handle expected errors

Microsoft Graph uses standard HTTP status codes to communicate its errors. For more information about status codes that Microsoft Graph APIs returns, see [Microsoft Graph error responses and resource types](/graph/errors).

## Generate and log a unique client-request-id

On every request to Microsoft Graph, generate a unique GUID, send it in the `client-request-id` HTTP request header, and log it in your application's logs. This information helps you to track and correlate requests and responses when you debug issues. It also helps Microsoft Support to diagnose the issue if you need to report it.

## Log the request ID and date from the HTTP response headers

Always log the request ID and date from the HTTP response headers. Together with the `client-request-id` header, they're required when you report issues in Microsoft Q&A or to Microsoft Support. Without these headers, diagnosing the issue is difficult.

Use Dev Proxy to see if your application adds the `client-request-id` HTTP request header to the requests that it sends to Microsoft Graph.

# Next step

> [!div class="nextstepaction"]
> [Check if my app uses the client-request-id header](../technical-reference/graphclientrequestidguidanceplugin.md)
