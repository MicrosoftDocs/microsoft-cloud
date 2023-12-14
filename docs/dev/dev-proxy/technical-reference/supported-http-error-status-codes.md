---
title: Supported HTTP error status codes
description: List of supported HTTP error status codes
author: garrytrinder
ms.author: garrytrinder
ms.date: 11/03/2023
ms.topic: reference
ms.service: microsoft-cloud-for-developers

categories:
  - developer-tools
products:
  - microsoft-365
  - microsoft-graph
  - sharepoint-online
  - m365
ms.custom:
  - fcp
  - team=cloud_advocates
  - tool=devproxy
---

# Supported HTTP error status codes

The following HTTP status codes can be used to simulate errors using the proxy.

> [!TIP]
> Descriptions from [HTTP response status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

Code | Name | Description |
--|--|--|
429 | Too Many Requests | Indicates the user has sent too many requests in a given amount of time ("rate limiting"). A [Retry-After](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After) header might be included to this response indicating how long to wait before making a new request. |
500 | Internal Server Error | Indicates that the server encountered an unexpected condition that prevented it from fulfilling the request. This error response is a generic "catch-all" response. Usually, this indicates the server can't find a better 5xx error code to response. |
502 | Bad Gateway | Indicates that the server, while acting as a gateway or proxy, received an invalid response from the upstream server. |
503 | Service Unavailable | Indicates that the server isn't ready to handle the request. Common causes are a server that is down for maintenance or that is overloaded. This response should be used for temporary conditions and the [Retry-After](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Retry-After) HTTP header should, if possible, contain the estimated time for the recovery of the service.
504 | Gateway Timeout | Indicates that the server, while acting as a gateway or proxy, didn't get a response in time from the upstream server that it needed in order to complete the request. |
507 | Insufficient Storage | Might be given in the context of the Web Distributed Authoring and Versioning (WebDAV) protocol (see [RFC 4918](https://datatracker.ietf.org/doc/html/rfc4918)). It indicates that a method couldn't be performed because the server can't store the representation needed to successfully complete the request.
