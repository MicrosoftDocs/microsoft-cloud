---
title: What Is Chaos Testing?
description: This article explains the concept of chaos testing and how it can help you build more resilient apps.
author: waldekmastykarz
ms.author: wmastyka
ms.date: 01/26/2024
---

<!-- INTENT: Understand chaos testing/engineering for app resilience -->

# What is chaos testing?

Chaos testing is a technique that's used to test the resilience of software systems by introducing unexpected failures or disruptions. Chaos testing is also known as chaos engineering. The goal of chaos testing is to identify weaknesses and improve your app's resilience.

Chaos testing is based on the idea that *systems fail in unexpected ways*. Traditional testing methods often fall short in uncovering these unexpected failure modes. When you use chaos testing, you simulate real-world scenarios, such as server crashes, network latency, or resource exhaustion. Simulating these behaviors helps to expose hidden issues and weaknesses that might not be evident under normal testing conditions.

Here are some key points to keep in mind about chaos testing:

- **Be proactive.** Instead of waiting for failures to happen, chaos testing proactively introduces failures to see how the system responds. Chaos testing allows you to identify and fix issues before they become major problems.
- **Gain insights.** The goal of chaos testing isn't to break the system but to learn from it. By introducing failures, you can gain valuable insights into how the system behaves under stress and use that information to improve it.
- **Promote a team effort.** Chaos testing is most effective when you do it collaboratively. You want input from developers, testers, operations, and other stakeholders. By working together, you can identify the most important areas to test and ensure that everyone is informed.
- **Start small and build up.** When you first start with chaos testing, it's a good idea to start small and gradually increase the complexity of your tests. Starting small helps you build confidence and develop a better understanding of how the system behaves under different conditions.

In summary, chaos testing is a powerful technique that can help you improve the resilience of your apps. By proactively introducing failures and learning from them, you can identify and fix issues before they become major problems.

Dev Proxy makes it easy to introduce failures into your apps and test how they respond. You can use Dev Proxy to simulate API failures in any type of app, on any technology stack, without changing your code.

## Next step

> [!div class="nextstepaction"]
> [Test my app with random errors](../how-to/test-my-app-with-random-errors.md)
