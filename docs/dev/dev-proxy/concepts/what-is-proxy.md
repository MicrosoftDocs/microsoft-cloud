---
title: What Is a Proxy?
description: What a proxy is, how forward, reverse, and transparent proxies work, and how developers use a proxy to debug API calls and test how apps handle API errors, rate limits, and slow responses.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Understand what a proxy server does and how Dev Proxy works -->

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

When you design your API or building distributed services, you must understand how proxies affect headers (such as `X-Forwarded-For`), timeouts, and request size limits.

### CORS and local development

During local development, especially in web applications, you might encounter cross-origin resource sharing (CORS) restrictions when you call APIs from the browser. A development proxy can forward your requests to the target API while it rewrites headers to bypass CORS limitations. Common examples of developer tools that rewrite CORS requests are `vite`, `webpack-dev-server`, or custom proxy middleware in frameworks like Express or ASP.NET Core.

### Service virtualization and testing

Proxies can simulate back-end APIs. This capability is useful when the real service is unavailable, unstable, or expensive to use during testing. By intercepting and mocking responses, you can test application behavior under different scenarios such as timeouts, errors, or malformed data.

Tools like Dev Proxy or custom proxy implementations are commonly used for this purpose in integration and end-to-end tests.

### Authentication and security

Proxies are often the frontline of defense in securing applications. They can enforce access controls, inject authentication headers, or terminate TLS/SSL connections. As a developer, it's important to be aware of how your application behaves when it sits behind a proxy and how to access headers that carry authentication or identity information.

## Common headers and proxy considerations

When a request passes through a proxy, certain headers are added or modified to preserve important metadata. For example:

- `X-Forwarded-For`: Indicates the original IP address of the client.
- `X-Forwarded-Proto`: Indicates the original protocol (HTTP or HTTPS).
- `X-Forwarded-Host`: Indicates the original host requested by the client.

When your application runs behind a reverse proxy, make sure that your framework or platform is configured to trust and interpret these headers correctly.

## Use a proxy to test how your app handles API failures

Because a forward proxy sees every request your app sends, it can also answer some of them with a failure instead of forwarding them: a `500`, a [`429 Too Many Requests`](http-429-too-many-requests.md) with a [`Retry-After`](retry-after-header.md) header, or a response that takes 8 seconds. Your app keeps calling the real API URLs, so you test the app as it runs in production, including its HTTP client, SDK, and retry policy.

Compared with other ways to test API failures:

| Approach | What you find | What you miss |
|---|---|---|
| Wait for production | Real failures | Everything, until a user hits it |
| Mock the API in your tests, or let your coding agent write the mock | Whether your error branches run | The API's real status codes, headers, and error bodies, and your SDK's retry policy. Your app also needs a test-only switch to reach the mock. |
| Call the real API and hope it fails | Real behavior | You can't trigger a failure on demand |
| Run your app through a proxy that simulates failures | Errors, throttling, and latency on the real URLs, on demand | Your code in isolation. Keep your unit tests for that. |

## Dev Proxy as a forward proxy for development and testing

[Dev Proxy](../overview.md?WT.mc_id=devproxy-learn-what-is-proxy) is a forward proxy that you run on your machine or in CI to intercept and modify requests from your application to the APIs you choose. With Dev Proxy, you can:

- See how your app responds to API errors.
- Verify how your app handles API rate limits and throttling.
- See how your app handles slow APIs.
- Stand up mock APIs without writing a line of code.
- Get contextual guidance on how you use APIs.

To try it on the API your app calls, download a preset and start Dev Proxy with it. For example, for GitHub:

```console
devproxy config get github-rate-limiting
devproxy --config-file "~dataFolder/configs/github-rate-limiting/.devproxy/devproxyrc.json"
```

Presets are also available for OpenAI (`openai-throttling`), Anthropic (`anthropic-throttling`), and Microsoft Graph OneDrive and SharePoint endpoints (`microsoft-graph-rate-limiting`).

## Try it yourself

Pick a scenario that matches what you're building:

- [Test how your app handles API errors](../how-to/test-my-app-with-random-errors.md?WT.mc_id=devproxy-learn-what-is-proxy) (5 minutes)
- [Simulate rate limiting on any API](../how-to/simulate-rate-limit-api-responses.md?WT.mc_id=devproxy-learn-what-is-proxy) (10 minutes)
- [Mock API responses without changing your code](../how-to/mock-responses.md?WT.mc_id=devproxy-learn-what-is-proxy) (10 minutes)
- [Use a local model instead of OpenAI while you develop](../how-to/simulate-openai.md?WT.mc_id=devproxy-learn-what-is-proxy) (15 minutes)

## Next step

> [!div class="nextstepaction"]
> [Get started](../get-started/set-up.md?WT.mc_id=devproxy-learn-what-is-proxy)
