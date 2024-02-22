---
title: What is Dev Proxy?
description: Dev Proxy is a command line tool that helps you simulate behaviors and errors of cloud APIs.
author: garrytrinder
ms.author: garrytrinder
ms.date: 02/22/2024
ms.topic: overview
---

# What is Dev Proxy?

Dev Proxy is an API simulator that helps you effortlessly test your app beyond the happy path.

You test your app to make sure it works as intended. But what if the APIs you use fail? Will your app lose your customer's data? How do you test for this? Simulating API failures is hard. You end up writing code that you won't be shipping or worse: not testing at all. That's why we built Dev Proxy, to simulate API errors so that you can easily test your app without changing your code.

With Dev Proxy you:

- [See how your app responds to API errors](./how-to/test-my-app-with-random-errors.md), without changing your app’s code, so that you can **build more robust apps and don't lose customers' data**.
- [Verify how your app handles API rate limits](./how-to/simulate-rate-limit-api-responses.md), so that you can avoid getting throttled and **improve the user experience for your customers**.
- [See how your app handles slow APIs](./how-to/simulate-slow-api-responses.md), so that you can implement the necessary affordances, and **make your app more user-friendly**.
- [Quickly stand-up mock APIs](./how-to/simulate-crud-api.md) without writing a line of code, so that you can **focus on building your app instead of writing code you won't be shipping**.
- Improve your app with contextual guidance on how you use APIs, to **make your app even better**.

Dev Proxy is a command-line tool that works on any platform. Because it intercepts network requests, it works with any type of app and tech stack. Dev Proxy is open source and free to use.

How does your app handle API errors?

> [!div class="nextstepaction"]
> [Get started](./get-started.md)
