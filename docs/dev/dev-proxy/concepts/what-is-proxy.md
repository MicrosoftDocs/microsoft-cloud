---
title: What is a proxy?
description: This article explains what a proxy is and how it works.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 03/18/2025
---

# What is a proxy?

A *proxy* is an intermediary server that sits between a client (such as your application) and a destination server (such as a back-end API). When your application sends a request, the proxy receives it first. The proxy can then forward the request to the target server, modify it, block it, or return a response directly.

In short, a proxy acts on behalf of either the client or the server to mediate communication.

## How a proxy works

Proxies operate at the HTTP level (or other application protocols) by receiving incoming requests and taking one or more of the following actions:

- **Forwarding** the request to the target server and then relaying the response back to the client.
- **Modifying** headers, URLs, or payloads before forwarding.
- **Intercepting** and responding to the request locally without contacting the target server.
- **Rejecting** the request based on rules or access policies.

From the perspective of the client, it's simply sending a request to a URL. The proxy handles everything else behind the scenes. The pattern is client to proxy to target server.

This pattern introduces a layer of control and abstraction that you can use to improve security, observability, performance, and testability.

## Types of proxies

There are different types of proxies. Each one is suited to specific roles in system architecture.

### Forward proxy

A forward proxy sits in front of the *client*. When your application makes a request, it goes through the proxy, which decides whether and how to forward it. Forward proxies are commonly used to:

- Control access to external resources.
- Anonymize client traffic.
- Log outgoing traffic for monitoring.
- Apply content filtering or transformation.

#### Reverse proxy

A reverse proxy sits in front of the *server*. Clients are unaware of the underlying back-end infrastructure. The reverse proxy receives incoming requests and forwards them to one of several back-end servers. Reverse proxies are commonly used to:

- Load balance traffic across multiple services.
- Serve cached responses to reduce back-end load.
- Terminate TLS/SSL connections.
- Hide internal service details from the public internet.

### Transparent proxy

A transparent proxy intercepts traffic without the client being explicitly configured to use it. This type is used in corporate or internet service provider environments to enforce policies or monitor usage.

## Why proxies matter to application developers

Often infrastructure or network teams manage proxies. However, proxies directly affect application behavior, especially in development and testing environments. Here are some practical ways they affect your day-to-day work.

### Debugging and observability

Proxies can capture and inspect HTTP traffic. Tools like Dev Proxy, Fiddler, Proxyman, Charles Proxy, or mitmproxy act as local forward proxies. You can run your application through them to analyze requests and responses, spot errors, and verify headers or authentication tokens.

### API gateway and routing

In many production systems, traffic to your application's back end is routed through an API gateway or reverse proxy, such as NGINX, or a cloud-native service like Azure API Management. These proxies handle routing, authentication, rate limiting, and more.

When you design your API or building distributed services, you must understand how proxies affect headers (such as `X-Forwarded-For`), time-outs, and request size limits.

### CORS and local development

During local development, especially in web applications, you might encounter cross-origin resource sharing (CORS) restrictions when you call APIs from the browser. A development proxy can forward your requests to the target API while it rewrites headers to bypass CORS limitations. Common examples of developer tools that rewrite CORS requests are `vite`, `webpack-dev-server`, or custom proxy middleware in frameworks like Express or ASP.NET Core.

### Service virtualization and testing

Proxies can simulate back-end APIs. This capability is useful when the real service is unavailable, unstable, or expensive to use during testing. By intercepting and mocking responses, you can test application behavior under different scenarios such as time-outs, errors, or malformed data.

Tools like Dev Proxy or custom proxy implementations are commonly used for this purpose in integration and end-to-end tests.

### Authentication and security

Proxies are often the front line of defense in securing applications. They can enforce access controls, inject authentication headers, or terminate TLS/SSL connections. As a developer, it's important to be aware of how your application behaves when it sits behind a proxy and how to access headers that carry authentication or identity information.

## Common headers and proxy considerations

When a request passes through a proxy, certain headers are added or modified to preserve important metadata. For example:

- `X-Forwarded-For`: Indicates the original IP address of the client.
- `X-Forwarded-Proto`: Indicates the original protocol (HTTP or HTTPS).
- `X-Forwarded-Host`: Indicates the original host requested by the client.

When your application runs behind a reverse proxy, make sure that your framework or platform is configured to trust and interpret these headers correctly.

## Dev Proxy as a forward proxy for development and testing

Dev Proxy is a forward proxy that you can use to intercept and modify requests from your application to any target server. With Dev Proxy, you can:

- See how your app responds to API errors.
- Verify how your app handles API rate limits.
- See how your app handles slow APIs.
- Quickly stand up mock APIs without writing a line of code.
- Improve your app with contextual guidance on how you use APIs.

## Next step

> [!div class="nextstepaction"]
> [Get started](../get-started/set-up.md)
