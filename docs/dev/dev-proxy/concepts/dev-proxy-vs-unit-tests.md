---
title: Dev Proxy vs unit tests
description: Understand how Dev Proxy complements unit testing for comprehensive API resilience testing
author: waldekmastykarz
ms.author: wmastyka
ms.date: 02/26/2026
---

<!-- INTENT: Understand how Dev Proxy complements unit testing -->

# Dev Proxy vs unit tests

Dev Proxy and unit tests serve different purposes in your testing strategy. While both are valuable, they test your application in fundamentally different ways. Using them together gives you comprehensive coverage of your application's behavior.

## Different testing approaches

**Unit tests** test isolated pieces of code with mocked dependencies. When you write a unit test for code that calls an API, you typically mock the HTTP client to return predefined responses. This approach lets you verify that your code handles specific scenarios correctly.

**Dev Proxy** tests your running application with simulated API behavior. Instead of mocking code, Dev Proxy intercepts actual HTTP requests and simulates real-world conditions like errors, throttling, and latency. This approach tests your application as a whole, including all the layers between your code and the API.

## When to use each

Use **unit tests** when you want to:

- Test specific function behavior and edge cases
- Verify business logic in isolation
- Get fast feedback during development
- Achieve high code coverage
- Test specific error handling paths

Use **Dev Proxy** when you want to:

- Test your running application end-to-end
- Simulate conditions that are hard to reproduce, like throttling or network issues
- Test real retry logic and error handling across all layers
- Run your application under load with random failures
- Verify resilience without changing your code

## Dev Proxy advantages

Dev Proxy offers several advantages over traditional mocking approaches:

- **No code changes needed.** You don't need to modify your application to use Dev Proxy. It works at the network level, intercepting HTTP requests transparently.
- **Works with any HTTP library.** Some HTTP libraries are notoriously difficult to mock. With Dev Proxy, it doesn't matter which library you use - RestSharp, HttpClient, Axios, or any other. Dev Proxy intercepts all HTTP traffic regardless of the library.
- **Tests the real application.** Unit tests with mocked HTTP clients don't test the actual retry logic, connection handling, or timeout behavior. Dev Proxy tests your real application with all its dependencies.
- **Easier to understand failures.** When you can trigger failures on demand in a running application, it's easier to understand and debug how your application handles them.
- **Simulates complex conditions.** Dev Proxy can simulate conditions that are impractical to reproduce with mocks, such as random failures under load, gradual throttling, or intermittent network issues.

## A complementary testing strategy

The most effective approach combines both testing methods:

1. **Use unit tests for logic.** Write unit tests to verify your business logic, data transformations, and specific error handling code paths.
1. **Use Dev Proxy for resilience.** Use Dev Proxy to verify that your application handles real-world API conditions correctly—throttling, errors, and slow responses.
1. **Use both in CI/CD.** Run unit tests for fast feedback on every commit. Use Dev Proxy in integration tests to catch resilience issues before deployment.

Unit tests tell you if your code is correct. Dev Proxy tells you if your application is resilient. You need both.

## Next step

> [!div class="nextstepaction"]
> [What is chaos testing?](what-is-chaos-testing.md)
