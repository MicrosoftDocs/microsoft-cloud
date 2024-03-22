---
title: Creating Generative AI Experiences with the Microsoft Cloud - A Guide for ISVs
description: "This article explores options for ISVs to extend Microsoft Copilots and how ISVs can leverage the different aspects of the Microsoft Cloud to create innovative and engaging AI experiences for users"
author: willstan
ms.author: willstanley
ms.service: cloud-for-industries
ms.topic: overview 
ms.date: 3/22/2024

#customer intent: As a developer, I want to understand how ISVs can leverage Microsoft Cloud to extend or create copilots to create value-added solutions for their end customers.

---

# Creating Generative AI Experiences with the Microsoft Cloud: A Guide for ISVs

Welcome to your guide to creating unique Generative AI (GenAI) experiences with the Microsoft Cloud. As an **independent software vendor (ISV)**, you're in a prime position to harness the power of GenAI to innovate and deliver captivating solutions to your customers.

**What is Microsoft Cloud?**  
The [Microsoft Cloud](https://www.microsoft.com/en-us/microsoft-cloud/what-is-microsoft-cloud) is a comprehensive and integrated platform offering a wide range of capabilities and services. It includes Azure AI, Microsoft 365, Microsoft Fabric, and more, putting it at the forefront of the global Generative AI revolution.

This platform allows you to surface your proprietary data and functionality into various areas, including Microsoft 365, a hub of productivity and collaboration accessed by millions.

This guide helps you navigate the expansive possibilities available across the Microsoft Cloud ecosystem.

**What are Copilots?**  
We refer to a copilot as an AI-powered virtual assistant that enhances user productivity by assisting humans with complex cognitive tasks, providing contextual suggestions, and driving data-rich insights. These copilots can be grounded in specific customer or ISV data and context, offering an opportunity for ISVs to create generative AI experiences that understand business-specific data.  

## Scenarios and Approaches

:::image type="complex" source="media/isv-genai-approaches.png" alt-text="Diagram showing Microsoft Copilot components, AI orchestration, and the underlying Microsoft Cloud infrastructure." border="false":::
A diagram listing the three copilot extensibility ISV approaches. First, extend Copilot allows you to surface your data and service into Microsoft’s Copilots. Second, create copilots allows you to create copilots anywhere with minimal coding and optional Microsoft data ingestion. Third, full control allows you to build your own end-to-end AI experiences. Each of the three options has more details that are outlined in the following text.
:::image-end:::

This guide provides scenario-led guidance to assist ISVs in navigating the expansive field of GenAI on Microsoft Cloud. Our aim is to help you select the most suitable patterns and technologies for your unique requirements, arranged in three high-level approaches to crafting AI experiences.

Our **approaches** are broken into **patterns** based on **scenarios** to help you navigate the most appropriate path for your scenario and requirements.

> [!IMPORTANT]
> Please note, these approaches and their patterns **are not mutually exclusive**. They can be combined to create a tailored solution that best fits your unique requirements and scenarios.

**Approach 1: Surface your data and services into Microsoft’s Copilots:**

This approach is designed for ISVs wishing to integrate their data and services into Microsoft's Copilots. The focus is using plugins and Graph connectors to enhance user experiences.

:::row:::
    :::column:::
       **Scenario**: I am an ISV where **my end users perform work in Microsoft apps such as Teams, Word, Outlook**, and they need to...
    :::column-end:::
    :::column:::
        ...access information using natural language interfaces and I have an existing service I want to make available via these Microsoft 365 apps.
    :::column-end:::
    :::column:::
        **Pattern A**: [Create plugins to augment Copilot’s capabilities with ISV data and functionality](#pattern-a-create-plugins-to-enhance-an-existing-copilots-functionality)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
        ...draw insights from our ISV data sources combined with user centric Microsoft Graph data and their organization’s line of business data.
    :::column-end:::
    :::column:::
        **Pattern B**: [Use Graph connectors to make ISV data available for Copilot experiences.](#pattern-b-use-graph-connecters-to-bring-your-data-to-copilot-experiences)
    :::column-end:::
:::row-end:::

**Approach 2: Create copilots anywhere, with minimal coding and optional Microsoft data integration:**

This approach is for ISVs aiming to enrich their apps with Microsoft's data and tools, or who want to create their own AI assistants with Azure. It involves making use of the Microsoft Graph API, Copilot Studio plugins, Teams AI Library, or enabling customers to create their own copilot experiences with your data via connectors.

:::row:::
    :::column:::
        **Scenario**: I'm an ISV where my end users work anywhere...
    :::column-end:::
    :::column:::
        ...and I want to enable them to create their own copilot experiences using our data and services.
    :::column-end:::
    :::column:::
        **Pattern C**: [Develop Power Platform Connectors to enable customer-driven copilots in Copilot Studio](#pattern-c-develop-power-platform-connectors-to-enable-customer-driven-copilots-in-copilot-studio)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **Scenario**: I'm an ISV where **my end users work in my existing applications and UI**, who want us to...
    :::column-end:::
    :::column:::
        ...incorporate Microsoft user-centric [Graph](/graph/overview) data into my copilot.
    :::column-end:::
    :::column:::
        **Pattern D**: [Use Microsoft Graph API in your copilots](#pattern-d-leverage-microsoft-graph-api-in-your-copilots)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
    :::column-end:::
    :::column:::
        ...provide a conversation experience within my existing application, which can answer questions and turn conversations into actions.
    :::column-end:::
    :::column:::
        **Pattern E**: [Create your own AI Assistants with Azure](#pattern-e-bring-a-copilot-experience-to-your-apps-with-azure-openai-assistants)
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        **Scenario**: I'm an ISV who’s Copilot experience targets **Microsoft Teams** and includes requirements to...
    :::column-end:::
    :::column:::
        ...create smart Teams bots that integrate to GenAI, run bots in Teams and have context-aware conversations or use Teams chat experience and I as an ISV focus on complex business logic.
    :::column-end:::
    :::column:::
        **Pattern F**: [Use the Teams AI Library to Build Your Own copilot](#pattern-f-use-the-teams-ai-library-to-build-your-own-copilot)
    :::column-end:::
:::row-end:::

**Approach 3: Full Control: Build Your Own (BYO) end-to-end AI experiences:**

This approach is geared towards ISVs seeking to craft entirely new AI experiences or copilots using tools such as Azure AI Studio and Semantic Kernel. It offers maximum control and customization, providing limitless possibilities.

:::row:::
    :::column:::
        **Scenario**: I'm an ISV seeking to develop cutting-edge interoperable AI experiences that...
    :::column-end:::
    :::column:::
        ...require specialized handling of intricate, multimodal data, which might include fine-tuning AI models to meet specific accuracy needs.

        ...use ISV's custom AI models and services for tailored solutions.

        ...provide complete control over the conversational experience, including system prompts, temperature, tone, and custom safety requirements.
    :::column-end:::
    :::column:::
        **Pattern G**: [Build new interoperable AI experiences or copilots using your existing APIs using Azure AI Studio](#pattern-g-build-your-own-copilot-with-azure-ai-studio)

        **Pattern H**: [Build your own copilot with Semantic Kernel](#pattern-h-build-your-own-copilot-with-semantic-kernel)
    :::column-end:::    
:::row-end:::

> [!TIP]
> For a deeper understanding, we encourage you to explore the detailed content available in the **Get Started** links provided in each pattern.

:::row:::
   :::column span="2":::
      The Microsoft Copilot stack comprises three distinct tiers: the back end (with the data sitting in respective repositories), an AI orchestration tier in the middle, and the front end (UI experience of your copilot). Within each tier, there are recommended layers that should be considered when building a copilot.

      As ISVs move from Approach 1 to Approach 3, they engage more deeply with each tier, assuming greater development responsibilities. For example, creating a plugin under Approach 1 means Microsoft handles AI orchestration, including data integration and Responsible AI. Conversely, developing a custom copilot might require full control over the user experience, orchestration layer, data management, and Responsible AI.    
   :::column-end:::
   :::column span="":::
      :::image type="complex" source="media/isv-copilot-stack.png" lightbox="media/isv-copilot-stack.png" alt-text="Diagram showing Microsoft Copilot Stack components, AI orchestration, and the underlying Microsoft Cloud infrastructure." border="false":::
        A square diagram showing the components of Microsoft Copilot Stack. Enveloping the blocks is the label "AI safety and security". Within the box, there are two sections separated by a connector. The top box stacks Copilot Studio, Microsoft Copilot, your copilots, and Microsoft applications together, then connects to the middle connector labeled "AI orchestration". The bottom box stacks your data, foundation models and AI toolchain, and AI infrastructure, with arrows pointing back to the middle connector labeled "AI orchestration". At the bottom of the entire stack Microsoft Cloud spans the entirety of the products.
      :::image-end:::
   :::column-end:::
:::row-end:::

> [!NOTE]
> While "copilot"  refers to the general concept of a generative AI assistant, "Copilot" refers to specific Microsoft products, such as Microsoft 365 Copilot or Dynamics 365 Copilot that ISVs can integrate with.

Each option varies in complexity and effort. Adopting existing Microsoft Copilots is straightforward, extending them with plugins requires minimal effort, and crafting a new copilot experience might need design, science, and engineering.  

It’s important to remember that an AI solution is as good as the data you ground your models on and use as the context. Ready-to-use Microsoft Copilots already support a range of scenarios and can be extended with your data, functions, and processes. However, the user interface can't be extended. Therefore, it's important to carefully consider your specific scenario, how you can apply GenAI algorithms, and how your user (the "pilot") can benefit from your copilot capability.

## Approach 1: Surface your data and services into Microsoft’s Copilots

ISVs looking to surface their existing services, data, and processes into Microsoft's Copilots or Microsoft 365 applications can do so by building plugins and connectors.  

This approach allows for example, Microsoft 365 Copilot to interact with APIs from other software and services, surfacing up-to-date information, execute actions, and performing new types of computations.

### Pattern A: Create plugins to enhance an existing Copilot’s functionality

:::row:::
   :::column span="":::
      Plugins are extensions that augment the capabilities of an existing Copilot, allowing it to interact with ISV apps and services. They can expand a user’s capabilities by enabling the interaction with your APIs, via natural language conversation. For example, a plugin could allow Copilot to retrieve useful information, perform new computations, or safely execute actions on the user's behalf.
   :::column-end:::
   :::column span="":::
      :::image type="complex" source="media/isv-genai-pattern-plugins-a.png" lightbox="media/isv-genai-pattern-plugins-a.png" alt-text="Diagram illustrating the interconnected components of Microsoft Copilot with ISV applications and data via plugins." border="false":::
      A diagram showing four components of Microsoft Copilot and their connected tissue. First, Microsoft Graph data points to Microsoft Copilot. Microsoft Copilot then has a bi-directional arrow point to and receiving from a plugin component connect to ISV application. The ISV application is connected to an ISV data source.
      :::image-end:::
   :::column-end:::
:::row-end:::

ISVs can create plugins using various tools, including Teams Message Extensions and Power Platform plugins through Copilot Studio. New plugins can be published to Microsoft’s Copilot ecosystem via Partner Center, where IT admins can approve them for use by end users.

**ISV scenarios:**

- ISVs looking to surface their existing services on Microsoft 365 client apps
- Users can search, update, and perform actions in an ISV application or any external systems, from Microsoft 365 client apps, such as Teams, Outlook, Word etc.
- A user on Teams could find relevant information from an external ticketing or CRM system your customers use, optionally including executing actions on the user's behalf, within the system

**Partner advantage:**

- Service the millions of users and companies using Microsoft 365 client apps, meet users where they work right now.
- Increase your solution service visibility by surfacing them in Microsoft 365 client apps.
- Reduce your users’ friction by eliminating the need to navigate between multiple apps and canvases.
- A consistent user experience across Microsoft 365 apps with universal integration and continuity across Microsoft 365 apps.
- For example, a Contoso user discovers latest customer account summarized with data coming from Outlook, SharePoint and Fabrikam's external system, without leaving Teams.

**Where to start:**

- [Build message extensions for Microsoft Copilot for Microsoft 365 | Microsoft Learn](/microsoft-365-copilot/extensibility/overview-message-extension-bot)
- [Create copilot plugins - overview (preview) - Microsoft Copilot Studio | Microsoft Learn](/microsoft-copilot-studio/copilot-plugins-overview)

**Key Takeaways:**

- Plugins are a way to surface ISV services and apps on Microsoft Copilots allowing end users interact with ISV apps and services from Microsoft 365 client apps.
- ISVs can create plugins using tools like Teams Message Extensions and Copilot Studio plugins.
- Plugins can increase your solution's visibility and discoverability through Partner Center

### Pattern B: Use Graph Connecters to bring your data to Copilot experiences

:::row:::
   :::column span="":::
      [Graph connectors](/graph/connecting-external-content-connectors-overview) enable ISVs to connect their data to the Microsoft 365 Semantic Index. Their data becomes searchable and actionable for users, directly from Microsoft 365 client apps such as Teams, Outlook, and Word. Microsoft Copilot becomes grounded in ISV data, whether cloud or on-premises, via the Microsoft Graph. Furthermore, ISVs can use [Microsoft Fabric](/fabric/get-started/microsoft-fabric-overview), a unified data platform delivered as a SaaS product, to bring their data into the Microsoft Cloud and easily connect it to the Microsoft Graph.
   :::column-end:::
   :::column span="":::
      :::image type="content" source="media/isv-genai-pattern-graph-connectors-b.png" lightbox="media/isv-genai-pattern-graph-connectors-b.png" alt-text="A diagram showing 'Copilot' connected to 'Microsoft Graph data' and an 'ISV app,' which is linked to an 'ISV data source' through a 'Graph connector.'" border="false":::
   :::column-end:::
:::row-end:::

ISVs can build Graph connectors using the Microsoft Graph Connectors API, which supports a range of data sources, file systems, web pages, enterprise applications, and more.

Graph connectors can also enrich the data with AI-powered capabilities, such as natural language processing, entity extraction, and image analysis. By using Graph connectors, ISVs can extend Microsoft Copilot with their own data, enhancing the user experience and enabling more personalized and secure interactions.

[Hundreds of Graph connectors already exist](https://www.microsoft.com/microsoft-search/connectors/?category=&query=). For example, the Jira Cloud graph connector can elevate Jira objects to the same level as Microsoft 365 Graph data, allowing comprehensive reasoning and universal integration, leading to enhanced and richer insights. The connector allows end users to search for Jira objects from Microsoft 365 Copilot using natural language interface.

**ISV scenarios:**

- ISVs whose customers perform work in Microsoft 365 and want to enable end-users to draw insights from ISV data sources combined with user centric Microsoft Graph data.
- Users can retrieve, summarize, and reason over data from ISV applications, combined with other Microsoft 365 graph data, for example, emails, word documents etc.
- Communications Director needs to find and redraft PR emails in Outlook that are enriched with content sourced from an ISV graphics design application

**Partner advantage:**

- Meet users where they work. A vast user base uses Microsoft 365 client applications and now can access Your own data and service in one unified experience.
- Enriched insights by combining ISV data with Microsoft Graph data.
- Universal integration with Microsoft 365 apps, and Microsoft Search, Context IQ and Viva, with one connector.
- Expanded channels to showcase ISV data, potentially increasing user base.

**Where to start:**

- Learn more about graph connectors at [aka.ms/graph-connectors](https://aka.ms/graph-connectors)
- [Build your own graph connector at Build Microsoft Graph connectors for Microsoft Copilot for Microsoft 365 | Microsoft Learn](/microsoft-365-copilot/extensibility/overview-graph-connector)

**Key Takeaways:**

- Graph connectors allow ISVs to bring their data into the Microsoft Copilot ecosystem, enhancing the user experience with personalized and secure interactions.
- ISVs can use [Microsoft Fabric to bring their data into the Microsoft Cloud and connect it to the Microsoft Graph](https://blog.fabric.microsoft.com/en-US/blog/microsoft-365-data-microsoft-fabric-better-together/).
- By using Graph connectors, ISVs can combine their data with Microsoft 365 graph data to provide enriched insights and achieve universal integration with Microsoft 365 apps.

## Approach 2: Create copilots anywhere with minimal coding and optional Microsoft data integration

ISVs can bring the power of tools and data sitting in Microsoft Graph into their own apps, enhancing their functionality and user experience.

### Pattern C: Develop Power Platform Connectors to enable customer-driven copilots in Copilot Studio

:::row:::
   :::column span="":::
      Copilot Studio enables customers to create low-code AI apps that can respond to common user queries, using data from their organization and Microsoft and partner data sources. Copilot Studio uses Power Platform Connectors to bring in data from potentially any source, where there are more than 500 connectors today. As an ISV you can create connectors to your data and services, to empower your customers to create their own internal copilots and AI apps, grounded in ISV data.
   :::column-end:::
   :::column span="":::
      :::image type="complex" source="media/isv-genai-pattern-power-platform-connectors-c.png" lightbox="media/isv-genai-pattern-power-platform-connectors-c.png" alt-text="Diagram of Microsoft Copilot Studio plugin integrating with various data sources and ISV applications." border="false":::
A diagram showing components of the Microsoft Copilot Studio plugin. Three sources connect to the Copilot Studio plugin, including Microsoft Graph data, Power Platform connectors, and other data sources. The Copilot Studio plugin then connects to ISV applications via a plugin.
:::image-end:::
   :::column-end:::
:::row-end:::

These AI apps can be surfaced to end users across various platforms, including websites, mobile apps, Microsoft Teams, or any channel supported by the Azure Bot Framework.

**Example scenarios:**

- ISVs looking to provide a chat bot experience to their customers, within their existing applications, which can respond to questions and turn conversations into actions.
- Users asking questions within your application and receiving answers grounded in ISV, Microsoft, or customer data sources.
- Create customer connector from your existing APIs and convert it to plugin using Copilot Studio allowing these APIs to be called from a chat bot with natural language interface.
- Convert existing Power Automate flows into plugins that can be called from Microsoft copilot chat to perform actions and retrieve information.
- Access data through natural language interface from enterprise systems such as Zendesk, GitHub, and Salesforce through these connectors in Power Platform.

**Partner Advantage:**

- Harness the power of existing Microsoft and non-Microsoft connectors to enhance and enrich your application effortlessly.
- Expedite plugin development by applying Power Platform custom connector approach for swift and efficient integration.
- Enhance time-to-value through the low-code capabilities of Copilot Studio.
- Gain a competitive edge by integrating AI capabilities into your app with minimal coding.

**Where to start:**

- [Microsoft Copilot Studio plugin architecture - Microsoft Copilot Studio | Microsoft Learn](/microsoft-copilot-studio/copilot-plugins-architecture)
- [Embed a Power Virtual Agents control using chatbot control | Microsoft Learn](/power-platform/release-plan/2023wave1/power-apps/maker-embed-pva-control-power-apps-via-chatbot-control)

**Key Takeaways:**

- Copilot Studio offers a platform for creating low-code AI apps that can enhance existing applications with chatbot capabilities grounded in ISV data or functions.
- The platform supports both existing Power Platform Connectors and custom connectors, offering flexibility in integrating ISV services and data sources.
- The integration of AI capabilities can significantly improve the user experience and give your app a competitive edge.

### Pattern D: Leverage Microsoft Graph API in your copilots

:::row:::
   :::column span="":::
      The Microsoft Graph API offers a powerful endpoint to access user-centric data from Microsoft 365 applications, which includes Calendar, Bookings, Outlook, Teams, OneDrive, SharePoint, and [more](/graph/overview-major-services). With this API, you can enrich your apps with data from Microsoft 365, enabling users to derive richer insights and analytics.
   :::column-end:::
   :::column span="":::
      :::image type="complex" source="media/isv-genai-pattern-graph-api-d.png" lightbox="media/isv-genai-pattern-graph-api-d.png" alt-text="Diagram illustrating the interconnected components of Microsoft Copilot with ISV applications and data via Graph API." border="false":::
      A diagram showing four components of Microsoft Copilot and their connected tissue. First, Microsoft Graph data points to Microsoft Copilot. The Microsoft Graph data also has an arrowing connecting it to ISV application, with the arrow symbolizing the Graph API. The ISV application is connected to an ISV data source.
      :::image-end:::
   :::column-end:::
:::row-end:::

**ISV scenarios:**

- Customer and partners using existing ISV application looking to combing Microsoft user centric Graph data in their copilot.
- An ISV with a project management app wants to incorporate Microsoft 365 calendar data and project documents data to help users track deadlines and milestones within the app.
- An ISV with a CRM app wants to incorporate Microsoft 365 contact and email data to enhance customer profiles and communication logs.

Consider Fabrikam, a versatile Human Capital Management (HCM) software equipped with a flexible HR suite, empowering seamless automation of various workflows such as talent acquisition, employee rewards management, and feedback processes. In their continuous pursuit of innovation, Fabrikam introduces a cutting-edge copilot feature atop their HR suite. Now, they aim to elevate their application even further by integrating user-centric graph data. This enhancement involves using Graph API to incorporate employees' calendars, encompassing details like scheduled time-offs and 1:1s for feedback processes etc.

**Partner advantage:**

- Uncover enriched insights by combining your data with Microsoft 365 Graph.
- Seamless Integration: Standardized access to Microsoft 365 data for easier integration with your apps.
- Improved User Experience: Provide a more seamless user experience with access to relevant Microsoft 365 data and features within your app.
- Enhanced Functionality: Add new features and capabilities to your app using Microsoft 365 data.
- Scalability and Efficiency: Focus on building and improving your apps while the Graph API handles data retrieval.

**Where to start:**

- [Use Graph Explorer to try Microsoft Graph APIs - Microsoft Graph | Microsoft Learn](/graph/graph-explorer/graph-explorer-overview)
- [Quick Start - Microsoft Graph](https://developer.microsoft.com/en-us/graph/quick-start)

**Key Takeaways:**

- The Microsoft Graph API allows ISVs to enrich their apps with user-centric data from Microsoft 365.
- Via Graph APIs you can leverage the Microsoft 365 Semantic Index, a more advanced search experience built for the era of Copilots.
- By using the Graph API, ISVs can enhance their apps with richer insights and analytics.

### Pattern E: Bring a copilot experience to your apps with Azure OpenAI Assistants

ISVs can adopt this low-code approach in Azure’s AI Services to bring copilot-like experiences to their own applications. It offers a fast path to apply GPT’s [function calling](/azure/ai-services/openai/how-to/assistant-functions?tabs=python) to call your own APIs simply by describing your function's structure in JSON and providing a sandboxed python environment to [run and execute code](/azure/ai-services/openai/how-to/code-interpreter) to help formulating responses to user’s questions.

Both these features can be useful in offloading non-language-based challenges to conventional code or existing systems that are better suited for the task, for example simple maths tasks.

While you don’t have direct access to the system prompt and temperature, you can similarly affect the behavior of your Assistant via Custom Instructions that have a heavy influence on the personality of your copilot-like experience.

**Partner Advantage:**

- Azure OpenAI Assistants provide a low-code approach, enabling ISVs to quickly integrate Generative AI capabilities into their applications without extensive development effort.

**Where to start:**

- [Quickstart - Getting started with Azure OpenAI Assistants (Preview) - Azure OpenAI | Microsoft Learn](/azure/ai-services/openai/assistants-quickstart?tabs=command-line&pivots=programming-language-studio)
- [How to create Assistants with Azure OpenAI Service - Azure OpenAI | Microsoft Learn](/azure/ai-services/openai/how-to/assistant)

**Key takeaways:**

- ISVs can use Azure OpenAI Assistants to create interactive, natural language interfaces that enhance user engagement. These assistants can call out to APIs via simply by describing them via JSON.
- An Azure OpenAI Assistant can write and execute code, in a sandbox, based on a user’s prompt, to solve a non GenAI problem.

### Pattern F: Use the Teams AI Library to build your own copilot

:::row:::
   :::column span="":::
      ISVs can also use the [Teams AI Library](/microsoftteams/platform/bots/how-to/teams%20conversational%20ai/teams-conversation-ai-overview) to add natural language capability in their existing Teams chatbot. This library allows ISVs to focus on their business logic, while using the Teams scaffolding to handle conversational interactions. ISVs can surface their chat bots in Teams, offering users a more natural and intuitive way to interact with their apps.
   :::column-end:::
   :::column span="":::
      :::image type="complex" source="media/isv-genai-pattern-teams-ai-library-f.png" lightbox="media/isv-genai-pattern-teams-ai-library-f.png" alt-text="Flowchart of the Microsoft Teams AI library pattern with prompt engineering and data exchange with ISV apps and Azure OpenAI Service." border="false":::
      A diagram showing the flow of Microsoft Teams’ AI library pattern. Frist, prompt engineering flows into the Teams AI Library. Data then flows out of the Teams AI Library into two sources: ISV application and Azure OpenAI Service. These two locations flow data bidirectionally.
      :::image-end:::
   :::column-end:::
:::row-end:::

**ISV Scenario:**

- End users are using Teams and ISV partner is looking to surface their service or functionality on Teams with Bot-like capabilities.
- No integration is needed with Graph data and ISV partner is looking to focus on the service and business logic without integrating with Teams Copilot capabilities.
- With prebuilt Teams app templates and built in moderation safety features, ISV partner can easily add LLM capability to their existing chat bot.

**Partner Advantage:**

- Add ChatGPT like conversational experiences, with control over prompt engineering to your bot and reuse built-in safety features.
- Built on top ready to reuse capabilities like
  - Conversation session history offered by Teams AI mechanism.
  - Multi-language support.
  - Multi Large Language Models support, beyond OpenAI models.
  - Action planner that can help map to actions based on the user intent.
  - Ready to use augmentation mechanism to change the way model is responding through parameters or system prompt change.
  - Extra reasoning that can ground the answers from the model on your data.

**Where to Start:**

- [Introduction to Teams AI Library: Teams AI library - Teams | Microsoft Learn](/microsoftteams/platform/bots/how-to/teams%20conversational%20ai/teams-conversation-ai-overview)
- [List of technical capabilities: Teams AI library capabilities - Teams | Microsoft Learn](/microsoftteams/platform/bots/how-to/teams%20conversational%20ai/how-conversation-ai-core-capabilities)

**Key Takeaways:**

- Team AI library provides an easy way to light up an ISV developed bot in Teams with the power of LLMs.
- It doesn't require the integration with current Microsoft Copilot capabilities, can provide a task-oriented experience.
- It offers many possibilities from an engineering perspective but also ready to use capabilities Out Of the Box, making the whole development process easier.

If you want to power your bot in Teams with LLMs, Teams AI Library is the way for you to go.

## Approach 3: Full Control: Build Your Own (BYO) end-to-end AI experiences

:::row:::
   :::column span="2":::
      ISVs can use the Microsoft Copilot Stack to build entirely new AI experiences, as copilots or intelligent assistants. An ISV building in this middle part of the stack takes responsibility for AI Orchestration - where Microsoft offer various options, all of which apply Microsoft’s foundational models, AI toolchain and AI infrastructure.

      [Semantic Kernel](https://github.com/microsoft/semantic-kernel) can be leveraged to build the same AI orchestration patterns that powers Microsoft Copilots, in your copilots. It's available as [an SDK you can develop against directly](#pattern-h-build-your-own-copilot-with-semantic-kernel).  
   :::column-end:::
   :::column span="":::
      :::image type="complex" source="media/isv-copilot-stack-orchestration-expanded.png" lightbox="media/isv-copilot-stack-orchestration-expanded.png" alt-text="Diagram depicting the Microsoft CopilotSstack with AI orchestration expanded between frontend apps and foundational models, all on the Microsoft Cloud." border="false":::
      A square diagram showing the components of the Microsoft Copilot stack orchestration. The first box is labeled "Apps and Copilot Frontend" and is connected by a bidirectional arrow to a larger box labeled "AI orchestration", which is filled with stack components. Inside Copilot Studio, Azure AI Studio, prompt flow, evaluations, experiments, model catalog, fine-tuning, Semantic Kernel, and Machine Learning. The "AI orchestration" stack is connected to another box labeled "foundation models and data." At the bottom of the entire stack Microsoft Cloud spans the entirety of the products.
      :::image-end:::
   :::column-end:::
:::row-end:::

With most the investment creating a copilot service in the middle of the stack, ISVs have the freedom to connect this copilot service to various surfaces, including Teams, Microsoft 365 Copilot, Microsoft Copilot, your own application surfaces, websites, chat bots – or all. Essentially, when it comes to integration with an application surface – the top of our stack – every other pattern described here is also an option.

### Pattern G: Build your own copilot with Azure AI Studio

[Azure AI Studio](/azure/ai-studio/what-is-ai-studio) is an all-in-one platform for ISVs to build custom, intelligent assistants, or copilots. It combines capabilities from various Azure AI services, providing a unified workspace for developing and deploying generative AI applications. It's a collaborative platform where data scientists, developers, and other stakeholders can converge and work together.

With Azure AI Studio, ISVs gain full control over their copilot's behavior, personality, and capabilities. Whether using existing pretrained models from our [extensive catalog](/azure/ai-studio/how-to/model-catalog), fine-tuning models on your data, or training your own custom AI models, Azure AI Studio accelerates the development of AI experiences that handle complex multimodal data.

A standout feature of Azure AI Studio is its diverse range of models, catering to various industries and use cases. It allows ISVs to combine different models within a single solution to meet their unique requirements.

Integration with [Azure AI Search](/azure/search/search-what-is-azure-search) enables ISVs to implement a Retrieval Augmented Generation (RAG) pattern for unstructured data directly from Azure AI Studio, with the added advantage of AI Search’s Integrated Vectorization feature. This means any data your copilot needs can be automatically kept up-to-date in a vector database, facilitating fast and efficient retrieval during user prompt evaluation, saving you the task of implementing an indexing, chunking, embedding, and vectorizing pattern yourself.  
  
[Prompt Flow](/azure/machine-learning/prompt-flow/overview-what-is-prompt-flow), a feature of Azure AI Studio, offers a visualized graph for orchestrating executable flows with Large Language Models (LLMs), prompts, and Python tools. It facilitates debugging, sharing, and iterating on your flows with ease through team collaboration.  
  
For ISV teams who prefer a code-first approach, the [Azure AI SDK](/azure/ai-studio/how-to/sdk-install) offers a suite of packages for accessing Azure AI services, including the setup of Azure AI Studio projects and related resources. This allows developers and data scientists to manage AI components, configure AI models, pipelines, and services directly from code, while still making the graphical interface available for those who prefer it.  
  
Prototyping is easy in Azure AI Studio via its Playground. A typical journey for a team working on a project in Azure AI Studio could start with an individual validating an idea in the Playground. Once attractive results are produced, they can be prompted from the Playground to Prompt Flow as a versioned and customized flow. Now a versioned artifact in the AI Project, the wider team can contribute where the flow is accessible via Azure AI Studio UI and via code-only. Multiple branches of logic to differing LLMs can be tested and evaluated at this point.
  
Beyond the development phase, Azure AI Studio also provides an LLMOps toolchain, handling your end-to-end prompt engineering from development to production and ongoing maintenance.  
  
Azure AI Studio supports integration with Azure AI Search, Azure Open AI Service, and other Azure AI services, simplifying resource management for ISVs. It also provides a project-oriented workspace, fostering collaboration against shared compute, model deployments, and services.  

**ISV Scenarios:**

- A healthcare ISV building a telemedicine platform wants a copilot that understands medical jargon, assists doctors in diagnosing patients, and provides relevant treatment recommendations.
- A financial services provider needs a copilot that can analyze market trends, answer customer queries about investment options, and generate personalized financial reports.
- An e-learning platform wants a copilot that tutors’ students, explains complex concepts, and adapts its teaching style based on individual learning preferences.
- An insurance company speeds up documents analysis during the claim process by validating if current claim can be covered by the contract.
- Airline copilot can help you plan the journey, look for the tickets and hotels and book them once you are satisfied with the offer.
- A chain of restaurants is creating a copilot app to help new employees to get onboarded by guiding them through the whole process.
- An ISV offers their customers a VS Code extension to help developers build the integration with their APIs.

**Partner Advantage:**

- Customization and Control: Build a bespoke copilot that aligns precisely with your application's requirements.
- Scenario Flexibility: Cater to a wide range of scenarios, from domain-specific copilots to task automation and content generation.
- Integration with Existing Systems: Connect to databases, APIs, and other services to enhance your copilot's capabilities.
- Brand Identity and User Experience: Shape your copilot's personality to align with your brand voice and enhance the user experience.
- Build experience: Open-source and highly extensible SDK, Semantic Kernel let you build intelligent agents that can call your existing APIs.  With Semantic Kernel, you can use the same AI orchestration patterns that power Microsoft’ copilots in our own apps.
- Scalability and Deployment: Deploy your copilot across multiple clients or applications, serving thousands of users simultaneously.

**Where to start:**

- [What is AI Studio? - Azure AI Studio | Microsoft Learn](/azure/ai-studio/what-is-ai-studio?tabs=home)
- [Build and deploy your own copilot with Prompt Flow in Azure AI Studio | Microsoft Learn](/azure/ai-studio/tutorials/deploy-copilot-ai-studio)
- [Build and deploy your own copilot with the Azure AI CLI and SDK | Microsoft Learn](/azure/ai-studio/tutorials/deploy-copilot-sdk)

**Key Takeaways:**

- Azure AI Studio offers a powerful platform for creating custom, intelligent assistants, or copilots.
- ISVs can shape their copilot's behavior, personality, and capabilities, creating a truly bespoke solution.
- Azure AI Studio supports a wide range of scenarios and integrates seamlessly with existing infrastructure.
- Creating a custom copilot with Azure AI Studio can enhance the user experience and provide tailored solutions for specific use cases.
- AI Studio delivers you a copilot service (or backend), surfaced as a single scaled endpoint
- An ISV then has options to connect the service to an app, front end, or conversation surface of their choice, including any of the previous patterns above.
- Remember, this pattern can be combined with others based on your specific needs. For instance, you might want to pair this pattern with Pattern A and plug-in to a Microsoft Copilot, or Pattern F to surface your own copilot bot in Teams.

### Pattern H: Build your own copilot with Semantic Kernel

Semantic Kernel is an open-source SDK that empowers developers to create sophisticated copilots within their applications. It supports a range of programming languages, including C#, Java, and Python, making it accessible to a wide developer community. Semantic Kernel enables the orchestration of AI plugins, allowing for integration with various AI models, including from Azure OpenAI and Hugging Face.

Semantic Kernel encapsulates the essence of Microsoft Copilots' AI orchestration patterns, providing developers with tools to build **agents** and **copilots**.

Agents are AI systems that can answer questions and automate processes for users. They range from simple chatbots to fully automated AI assistants.
Copilots, a special type of agent, work alongside users. Unlike fully automated agents, copilots provide suggestions and recommendations, allowing users to retain control.

[Plugins](/semantic-kernel/agents/plugins/?tabs=Csharp): These provide skills to your agent. You can create plugins for tasks like sending emails, retrieving information from databases, or asking for help.

[Planners](/semantic-kernel/ai-orchestration/planner): Agents use planners to generate plans for completing tasks. For instance, a copilot helping a user write an email would create a plan with steps like gathering recipient details and composing the email.

The SDK comes with VS Code extension, sample [Chat Copilot](https://github.com/microsoft/chat-copilot) app but also with starters to offer you a scaffolding to bring your ideas to live.

One you decided to start working with Semantic Kernel, we suggest defining couple of capabilities before starting to code:

- Start by defining a copilot’s persona and behavior.
- Create plugins for common tasks your copilot will assist with.
- Use planners to generate plans for copilot actions.
- Plan to test thoroughly to ensure a refined user experience.
- Make sure you're able to gather feedback from your users and implement this in the behavior of the agent or copilot.

**ISV Scenarios:**

- You're building a copilot that is a part of your own application (customer development tool or HR system), and you want people to stay in the realm of the same UI.
- You need a full control on the orchestration engine, RAG implementation, model choices and model parameters.
- With Your copilot service, you want to allow your customers to build extension on top of your solution through plugins.
- Your solution utilizes canvases and other media than just text.

**Partner Advantage:**

- Full control over your copilot behavior with access to opinionated orchestration engine used by Microsoft to build first party Copilots.
- Seamlessly ground models on your own enterprise data and integrate structured, unstructured, and real-time data using Microsoft Fabric OneLake. This allows developers to employ sophisticated hybrid and semantic search to power retrieval augmented generation (RAG) applications.
- Access to superior tools for refining AI responses using prompt engineering and LLMOps tools like prompt flow.

**Where to start:**

- [GitHub - microsoft/semantic-kernel: Integrate cutting-edge LLM technology quickly and easily into your apps](https://github.com/microsoft/semantic-kernel)
- [Building agents and copilots with Semantic Kernel | Microsoft Learn](/semantic-kernel/agents/)
- [Understanding AI plugins in Semantic Kernel and beyond | Microsoft Learn](/semantic-kernel/agents/plugins/?tabs=Csharp)

**Key Takeaways:**

- Semantic Kernel is an opinionated open-source framework helping developers to build GenAI capabilities into their apps easier
- It’s being maintained and developed by Microsoft and used by first party teams to build Microsoft Copilot solutions.
- With set of samples, it helps you to start easily your GenAI journey inside your own application stack.
- Remember, this pattern can be combined with others based on your specific needs. For instance, you might want to pair this pattern with Pattern A and plug-in to a Microsoft Copilot, or Pattern F to surface your own copilot bot in Teams.

## Conclusion

We’ve started with scenarios and bought you to one or more patterns of interest, which we’ve collected into one of three Approaches. While each pattern has some variance, there are some common features for each Approach:

|  | Approach 1: Surface your data and services into Microsoft’s Copilots  |Approach 2: Create copilots anywhere with minimal coding and optional Microsoft data integration   |Approach 3: Full Control: Build Your Own (BYO) end-to-end AI experiences  |
|---------|---------|---------|---------|
|**Development effort**     |  Low (No/low code) |   Medium (Minimal code)            | High (Pro code)        |
|**Data sources**    |   Microsoft Graph (Microsoft/M365 or non-Microsoft via connectors)      |    Various. Power Platform connectors, Microsoft Graph, Your APIs.     |   Can span multiple data sources, service, and apps inside or outside of Microsoft tenant     |
|**User Interface or conversational surface** |  Provided by Copilot being extended, for example, Teams, Microsoft 365 etc.       | Varies per approach from Provided by Microsoft, to bring your own.        |  Bring your own. Multiple surfaces possible with same copilot    |
|**Influence over copilot’s tone, behavior, and model parameters**    |  No direct control. Model parameters are responsibility of Copilot being extended.       |  Some influence especially for behavior and tone, via custom instructions that form part of the metaprompt. With Teams AI Library, you can control model parameters.       |   Direct control of model parameters such as temperature, system prompts, max tokens etc.  Custom copilot behavior.      |
|**Multi-model capable**     |   No      |   No      |  Yes Multiple calls to diverse models within same flow   |
|**Model support**     |  Provided by system       |  Choice of OpenAI models       | Choice of any model OpenAI and full [model catalog](/azure/ai-studio/how-to/model-catalog) |
|**Responsible AI**    |    Provided by system     |   Either provided by system or leverageable options in each pattern      |  ISV responsibility with platform options in each pattern.       |
|**Support for chat history**     |  Provided by system       |  Either provided by system or leverageable options in each pattern       |  ISV responsibility with platform options in each pattern.       |
|**Example scenarios**     |  Users in **Microsoft Copilots** can perform actions on, or gain insights from ISV data and services.       |   Introduce a GenAI assistant in **existing ISV application** surface to reason over customer or ISV data. Present your own copilot or chatbot in an existing Microsoft surface, such as Teams, with a **separate identity** and experience to Microsoft Copilots.      |  Your customers and users interact with a **fully customized copilot to your brand and behavior**, which can reason over multiple data sets and connected systems from a multiple choice of UI or conversational surfaces.       |

These approaches are in order of increasing possibilities to customize, which also requires an ISV to pick up more responsibility via the control gained and increases the overall development effort.

We therefore highly recommend starting from Approach 1, which might well be the fastest way to market for your initial requirements. Microsoft is releasing new first party Copilots often. Continually check in to see if a new Copilot might address your users needs more efficiently by extending your data and services to it.

Move to Approaches 2 and then 3 gradually, as your requirements lead you to the need for more control and customization.

An exception here is perhaps where an ISV already has an existing AI capability in house with existing assets. For example, an ISV who already has a GenAI team with existing AIOps processes and already has IP created in say Python or LangChain, might be naturally better orientated to Approach 3.

A final key callout is that this list of patterns isn't exhaustive or mutually exclusive. We have curated here select patterns where we see synergies for ISVs and it’s important to understand they can be combined in various ways to create a solution that perfectly fits your needs. For instance, while working with Approach 3 (Patterns G or H), you might need a frontend. In this case, you could use plugins (Pattern A) or the Teams AI Library (Pattern F) along with it. Always consider the synergies between different patterns when planning your AI strategy.
