---
title: Glossary
description: Definitions of terms used in Dev Proxy documentation
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/03/2026
ms.topic: concept
---

<!-- INTENT: Define terminology consistently for users and LLMs -->

# Dev Proxy glossary

This glossary defines terms used throughout the Dev Proxy documentation.

## A

### API simulator

A tool that mimics the behavior of an API without connecting to the real API server. Dev Proxy is an API simulator that can mock responses, simulate errors, and inject latency.

## C

### Chaos testing

A testing methodology that deliberately introduces failures into a system to verify that it can handle unexpected conditions. Dev Proxy enables chaos testing by simulating random API errors and failures.

### Configuration file

The JSON file (`devproxyrc.json`) that defines Dev Proxy's behavior, including which URLs to watch, which plugins to enable, and plugin-specific settings. See [Configure Dev Proxy](../get-started/configure.md).

## E

### Error simulation

The practice of making API calls fail on purpose to test how an application handles failures. Dev Proxy simulates errors by returning HTTP error responses instead of forwarding requests to the real API.

## I

### Intercept

When Dev Proxy captures an HTTP/HTTPS request before it reaches its destination. Intercepted requests can be passed through to the API, modified, or replaced with mock responses.

## L

### Latency

The time delay between sending a request and receiving a response. Dev Proxy can inject artificial latency to simulate slow network conditions or overloaded APIs.

## M

### Mock response

A predefined response that Dev Proxy returns instead of forwarding a request to the real API. Mocks are useful for testing against APIs that don't exist yet or when you want predictable responses.

### Mocks file

A JSON file (often `mocks.json`) containing mock response definitions. Each entry maps a URL pattern to a response body, status code, and headers.

## P

### Pass through

When Dev Proxy forwards a request to the real API without modification. This happens when a request doesn't match any active plugins or when plugins decide not to act on it.

### Plugin

A modular component that extends Dev Proxy's functionality. Plugins can intercept requests, generate reports, or provide guidance. See [Plugin architecture](../technical-reference/plugin-architecture.md).

### Preset

A pre-built configuration file for common scenarios. Presets combine plugins and settings to accomplish specific tasks. See [Use preset configurations](../how-to/use-preset-configurations.md).

### Proxy

A server that sits between a client application and an API server, relaying requests and responses. Dev Proxy runs as a local proxy on your machine.

## R

### Rate limiting

A technique APIs use to restrict the number of requests a client can make in a time period. Dev Proxy can simulate rate limiting by returning 429 (Too Many Requests) responses. Compare with [throttling](#throttling).

### Recording

The process of capturing API requests and responses for later analysis. Dev Proxy can record traffic and export it to various formats.

### Reporter

A plugin that converts recorded data into human-readable formats like Markdown, JSON, or plain text. Reporters process the output of reporting plugins.

### Reporting plugin

A plugin that analyzes recorded requests and generates reports about API usage, permissions, or best practices.

## S

### Schema

A JSON Schema file that defines the structure and validation rules for Dev Proxy configuration files. Schemas enable IntelliSense in editors that support them.

### Shadow API

An API endpoint that an application uses but isn't formally documented or known to the organization. Dev Proxy can help discover shadow APIs.

## T

### Throttling

When an API intentionally slows down or rejects requests because of high load or to protect resources. Often used interchangeably with [rate limiting](#rate-limiting), though throttling typically implies a temporary condition while rate limiting is a fixed policy.

## U

### URLs to watch

The URL patterns that Dev Proxy monitors for requests. Only requests matching these patterns are intercepted. Patterns support wildcards, for example `https://api.contoso.com/*`.

## W

### Watch

To monitor requests to specific URLs. Dev Proxy watches URLs you configure and can intercept matching requests.

## See also

- [What is Dev Proxy?](../overview.md)
- [Configure Dev Proxy](../get-started/configure.md)
- [Technical reference](../technical-reference/overview.md)
