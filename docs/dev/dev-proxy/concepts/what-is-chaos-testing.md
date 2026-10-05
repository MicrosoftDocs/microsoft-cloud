---
title: What Is Chaos Testing?
description: What chaos testing is, how to apply it to the APIs your app depends on, and how to inject API errors, throttling, and latency into your app without changing its code.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 10/03/2026
---

<!-- INTENT: Understand chaos testing/engineering for app resilience -->

# What is chaos testing?

Chaos testing is a technique that's used to test the resilience of software systems by introducing unexpected failures or disruptions. Chaos testing is also known as chaos engineering. The goal of chaos testing is to identify weaknesses and improve your app's resilience.

Chaos testing is based on the idea that *systems fail in unexpected ways*. Traditional testing methods often fall short in uncovering these unexpected failure modes. When you use chaos testing, you simulate real-world scenarios, such as server crashes, network latency, or resource exhaustion. Simulating these behaviors helps to expose hidden issues and weaknesses that might not be evident under normal testing conditions.

Here are some key points to keep in mind about chaos testing:

- **Be proactive.** Instead of waiting for failures to happen, chaos testing proactively introduces failures to see how the system responds. Chaos testing allows you to identify and fix issues before they become major problems.
- **Gain insights.** The goal of chaos testing is to learn from failures. By introducing them, you can gain valuable insights into how the system behaves under stress and use that information to improve it.
- **Promote a team effort.** Chaos testing is most effective when you do it collaboratively. You want input from developers, testers, operations, and other stakeholders. By working together, you can identify the most important areas to test and ensure that everyone is informed.
- **Start small and build up.** When you first start with chaos testing, it's a good idea to start small and gradually increase the complexity of your tests. Starting small helps you build confidence and develop a better understanding of how the system behaves under different conditions.

In summary, chaos testing is a powerful technique that can help you improve the resilience of your apps. By proactively introducing failures and learning from them, you can identify and fix issues before they become major problems.

## Chaos testing for the APIs your app calls

Chaos testing often focuses on infrastructure: servers, networks, and containers. If your app depends on APIs, some of the failures your users notice come from those APIs: a `500` from a payment provider, a [`429`](http-429-too-many-requests.md) from GitHub or OpenAI, or a response that takes 8 seconds instead of 80 milliseconds. You can apply the same chaos testing ideas to those dependencies, at the level of individual API responses:

- **Errors.** Return [`5xx` errors](http-5xx-api-errors.md) at random, and check that your app retries what's safe to retry and shows a useful message for the rest.
- **Throttling.** Return `429` with a `Retry-After` header, and check that your app waits before it calls again.
- **Latency.** Delay responses, and check your timeouts, spinners, and what happens when responses arrive out of order.

| Approach | What you find | What you miss |
|---|---|---|
| Wait for production | Real failures | Everything, until a user hits it |
| Mock the API in your tests, or let your coding agent write the mock | Whether your error branches run | The API's real failure formats, your SDK's retry policy, and how the running app behaves. Your app also needs a test-only switch to reach the mock. |
| Break real infrastructure (for example, kill a service or block network traffic) | How your system copes with outages | API-level failures like a specific error code or a `Retry-After` header |
| Intercept your app's real traffic and inject API failures | Errors, throttling, and latency on the real URLs, at a rate you choose | Infrastructure failures. Use infrastructure chaos tools for those. |

## Try it on your app

[Dev Proxy](../overview.md?WT.mc_id=devproxy-learn-what-is-chaos-testing) introduces API failures into your app while it keeps calling the real URLs. It works with any type of app, on any technology stack, without changing your code. For example, to fail half of the requests to an API with random errors, follow [Test my app with random errors](../how-to/test-my-app-with-random-errors.md?WT.mc_id=devproxy-learn-what-is-chaos-testing), and to make them slow, see [Simulate slow API responses](../how-to/simulate-slow-api-responses.md?WT.mc_id=devproxy-learn-what-is-chaos-testing). To run the same tests in your CI pipeline, see [Use Dev Proxy in CI/CD](../how-to/use-dev-proxy-in-ci-cd-overview.md?WT.mc_id=devproxy-learn-what-is-chaos-testing).

## Next step

> [!div class="nextstepaction"]
> [Test my app with random errors](../how-to/test-my-app-with-random-errors.md?WT.mc_id=devproxy-learn-what-is-chaos-testing)
