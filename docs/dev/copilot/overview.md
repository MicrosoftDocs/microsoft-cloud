---
title: Adopt, extend and build Copilot experiences across the Microsoft Cloud
description: Learn how you can adopt, extend and build Copilot experiences.
author: DanWahlin
ms.author: donasa
ms.date: 11/15/2023
ms.topic: conceptual
ms.service: microsoft-cloud-for-developers

categories:
  - developer-tools
products:
  - azure
  - power-platform
  - github
  - m365
  - dynamics-365
ms.custom:
  - fcp
  - team=cloud_advocates
---

# Adopt, extend and build Copilot experiences across the Microsoft Cloud

Copilot is an AI assistant powered by Large Language Models (LLMs) that offer innovative solutions across the Microsoft Cloud. They aim to enhance productivity, creativity, and data accessibility while providing enterprise-grade data security and privacy features. They bridge the gap between complex tasks and user-friendly solutions, providing a natural language conversational interface to interact with data, create automations, build applications, and even assist with coding tasks. Their integration within various Microsoft platforms and products provides a more interactive and efficient digital workspace. 

The core value of Copilot stems from its ability to interpret natural language commands, enhancing the user experience by making it more intuitive and user-friendly. From answering queries in Microsoft Copilot Pro, assisting with data navigation and analysis in Microsoft 365, aiding in app and automation creation in Power Apps and Power Automate, to suggesting code in GitHub Copilot, they allow users to have an AI-powered assistant at their fingertips. Copilot makes complex tasks more manageable, fostering a collaborative environment for both individual users and enterprises. 

:::image type="content" source="media/copilot-across-microsoft-cloud.png" alt-text="A diagram showing the adopt, extend, and build capabilities of Copilot across the Microsoft Cloud." border="false" :::

Users can effortlessly adopt Copilot to streamline their workflows, while developers or creators have the flexibility to extend Copilot by integrating custom data. The core features that Copilot is built upon can also be used to build custom copilot solutions, which can be seamlessly integrated into new or existing applications. 

The following table shows differences between the adopt, extend, and build options showcasing the breadth of possibilities that Copilot offers.

|| Adopt Copilot | Extend Copilot | Build your own Copilot |
|---|---|---|---|
| **Development effort** | Low effort | Medium effort | Large to XL effort (depends on your data) |
| **Data source** | In Microsoft systems such as your Microsoft tenant | In Microsoft systems plus 3rd party data sources | Spans multiple data stores, services and apps inside and outside your Microsoft tenant |
| **Users of Copilot** | Users in your Microsoft tenant | Users in your Microsoft tenant | Users in your Microsoft tenant or external customers |
| **Customization (temperature, max_tokens, metaprompts)** | Low | Medium | High |
| **Scenario** | Copilot for Microsoft 365: "Show me all the upcoming trips on my calendar" | Copilot in Microsoft Teams: "Book a flight to Seattle" through the Flight API | Copilot for custom employee travel using Azure OpenAI Service or Microsoft Copilot Studio |

## Adopt Copilot

Microsoft offers a variety of Copilot assistants that can be adopted to enhance user productivity and creativity. Copilot is integrated into various Microsoft platforms and products, providing a more interactive and efficient digital workspace.

:::image type="content" source="media/copilot-across-microsoft-cloud-adopt.png" alt-text="A diagram showing the adopt options for a Copilot across the Microsoft Cloud." border="false" :::

Copilot assistants discussed in this section include:
- [Copilot for Dynamics 365](#copilot-for-dynamics-365)
- [Copilot for Microsoft 365](#copilot-for-microsoft-365)
- [Copilot in Microsoft Fabric](#copilot-in-microsoft-fabric)
- [Copilot in Power Apps](#copilot-in-power-apps)
- [Copilot in Power Automate](#copilot-in-power-automate)
- [Copilot in Windows](#copilot-in-windows)
- [GitHub Copilot](#github-copilot)
- [Microsoft Copilot](#microsoft-copilot)
- [Microsoft Copilot Pro](#microsoft-copilot-pro)
- [Microsoft Security Copilot](#microsoft-security-copilot)

### Copilot for Microsoft 365

Copilot for Microsoft 365 combines Large Language Models (LLMs) with Microsoft Graph, linking your organization's data with Microsoft 365 apps to create a new way of working. Users can easily find and navigate business data, analyze trends, and create insightful reports using simple natural language commands. This is a major upgrade for productivity, as it's integrated across the Microsoft 365 ecosystem, making tasks quicker and easier to complete. Plus, with built-in enterprise-grade security, privacy, and responsible AI, it ensures a safe, secure, and compliant digital workspace.  

In addition, Copilot functionality is also available directly in Microsoft 365 apps. The integrated Copilot functionality provides users with a conversational interface to jumpstart their work and offer suggestions and insights to enhance productivity and creativity.

Learn more:
- [Copilot for Microsoft 365 Documentation](/microsoft-365-copilot)

### Copilot for Dynamics 365

Copilot for Dynamics 365 provides AI assistance to boost the productivity and efficiency of sales, support, supply chain management, finance, marketing and other teams involved with your business operations. It provides a chat interface for quick summaries of sales opportunities and leads, updates, meeting preparations, account-related news, and much more. Users can seamlessly integrate Copilot with Outlook and Teams for better data management and utilize its generative AI capabilities for record summarization and email content generation. 

Learn More:
- [Copilot for Dynamics 365 Overview](/dev/copilot/copilot-for-dynamics365)

### Copilot in Microsoft Fabric

Copilot in Microsoft Fabric brings a new way to transform and analyze data, generate insights, and create visualizations and reports. 

Copilot for Power BI in Microsoft Fabric is infusing the power of large language models into Power BI at every layer to help users get more done and create additional value from their data. Users can create and tailor reports in seconds, generate and edit DAX calculations, create narrative summaries, and ask questions about their data, all in conversational language. It enables you to quickly create reports with just a few clicks. It saves you hours of effort building your report pages. Copilot provides a summary of your dataset and an outline of suggested pages for your report. 

Copilot for Power BI does the following:
- A summary of the dataset. 
- A report outlines of suggested pages for your report: for example, what each page in the report will be about, and how many pages it will create. 
- The visuals for the individual pages. 
- A summary of the current page. 

Learn more:
- [Overview of Copilot in Microsoft Fabric](/fabric/get-started/copilot-fabric-overview)
- [Overview of Copilot for Power BI](/power-bi/create-reports/copilot-introduction)
- [Copilot for Power BI Demo](https://www.youtube.com/watch?v=wr__6tM5U6I)
- [Analyzing Snapshot Serengeti data with Microsoft Fabric](https://aka.ms/fabric-e2e-serengeti)

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

### Copilot in Windows
 
Copilot in Windows is an AI-powered intelligent assistant designed to enhance user efficiency and creativity. It aids in retrieving answers and inspirations from the web, supporting creative tasks, and aiding focus on the current task. Users can adjust PC settings, organize windows, and initiate creative projects with Copilot's assistance. It's easily accessible, ready with a keystroke, and extends support both online and within Windows apps, making it a user-friendly tool for various tasks. 
 
Learn more:
- [AI with Copilot in Windows](https://www.microsoft.com/windows/copilot-ai-features)

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

## Extend Copilot

Copilot offers a comprehensive range of features right out of the box, yet many users continue to depend on external tools, services, and data to carry out tasks. To bridge this gap, Copilot can be extended using plugins, connectors, or message extensions.

:::image type="content" source="media/copilot-across-microsoft-cloud-extend.png" alt-text="A diagram showing extend options for a Copilot across the Microsoft Cloud." border="false" :::

### Benefits of extending Copilot

By extending Copilot, developers can integrate external data directly to minimize user context shifts while enhancing productivity and collaboration.  Copilot can also be extended to provide a new way for developers to surface external data directly in Microsoft products that users use every day. For instance, enterprises and ISVs can build plugins to integrate their own line-of-business APIs and data directly into Copilot.

Adding plugins, connectors, or message extensions to Copilot helps users make the most out of these AI assistants. Users can more easily discover and work with enterprise data, tools, and services using natural language, which improves productivity and enhances teamwork.


#### Extending with Plugins

Plugins allow users to interact directly with APIs and services enhancing the capabilities of Copilot and broadening its range of functionality by pulling in external data. For example, plugins can enable Copilot to retrieve real-time data, such as the latest news coverage on a product launch, or knowledge-based information such as a team's project items.

:::image type="content" source="media/api-data-is-processed-by-copilot.png" alt-text="A diagram showing Microsoft Copilot interacting with a plugin." border="false" :::

One example of plugins is Power Platform connectors which offer a set of prebuilt actions and triggers for developers to build their own apps and workflows. They can be used to extend Copilot for Microsoft 365 and Copilot in Power Platform experiences with data from external apps.

:::image type="content" source="media/power-platform-connector.png" alt-text="A diagram showing the key tasks that a Power Platform connector performs." border="false" :::

Developers can also create plugins that use schemas to add custom functionality to Copilot experiences by connecting their own application data to Microsoft Copilot. Plugins enable a Copilot experience to interact with your own APIs, enhancing the experience with the ability to perform a wide range of actions.

Learn more:
- [Power Platform Connectors Overview](/connectors/connectors)
- [Create a Custom Power Platform Connector from Scratch](/connectors/custom-connectors/define-blank)
- [Overview of Plugins for Microsoft Copilot](https://review.learn.microsoft.com/copilot-plugins/overview?branch=pr-en-us-1)

#### Extending with Microsoft Graph connectors

Microsoft Graph connectors import external content into Microsoft 365. Once the content is imported, all of Microsoft 365 including search, context IQ, microsoft365.com, and Copilot for Microsoft 365 can access the content. Microsoft Graph connectors are responsible for three key tasks:

1. Creating an external connection in Microsoft 365
1. Defining a schema for the external content
1. Managing external content that’s imported into Microsoft 365

:::image type="content" source="media/graph-connector.png" alt-text="A diagram showing the key tasks that a Microsoft Graph connector performs." border="false" :::

Learn more:
- [Build your First Custom Microsoft Graph Connector](/graph/connecting-external-content-build-quickstart)
- [Publish your Custom Microsoft Graph Connector](/graph/custom-connector-sdk-sample-publish)

#### Extending with message extensions

Message extensions allow users to interact with enterprise services through Microsoft Teams chats. They can search or initiate actions in an external service and post that information, in the form of adaptive cards, into a Teams message. By using message extensions, you can extend the built-in functionality of Copilot for Microsoft 365.

Learn more:
- [Message Extensions Overview](/microsoftteams/platform/messaging-extensions/what-are-messaging-extensions)
- [Build Message Extensions using Bot Framework](/microsoftteams/platform/messaging-extensions/build-bot-based-message-extension)

## Build your own Copilot

In addition to adopting and extending Microsoft Copilot, a custom AI copilot can be created to create a personalized conversational AI experience for users using Azure OpenAI, Cognitive Search, Microsoft Copilot Studio, and other Microsoft Cloud technologies. A custom copilot can integrate company data and documents, retrieve real-time data from external APIs, and be embedded in company applications. For example, a custom copilot can be built to provide a chat interface to access help desk tickets and knowledge base articles. Rather than using traditional search techniques or navigating across multiple applications, users can use the conversational UI provided by the custom Copilot to find the information they need.

:::image type="content" source="media/copilot-across-microsoft-cloud-build.png" alt-text="A diagram showing the build options for creating a custom Copilot." border="false" :::

### Chat with your own data using Azure OpenAI and Cognitive Search

Companies can integrate their own data with Azure OpenAI models to create an advanced conversational AI platform that can be embedded in their applications. This enables users to interact with company data and documents using natural language, making it easier to find information and complete tasks.

:::image type="content" source="media/chat-with-your-data.png" alt-text="A diagram showing how Cognitive Search can be used with Azure OpenAI to chat against your own data and documents." border="false" :::

Learn more:
- [Quickstart: Chat with Azure OpenAI Models Using Your Own Data](/azure/ai-services/openai/use-your-data-quickstart)
- [Integrate OpenAI, Communication, and Organizational Data Features into a Line of Business App](/microsoft-cloud/dev/tutorials/openai-acs-msgraph?WT.mc_id=m365-94501-dwahlin)

### Solution Accelerators and samples

Accelerators and samples are available to help you get started building a custom copilot that integrates with your own data and applications.

- ["Chat with your data" Solution Accelerator](https://github.com/azure-samples/chat-with-your-data-solution-accelerator)
- [Azure Chat Solution Accelerator powered by Azure Open AI Service](https://github.com/microsoft/azurechat)
- [ChatGPT + Enterprise data with Azure OpenAI and Cognitive Search](https://github.com/Azure-Samples/azure-search-openai-demo)
- [ChatGPT-Powered and Voice-Enabled Assistant](https://github.com/gcordido/VoiceEnabledGPT)

### Microsoft Copilot Studio

Microsoft Copilot Studio is a low-code tool that enables the creation of robust AI-driven chatbots capable of handling an array of interactions, from addressing standard inquiries to navigating intricate dialogues for issue resolution. Copilot Studio not only simplifies the process of building intelligent chatbots, but also seamlessly integrates across a variety of digital platforms such as websites, mobile apps, Facebook, Microsoft Teams, and other channels supported by Azure Bot Framework.

Learn more:
- [Microsoft Copilot Studio Collection](https://aka.ms/Ignite23CollectionsBRK212H)