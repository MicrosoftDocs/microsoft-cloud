---
title: Microsoft Copilots Across the Microsoft Cloud
description: Learn how you can adopt, extend, and build Microsoft Copilots.
author: DanWahlin
ms.author: donasa
ms.contributors: dwahlin-5182022
ms.date: 11/03/2023
ms.topic: conceptual
ms.service: microsoft-cloud-for-developers

categories:
  - developer-tools
products:
  - azure
  - power-platform
  - github
  - m365
ms.custom:
  - fcp
  - team=cloud_advocates
---

# Microsoft Copilots Across the Microsoft Cloud

Microsoft Copilots are AI assistants powered by Large Language Models (LLMs) that offer innovative solutions across the Microsoft Cloud. They aim to enhance productivity, creativity, and data accessibility while providing enterprise-grade data security and privacy features. They bridge the gap between complex tasks and user-friendly solutions, providing a conversational interface to interact with data, create automations, build applications, and even assist in coding tasks. Their integration within various Microsoft and GitHub platforms provides a more interactive and efficient digital workspace. 

The core value of Copilots stems from their capability to interpret natural language commands, enhancing the user experience by making it more intuitive and user-friendly. From answering queries in Copilot Pro, assisting with data navigation and analysis in Microsoft 365, aiding in app and automation creation in Power Apps and Power Automate, to suggesting code in GitHub Copilot, they encapsulate the notion of having an AI-powered assistant at your fingertips. Copilots make complex tasks more manageable, fostering a collaborative environment for both individual users and enterprises. 

:::image type="content" source="media/copilots-across-microsoft-cloud.png" alt-text="A diagram showing the adopt, extend, and build capabilities of Copilots across the Microsoft Cloud." border="false" :::

| | Adopt a Copilot | Extend a Copilot | Build your Own Copilot |
|---|---|---|---|
| Development Effort | Low effort | Medium Effort | Large to XL effort (depends on your data) |
| Data Source | In Microsoft systems such as your Microsoft tenant | In Microsoft systems plus 3rd party data sources | Spans multiple data stores, services and apps inside and outside your Microsoft tenant |
| Users of the Copilot | Users in your Microsoft tenant | Users in your Microsoft tenant | Users in your Microsoft tenant or external customers |
| Security Factors | Secure by Default | We need to do work //call security people | We need to do work //call security people |
| Customization (Temperature, Max_Tokens, Metaprompts) | Low | Medium | High |
| Scenario | Copilot for Microsoft 365: "Show me all the upcoming trips on my calendar" | Copilot in Microsoft Teams: "Book a flight to Seattle" through the Flight API | Custom Employee Travel Copilot using Azure OpenAI Service or Microsoft Copilot Studio |


## Adopt a Copilot

Microsoft offers a variety of Copilots that can be adopted to enhance user productivity and creativity. These Copilots are integrated into various Microsoft platforms and products, providing a more interactive and efficient digital workspace.

:::image type="content" source="media/copilots-across-microsoft-cloud-adopt.png" alt-text="A diagram showing the adopt options for Copilots across the Microsoft Cloud." border="false" :::

### Copilots in Dynamics 365

Copilots in Dynamics 365 provide AI assistance to boost the productivity and efficiency of sales, support, supply chain management, finance, marketing and other teams involved with your business operations. It provides a chat interface for quick summaries of sales opportunities and leads, updates, meeting preparations, account-related news, and much more. Users can seamlessly integrate Copilot with Outlook and Teams for better data management and utilize its generative AI capabilities for record summarization and email content generation. 

Learn More:
- [Copilots in Dynamics 365 Overview](/dev/copilots/copilots-in-dynamics365)

### Copilots for Microsoft 365

Copilots for Microsoft 365 combines Large Language Models (LLMs) with Microsoft Graph, linking your organization's data with Microsoft 365 apps to create a new way of working. Users can easily find and navigate business data, analyze trends, and create insightful reports using simple natural language commands. This is a major upgrade for productivity, as it's integrated across the Microsoft 365 ecosystem, making tasks quicker and easier to complete. Plus, with built-in enterprise-grade security, privacy, and responsible AI, it ensures a safe, secure, and compliant digital workspace.  

In addition, Copilot functionality is also available directly in Microsoft 365 apps. The integrated Copilot functionality provides users with a conversational interface to jumpstart their work and offer suggestions and insights to enhance productivity and creativity.

Learn more:
- [Microsoft 365 Copilot Documentation](/microsoft-365-copilot)

### Copilot in Power BI

Microsoft Fabric provides a new way to transform and analyze data, generate insights, and create visualizations and reports. 

Copilot in Power BI infuses the power of Large Language Models (LLMs) into Power BI at every layer to help users get more done and create more value from their data. Users can create and tailor reports in seconds, generate and edit DAX calculations, create narrative summaries, and ask questions about their data, all by using natural conversational language. It enables users to quickly create reports with just a few clicks saving hours of effort building report pages.

Copilot in Power BI provides the following features:
- Creates a summary of the dataset. 
- Outlines suggested pages for your report: for example, what each page in the report will be about, and how many pages it will create. 
- Creates visuals for the individual pages. 
- Creates a summary of the current page. 

Learn more:
- [Introducing Microsoft Fabric and Copilot in Microsoft Power BI](https://powerbi.microsoft.com/blog/introducing-microsoft-fabric-and-copilot-in-microsoft-power-bi/)
- [Copilot in Power BI Demo (video)](https://www.youtube.com/watch?v=wr__6tM5U6I)

### Copilots in Dynamics 365

Copilots in Dynamics 365 provide AI assistance to boost the productivity and efficiency of sales, support, supply chain management, finance, marketing and other teams involved with your business operations. It provides a chat interface for quick summaries of sales opportunities and leads, updates, meeting preparations, account-related news, and much more. Users can seamlessly integrate Copilot with Outlook and Teams for better data management and utilize its generative AI capabilities for record summarization and email content generation.

Learn more:
- [Copilots in Dynamics 365 Overview](/dev/copilots/copilots-in-dynamics365)

### Copilot in Power Apps

The AI Copilot in Power Apps enables app creators to build applications more efficiently than ever. It enables the construction of an app, along with its data, by articulating the requirements through a conversational, natural language approach over several steps. This feature ensures a Copilot-driven experience from the initial screen, offering users a more conversational and less click-intensive interaction as they build apps. 

Learn more:
- [Copilot in Power Apps Overview](/power-apps/maker/canvas-apps/ai-overview)
- [Build a canvas app for a real estate solution with Copilot in Power Apps](/training/modules/build-canvas-app-real-estate-power-apps-copilot/)

### Copilot in Power Automate 
 
Copilot in Power Automate simplifies automation creation using natural language expressions. Users describe what they need through conversation, and Copilot assists by understanding intent, setting up connections, applying necessary parameters, and making requested changes to the flow. It can also be used to answer queries about the flow that a user is creating.

Learn more:
- [Get started with Copilot in Cloud Flows](/power-automate/get-started-with-copilot)
- [Build flows for a real estate solution using Copilot in Power Automate](/training/modules/build-real-estate-power-automate-copilot/)

### GitHub Copilot 
 
GitHub Copilot is an AI developer tool that assists in the coding process by suggesting code as the user types. It serves as a programming aid, helping to streamline coding tasks and explore coding solutions efficiently. Integrated within the GitHub platform, it provides a supportive environment for developers to tackle programming challenges and enhance their coding workflow. Additionally, GitHub Copilot can be used to learn new programming languages or frameworks by providing real-time code suggestions based on developer input. 

Learn more:
- [GitHub Copilot Overview](https://github.com/features/copilot)

### Microsoft Copilot

Microsoft Copilot is an AI-powered chat assistant designed to aid users in web browsing and much more. It can answer both simple and complex queries, assist with research, and provide summaries of various content such as articles, books, or events. It can also offer product comparisons, find comprehensive answers, provide inspiration, generate images, and much more. 

Access Microsoft Copilot by visiting [bing.com](https://bing.com) and clicking on "Chat" at the top of the page. Microsoft Edge users can access the chat feature by clicking on the "Copilot" icon in the top right corner of the browser.

Learn more:
- [Copilot in Microsoft Edge](https://www.microsoft.com/edge/features/bing-chat)
- [bing.com](https://bing.com)

### Microsoft Copilot Pro
 
Copilot Pro utilizes Large Language Models (LLMs) and AI to improve data access and boost user productivity and creativity in the enterprise workspace. It provides a secure AI-powered chat with commercial data protection, ensuring user and business data safety. The system doesn't save chat data and restricts data access, including from Microsoft, ensuring privacy. User data isn't used for model training, maintaining data integrity. Copilot Pro offers accurate, verifiable, and visual answers from web data, aligning with Microsoft's responsible AI principles, making it a secure and efficient tool for modern enterprises. 
 
Learn more:
- [Copilot Pro Documentation](/bing-chat-enterprise)

### Microsoft Security Copilot

Microsoft Security Copilot is an AI-powered security analysis tool that empowers defenders to move at the speed and scale of AI, combining advanced large language models with a security-specific model from Microsoft. It integrates insights and data from security tools and delivers guidance that’s tailored to your organization. Security Copilot accepts natural language inputs and includes a pinboard section for co-workers to collaborate and share information. It also surfaces prioritized threats in real-time while anticipating a threat actor’s next move with continuous reasoning based on Microsoft’s global threat intelligence.

Learn more:
- [Microsoft Security Copilot Overview](/security-copilot/microsoft-security-copilot)

### Windows Copilot 
 
Windows Copilot is an AI-powered intelligent assistant designed to enhance user efficiency and creativity. It aids in retrieving answers and inspirations from the web, supporting creative tasks, and aiding focus on the current task. Users can adjust PC settings, organize windows, and initiate creative projects with Copilot's assistance. It's easily accessible, ready with a keystroke, and extends support both online and within Windows apps, making it a user-friendly tool for various tasks. 
 
Learn more:
- [AI with Copilot in Windows](https://www.microsoft.com/windows/copilot-ai-features)

## Extending Copilots

Microsoft Copilots offer a comprehensive range of features right out of the box, yet many users continue to depend on external tools, services, and data to carry out tasks. To bridge this gap, Copilots can be extended using plugins, connectors, or message extensions.

:::image type="content" source="media/copilots-across-microsoft-cloud-extend.png" alt-text="A diagram showing extend options for Copilots across the Microsoft Cloud." border="false" :::

### Benefits of Extending Microsoft Copilots

By extending Microsoft Copilots, developers can integrate external data directly into Copilots to minimize user context shifts while enhancing productivity and collaboration. Copilots can also be extended to provide a new way for developers to surface external data directly in Microsoft products that users use everyday. For instance, enterprises and ISVs can build plugins to integrate their own line-of-business APIs and data directly into a Copilot.

Adding plugins, connectors, or message extensions to Microsoft Copilots helps users make the most out of these AI assistants. Users can more easily discover and work with enterprise data, tools, and services using natural language, which improves productivity and enhances teamwork.

#### Extending Copilots with Plugins

Plugins allow users to interact directly with APIs and services enhancing Copilot's capabilities and broadening its range of functionality by pulling in external data. For example, plugins can enable a Copilot to retrieve real-time data, such as the latest news coverage on a product launch, or knowledge-based information such as a team's project items.

One example of plugins is Power Platform Connectors which offer a set of prebuilt actions and triggers for developers to build their own apps and workflows. They can be used to extend Copilot in Microsoft 365 and Copilots in Power Platform experiences with data from external apps.

:::image type="content" source="media/power-platform-connector.png" alt-text="A diagram showing the key tasks that a Power Platform connector performs." border="false" :::

Developers can also create plugins that use OpenAI schemas to add custom functionality to Copilot experiences by connecting their own application data to Microsoft Copilot. Plugins enable a Copilot experience to interact with your own APIs, enhancing the experience with the ability to perform a wide range of actions.

Learn more:
- [Power Platform Connectors Overview](/connectors/connectors)
- [Create a Custom Power Platform Connector from Scratch](/connectors/custom-connectors/define-blank)
- [Overview of Plugins for Microsoft Copilot](https://review.learn.microsoft.com/copilot-plugins/overview?branch=pr-en-us-1)

#### Extending Copilots with Microsoft Graph Connectors

Microsoft Graph connectors import external content into Microsoft 365. Once the content is imported, all of Microsoft 365 including search, context IQ, microsoft365.com, and Microsoft 365 Copilot can access the content. Microsoft Graph connectors are responsible for three key tasks:

1. Creating an external connection to your data sources
1. Defining a schema for the external content
1. Managing external content that’s imported into Microsoft 365

:::image type="content" source="media/graph-connector.png" alt-text="A diagram showing the key tasks that a Microsoft Graph connector performs." border="false" :::

Learn more:
- [Build your First Custom Microsoft Graph Connector](/graph/connecting-external-content-build-quickstart)
- [Publish your Custom Microsoft Graph Connector](/graph/custom-connector-sdk-sample-publish)

#### Extending Copilots with Message Extensions

Message extensions allow users to interact with enterprise services through Microsoft Teams chats. They can search or initiate actions in an external service and post that information, in the form of adaptive cards, into a Teams message. By using message extensions, you can extend the built-in functionality of Copilot for Microsoft 365.

Learn more:
- [Message Extensions Overview](/microsoftteams/platform/messaging-extensions/what-are-messaging-extensions)
- [Build Message Extensions using Bot Framework](/microsoftteams/platform/messaging-extensions/build-bot-based-message-extension)

## Building Copilots

TBD

:::image type="content" source="media/copilots-across-microsoft-cloud-build.png" alt-text="A diagram showing the build options for creating custom Copilots." border="false" :::

### Microsoft Copilot Studio



Learn more:
- [Microsoft Copilot Studio Overview](/power-virtual-agents/fundamentals-what-is-power-virtual-agents)
- [Create a Low-Code Bot](/training/modules/create-bots-power-virtual-agents-copilot/)