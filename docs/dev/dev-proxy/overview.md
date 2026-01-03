---
title: What is Dev Proxy?
description: Dev Proxy is a command line tool that helps you simulate behaviors and errors of cloud APIs.
author: garrytrinder
ms.author: garrytrinder
ms.date: 01/03/2026
ms.topic: overview
---

<!-- INTENT: Help users understand what Dev Proxy is and whether it's right for them -->
<!-- AUDIENCE: Developers evaluating API testing tools -->

# What is Dev Proxy?

Dev Proxy is an API simulator that helps you effortlessly test your app beyond the happy path.

You test your app to make sure it works as intended. But what if the APIs you use fail? Will your app lose your customer's data? How do you test for this? Simulating API failures is hard. You end up writing code that you won't be shipping or worse: not testing at all. That's why we built Dev Proxy, to simulate API errors so that you can easily test your app without changing your code.

With Dev Proxy you:

- [See how your app responds to API errors](./how-to/test-my-app-with-random-errors.md), without changing your app's code, so that you can **build more robust apps and don't lose customers' data**.
- [Verify how your app handles API rate limits](./how-to/simulate-rate-limit-api-responses.md), so that you can avoid getting throttled and **improve the user experience for your customers**.
- [See how your app handles slow APIs](./how-to/simulate-slow-api-responses.md), so that you can implement the necessary affordances, and **make your app more user-friendly**.
- [Quickly stand-up mock APIs](./how-to/simulate-crud-api.md) without writing a line of code, so that you can **focus on building your app instead of writing code you won't be shipping**.
- Improve your app with contextual guidance on how you use APIs, to **make your app even better**.

Dev Proxy is a command-line tool that works on any platform. Because it intercepts network requests, it works with any type of app and tech stack. Dev Proxy is open source and free to use.

## Who is Dev Proxy for?

Dev Proxy helps developers who:

- **Build apps that call APIs** - Test resilience without changing your code
- **Build apps with Microsoft Graph** - Get guidance on permissions and best practices
- **Design APIs** - Prototype and mock APIs before implementation
- **Automate testing** - Integrate chaos testing into CI/CD pipelines

## When to use Dev Proxy

**Use Dev Proxy when you need to:**

- Test API resilience without modifying your application code
- Work with any tech stack (browser, Node.js, .NET, Python, etc.)
- Simulate failures for APIs you don't control
- Get guidance on Microsoft Graph best practices
- Automate chaos testing in CI/CD pipelines

**Consider other approaches when:**

- You only need in-browser mocking for frontend unit tests
- You're building the API and need contract testing
- You need to modify request/response bodies programmatically (Dev Proxy can do this, but dedicated tools may be simpler)

## Quick start by scenario

Choose your path based on what you want to accomplish:

| What do you want to do? | Time | Guide |
|-------------------------|------|-------|
| Test my app handles API errors | 5 min | [Test with random errors](./how-to/test-my-app-with-random-errors.md) |
| Mock an API that doesn't exist yet | 10 min | [Simulate a CRUD API](./how-to/simulate-crud-api.md) |
| Check my Microsoft Graph permissions | 10 min | [Detect minimal permissions](./how-to/detect-minimal-microsoft-graph-api-permissions.md) |
| Understand what APIs my app calls | 5 min | [Discover URLs to watch](./how-to/discover-urls-watch.md) |
| Automate API testing in CI/CD | 15 min | [Use Dev Proxy in CI/CD](./how-to/use-dev-proxy-in-ci-cd-overview.md) |

How does your app handle API errors?

> [!div class="nextstepaction"]
> [Get started](./get-started/set-up.md)

## See also

- [Set up Dev Proxy](./get-started/set-up.md) - Installation and first run
- [Configure Dev Proxy](./get-started/configure.md) - Customize to your needs
- [How-to guides](./how-to/overview.md) - Task-oriented guides
- [Technical reference](./technical-reference/overview.md) - Plugin documentation
