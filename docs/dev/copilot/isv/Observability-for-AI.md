---
title: Creating solutions with the Microsoft Cloud Solution Toolkit
description: Learn how to use the Microsoft Cloud Solution Toolkit and the C# Copilot SDK to build solutions using capabilities across the Microsoft Cloud.
author: miglaros
ms.author: miglaros
ms.service: cloud-for-industries
ms.topic: overview 
ms.date: 09/16/2024

#CustomerIntent: As an ISV developing solutions on the Microsoft Cloud, I want to learn best practices for Observability so that I can monitor and observe my generative AI solution.
---

## Observability for pro-code generative AI solutions

## In this article

- [Building and operationalizing copilot applications](#building-and-operationalizing-copilot-applications)
- [Observability challenges in generative AI](#observability-challenges-in-generative-ai)
- [Metrics to monitor and evaluate](#metrics-to-monitor-and-evaluate)
- [Developer tools to get started](#developer-tools-to-get-started)
- [Next Steps](#next-steps)

## Introduction

Microsoft partners, including independent software vendors (ISVs) like yourself, can create generative AI solutions through many different pro-code and low-code approaches. To support you in this process, Microsoft is [creating guidance](Capability-Envisioning.md) for ISVs to better enable you to build these solutions.

As ISVs aim to manage specialized queries and tasks, the complexity of their generative AI solutions increases. These complex generative AI solutions require unique precautions during development and consistent monitoring and observation throughout production. By observing your product's behavior and outputs, you can quickly identify areas for growth, promptly address risks and issues, and drive even higher performance for your application.

## Building and operationalizing copilot applications

To understand how observability impacts your application from the beginning of the solution lifecycle, it's key to think of the lifecycle in three major stages: ideating on your use case, building your solution, and operationalizing it for use after deployment.

Image titled LLM Lifecycle in the real world. It's composed of three circles, connected with arrows and surrounded by a larger arrow labeled "managing". The first circle is labeled "Ideating/Exploring" and includes "try prompts", "Hypothesis", and "Find LLMs". It stems from the business need arrow and is connected to the second circle with an arrow labeled "advance project". The second circle is labeled "Building/augmenting" and is composed of "retrieval augmented generation", "exception handling", "prompt engineering or fine-tuning", and "evaluation". The second circle connects to the last circle with "prepare for app deployment". The last circle is the largest, is labeled "operationalizing", and is composed of "deploy LLM app/UI", "quota and cost management", "content filtering", "safe rollout/staging", and "monitoring". The last circle is connected to the second circle with "send feedback", while the second is connected to the first with !["revert project".](media/1_Observability%20for%20AI%20Graphic.png)

The first phase is made up of identifying your use case and ideating on technological approaches to building it. Once you identify a path to building your application, you enter the second phase, which consists of developing and evaluating the application. After the application is deployed to production, it enters the last phase, where it can be observed and updated.

Observability insights gleaned from the second and third phases become critical when returning to earlier phases to perform updates and deployments. By performing continuous testing and retrieving metrics to inform earlier processes, you are able to keep fine-tuning your application.

It's important to understand what actions you can take to practice observability at different points in the process. At a high level, the following observability actions happen during the application lifecycle:

### First phase

1. Non-technical personas such as product managers practice observability by ideating on key qualities of their application.
    - Stakeholders can define what metrics are most important to measure the performance of their application, such as risk and safety metrics, quality metrics, or executional metrics.
    - Teams can define goals for metrics such as user engagement and cost management.
2. Technical personas focus on identifying platforms, tools, and methods that can most successfully enable building the application.
    - This step could include choosing a pattern to use, choosing a large language model (LLM), and identifying key data sources to draw from.

### Second phase

1. During this stage, developers and data scientists can set their solution up to be easily monitored and iterated on. To promote observability later on, ISVs may:
    - Create golden datasets and automated multi-turn conversation datasets for copilot evaluation.
    - Debug, run, and evaluate flow with a subset of data.
    - Create variations for model evaluation with different prompts.
    - Perform pricing and resource optimization fit for LLM cost reduction to iterate and build.
2. Developers focus on performing experiments throughout development and evaluating their application's quality before deploying it to production. During this stage, it's critical to:
    - Evaluate overall application performance with test datasets using predefined metrics, including model and prompt efficacy.
    - Compare the results of these tests, establish a baseline, and then deploy the code to production.

### Third phase

1. Many experiments are conducted while the application is being actively used in production. It's important to monitor your solution to ensure it's performing adequately. During this phase:
    - Telemetry instrumentation embedded within the app emits the relevant traces, metrics, and logs. 
    - Cloud services emit Azure OpenAI health and other relevant metrics. 
    - Tailored telemetry storage holds data on traces, metrics, logs, usage, consent, and other relevant metrics.
    - Pre-canned dashboard experiences enable developers, data scientists, and administrators to monitor LLM API performance and system health in the production environment.
    - Feedback from end users is submitted back to developers and data scientists for evaluation to help improve the solution.

Collecting data and telemetry gives you insight into areas to address and improve in the future. By taking preemptive steps during the ideation phase, such as identifying appropriate metrics and performing rigorous evaluations of your solution while building your application, you are able to prepare your solution for success later on. 

## Observability challenges in generative AI

While observability in general may require ISVs to navigate many obstacles, generative AI solutions introduce specific considerations and challenges.

## Qualitative evaluation

Because generative AI prompt responses are given in natural language form, they need to be evaluated uniquely. For instance, they can be checked for qualities such as groundedness and relevance.

ISVs need to consider the best route for measuring these qualities, whether they choose manual evaluation or AI-assisted metrics with a human in the loop for final validation.

No matter how you evaluate it, you also likely need preexisting datasets to compare your prompt responses to. These preperations can mean more work during development of your application as you identify an ideal response to common prompt topics.

## Responsible AI

New considerations and expectations for ethical AI introduce a need to monitor privacy, security, inclusivity, and more. ISVs need to monitor these attributes in order to promote their end-user's safety, reduce risk, and minimize negative user experiences.

Microsoft's six principles of Responsible AI help promote safe, ethical, and trustworthy AI systems. To promote these values within your solution, evaluating your application against these standards is critical.

## Usage and cost monitoring metrics

Tokens are the main unit of measurement for generative AI applications, and all prompts and responses are tokenized so that they can be measured. Tracking the number of tokens used is essential, as it affects the cost to run your application.

## Utility metrics

Monitoring your application's user satisfaction and business impact are as critical as performance or quality metrics. Because AI interacts with customers in different ways, there are new considerations for monitoring customer engagement and retention.

Measuring the usefulness of your AI's responses can be accomplished in many different ways. For instance, prompt and response funnels track how long it takes for the interaction to result in a usable or helpful response. It's also important to consider tracking the time your user engages with the AI, the length of the conversation, and the amount of times your user accepts the provided response. In scenarios where your user can edit the response, it's essential to measure edit distance, or the extent to which they edit the response.

## Performance metrics

AI requires increasingly complex, high-performance systems that must be properly maintained to ensure your solution can quickly and efficiently process prompts and data. As generative AI creates qualitative content with a large degree of variety, it's important to have systems in place to evaluate and test your AI within different scenarios.

Because LLM interactions are more complex than a typical application, they need to be measured at multiple layers to identify issues with latency. For example, the times to tokenize your user's prompt, generate a response, and return the response to a user can be measured separately, or as a whole. Each individual component of the workflow should be evaluated to identify areas for potential issues.

Your ability to observe your solution also depends on your deployment method. ISVs usually adopt one of two deployment patterns for their copilot applications. You can either deploy and manage your applications in an environment you own or deploy applications in an environment that belongs to your customers.

To learn more about how deployment affects observability across solution types, visit the [pro-code observability guide](Choosing-a-pro-code-pattern.md).

## Metrics to monitor and evaluate

In the ISV realm of generative AI applications and machine learning models, it's important to continuously evaluate your solution and intervene promptly to curb undesirable behaviors. Monitoring metrics related to user experience or feedback, guardrails and Responsible AI, consistency of output, latency, and cost are essential in optimizing performance of your copilot applications.

## Qualitative evaluation using AI-assisted metrics

To measure qualitative information, ISVs can use AI-assisted metrics to monitor their solutions. AI-assisted metrics utilize LLMs like GPT-4 to evaluate metrics similarly to human judgment, which provides you with more nuanced input on your solution's capabilities.

These metrics typically require parameters such as the question, answer, and any surrounding context from the conversation. They broadly fall into two categories:

1. **Risk and safety metrics** monitor for high-risk content such as violence, self-harm, sexual content, and hateful content
2. **Generation quality metrics** track qualitative measurements such as:
    - **Groundedness**-how well the model's response aligns with information from the prompt or input source.
    - **Relevance**-how well the model's response relates to the original prompt.
    - **Coherence**-the extent to which the model's response is understandable and human-like.
    - **Fluency**-the linguistics, grammar, and syntax of the model's response.

Metrics like these allow ISVs to more easily evaluate the quality of their application's responses. They provide a quick and measurable evaluation of many different values that can be difficult to interpret.

### Responsible AI Standards

Microsoft is committed to upholding standards for Responsible AI. To support this, we established a set of [Responsible AI standards](https://www.microsoft.com/ai/responsible-ai?ef_id=_k_6fa9434ea2651a73e5b225fd1409313b_k_&OCID=AIDcmm1o1fzy5i_SEM__k_6fa9434ea2651a73e5b225fd1409313b_k_&msclkid=6fa9434ea2651a73e5b225fd1409313b) that can help you mitigate risks associated with generative AI:

- Accountability
- Transparency
- Fairness
- Inclusivity
- Reliability and safety
- Privacy and security

ISVs can monitor metrics that notify them when an issue arises. These notifications could include qualitative AI-assisted metrics that screen responses or prompts for harmful content, or alert ISVs to certain errors or flagged messages.

For example, Azure OpenAI offers solutions that can measure the percentage of filtered prompts and responses that didn't return content due to content filtering. ISVs should monitor for prompts that return these errors and aim to reduce the amount they occur.

### Customer usage and satisfaction

Some features of AI can be monitored similarly to other types of applications, such as monitoring customer retention and time spent using the application. However, there are many differences in monitoring customer satisfaction that apply specifically to AI:

- **User's reaction to the response.** This can be measured through metrics as simple as whether a user reacts to a response with a thumb up or down.
- **User's changes to the response.** In scenarios where your user can modify your AI's response to fit their needs, insight can be gained by monitoring how much your user modifies the response. For instance, a drafted email that the user changed drastically was likely not as helpful as a drafted email that the user sent as-is.
- **User's utilization of the response.** Consider monitoring whether your user takes an action through your application in response to the AI. If an AI suggests taking an action through your application, measure the rate of users who accept the suggestion.

The goal of many AI applications is to create a response that your user finds helpful. Using a prompt and response funnel is a common way to measure how quickly your solution can generate a useful response. This funnel measures the amount of time and interactions it takes for your solution to create a response the user keeps or ends the conversation with.

In this concept, the funnel begins when a user submits a prompt. As the AI generates responses that the user can interact with, the funnel narrows as the responses get closer to what the user wants. For instance, the user may edit the AI response or ask for a slightly different answer. Once the user is satisfied with the interaction, they have the specific information they were looking for and the funnel ends. Measuring how many interactions it takes for your prompt to go from broad to useful and specific is helpful for determining how effective your application is to your customer.

By observing how your users engage with your solution, you can make inferences about how helpful your application is. If your users consistently utilize your LLM's outputs without taking more action, then it's likely that the response was useful to them.

### Cost monitoring

As the resources required to run a generative AI application can quickly add up, it's essential to observe them consistently.

Some areas that can impact the cost optimization of your application include:

- GPU utilization
- Storage costs and considerations
- Scaling considerations

Ensuring visibility into these metrics can help you keep costs under control, while setting up alert systems or automatic processes that are related to these metrics can also be useful for prompting immediate action.

For example, the number of prompt and completion tokens that your application uses directly affects your GPU utilization and the cost to operate your solution. Closely monitoring your token usage and setting up alerts if it crosses certain thresholds can help you stay aware of your application's behavior.

### Solution availability and performance

As with all solutions, consistent monitoring of AI applications can help drive a high level of performance. One major difference between generative AI applications and others is the concept of tokenization, which needs to be considered when measuring performance.

ISVs building generative AI solutions can measure:

- The time to render the first token
- The tokens rendered per second
- The requests per second that your application can manage

While all these metrics can be measured as a group, it's also important to note that LLMs have multiple layers. For example, the time it takes for your AI to generate a response is composed of the time it takes to:

1. Receive the prompt from the user
2. Process the prompt through tokenization
3. Infer any relevant missing information
4. Generate a response
5. Compile this information into a response through detokenization
6. Send this response back to the user

Measuring at each of these steps can help you identify delays and where they're occurring, allowing you to address the problem at its source.

## Other generative AI evaluation techniques

### Golden Datasets

A Golden Dataset is a collection of expert answers to realistic user questions that are used to provide copilot quality assurance. These answers aren't used to train your model, but they can be compared to the answers your model gives to the same user question.

While it isn't a metric you can measure, having a high-quality standardized response you can compare your LLM's responses to helps your solution outputs. In this way, [creating your own Golden Datasets](https://github.com/microsoft/promptflow-resource-hub/blob/main/sample_gallery/golden_dataset/copilot-golden-dataset-creation-guidance.md) for evaluating copilot performance helps accelerate your copilot evaluation process.

### Multi-turn conversation simulation

Manually curating evaluation datasets can be primarily limited to single-turn conversations because of the difficulty creating natural-sounding multi-turn chats. Instead of writing scripted interactions to compare your model's answers to, ISVs can develop simulated conversations to test their copilot's multi-turn conversation abilities.

This simulation could generate dialogue by allowing your AI to interact with a simple virtual user. This user would then interact with your AI through a pre-generated script of prompts or it would generate them through AI, enabling you to create a large number of test conversations to evaluate. You could also employ human evaluators to interact with the application and generate longer conversations to review.

By evaluating your application's interactions within a longer conversation, you can evaluate how effectively it identifies your user's intent and uses context throughout the conversation. As many generative AI solutions are intended to build on multiple user interactions, it's essential to evaluate how your application handles multi-turn conversations.

## Developer tools to get started

ISV developers and data scientists need to use tools and metrics to evaluate their LLM solutions. Microsoft has many options available for you to explore.

### Azure AI Studio

[Azure AI Studio](/azure/ai-studio/what-is-ai-studio?tabs=home) provides observability features for model management, model benchmarks, tracing, evaluation, and fine-tuning your LLM solution.

It supports [two types of automated metrics](https://github.com/MicrosoftDocs/azure-docs/blob/main/articles/ai-studio/concepts/evaluation-metrics-built-in.md) to evaluate generative AI applications: traditional ML metrics and AI-assisted metrics. You can also use the [chat playground](/azure/ai-studio/quickstarts/get-started-playground) and related features easily test your model.

### Prompt flow

[Prompt flow](https://microsoft.github.io/promptflow/) is a suite of development tools designed to streamline the end-to-end development cycle of LLM-based AI applications, from prototyping and testing, to deployment and monitoring. The [prompt flow SDK](https://github.com/MicrosoftDocs/azure-docs/blob/main/articles/ai-studio/how-to/develop/flow-evaluate-sdk.md) provides:

- [Built-in evaluators](https://github.com/microsoft/promptflow/blob/main/src/promptflow-evals/samples/built_in_evaluators.py) that support custom code-based or prompt-based evaluators via [Prompty](https://microsoft.github.io/prompty/#whatIsPrompty) to cater to task-specific evaluation needs.
- [Prompt tracing](https://microsoft.github.io/promptflow/how-to-guides/tracing/index.html) which tracks the inputs, outputs and context of prompts, and enables developers to identify the causes and origins of model issues
- [Monitoring dashboards](/azure/ai-studio/how-to/develop/trace-production-sdk) including system (for example, token usage, latency) and custom metrics from evaluation to support pre- and post-deployment observability in [Azure AI Studio](https://build.microsoft.com/sessions/de3dfc8d-3aaf-4dc2-97c3-5b2937961d54?source=/speakers/1ba2b218-1cde-4724-902d-9d951536366c) and [Application Insights](https://build.microsoft.com/sessions/b27da931-0604-4072-84a1-3cdc84c537cc).

### Other tools

[Prompty](https://github.com/microsoft/prompty) is a language-agnostic asset class for creating and managing LLM prompts. It enables you to speed up the development process by providing options to design, test, and enhance your solutions.

[PyRIT (Python Risk Identification Tool for Generative AI)](https://www.microsoft.com/security/blog/2024/02/22/announcing-microsofts-open-automation-framework-to-red-team-generative-ai-systems/) is Microsoft's open automation framework for red-teaming Generative AI systems. It enables you to assess the robustness of your copilots against different harm categories.

## Next steps

By designing your generative AI application with observability and monitoring in mind, you can evaluate its quality from development through production. Get started with the tools available to begin developing your application or explore options for monitoring a solution that is already in production.

## Additional Resources

[How to Evaluate LLMs: A Complete Metric Framework - Microsoft Research](https://www.microsoft.com/research/group/experimentation-platform-exp/articles/how-to-evaluate-llms-a-complete-metric-framework/)

Further guidance on evaluating your LLM application

[Get started in prompt flow - Azure Machine Learning | Microsoft Learn](/azure/machine-learning/prompt-flow/get-started-prompt-flow?view=azureml-api-2)

Information on setting up and beginning to work with prompt flow
