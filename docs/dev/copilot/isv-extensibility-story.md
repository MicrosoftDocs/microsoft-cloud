---
title: ISV Extensibility for Microsoft Copilot
description: "This document explores the ISV extensibility story for Microsoft Copilot, and how ISVs can leverage the different aspects of the platform to create innovative and engaging experiences for users."
author: willstan
ms.author: eringuthrie
ms.service: cloud-for-industries
ms.topic: overview 
ms.date: 01/11/2024

#customer intent: As a developer, I want to understand how ISVs can leverage Microsoft Copilot to create value-added solutions for their customers.  

---

# What is the ISV Extensibility Story for Microsoft Copilot?

Microsoft 365 is a platform that enables productivity, collaboration, and intelligence for millions of users and organizations. Together with Azure AI and Microsoft Fabric, Microsoft Cloud offers a rich set of capabilities and services that can be customized and extended by independent software vendors (ISVs) to create value-added solutions for their customers.

:::image type="content" source="media/isv-extensibility-navigator-overview.png" alt-text="A flowchart detailing three steps to enhance Microsoft Copilot experiences with integration of ISV data, Microsoft Graph, Semantic Index, and Azure AI." border="false" :::

In this article, we will explore the ISVs extensibility story for Microsoft Copilot, and how ISVs can leverage the different aspects of the platform to create innovative and engaging experiences for users, as well as some guiding principles for ISVs to consider when choosing extensibility approach is best for their scenarios.  

Users work in many surfaces to achieve their organizational goals. As a result, the Microsoft Copilot ecosystem of extensibility tools was designed to provide flexibility to ISVs to reach users wherever they are, combining the right sources of data for users’ unique needs.

## Main Approaches for ISVs  

There are three main approaches for ISVs to connect with end users:

1. **Embed data and function into users’ everyday productivity work through Microsoft Copilot:** Leading Financial Service Industry (FSI) ISVs are building connectors to embed in M365 apps, allowing Finance users to easily surface up-to-date information embedded in their core work.

1. **Enhance ISV experiences in non-Microsoft surfaces using Graph data and Copilot Studio:** Business applications, such as HR tools or patient communication tools, can benefit from the rich data in Microsoft Graph and the M365 Semantic Index. For example, Contract Management ISVs are enhancing their customers’ enterprise contracting capabilities by converting existing legal documents into structured data. Now with Copilot Studio, ISVs building similar scenarios can bring in M365 Semantic Index data, making it easier than ever for end customers to pair their proprietary document data and insights with Generative AI copilots.

1. **Build new insights and/or experiences with Generative AI:** Data has historically lived in silos, but end users’ work requires bringing these sources together to run their organizations. ISVs can unify many data sources via Fabric, combined with Azure Open AI APIs, to unlock new insights or create their own new Gen AI applications. Manufacutring ISVs are building AI-powered assistants that will enable users to rapidly generate, optimize and debug complex automation code, and significantly shorten simulation times.

## Embed data and function into Microsoft Copilot via Apps

### Create connections to ISV apps and services from inside Microsoft Copilot

One of the ways that ISVs can extend Microsoft copilot is by creating plugins. Copilot plugins are extensions that augment the capabilities of Microsoft Copilot. For example, Copilot for Service uses Copilot Studio to extend business logic into customer-facing virtual agents’ responses and into human agent software, to generate answers quickly based on the organization’s unique context. They allow Copilot to interact with APIs from other software and services to retrieve up to date information, incorporate company and other business data, perform new types of computations, and safely act on the user's behalf. These plugins can be built by leading  ISVs  and enable users to interact directly with third-party apps and services, certified by Microsoft, from inside Copilot. They can be used to retrieve useful information, perform new computations, and execute actions. [Teams Message Extensions and Power Platform Connectors](/microsoft-365-copilot/extensibility/publish) can both work as plugins in Copilot, and new plugins will be built in Copilot Studio and published to Microsoft’s Copilot ecosystem via Partner Center. IT admins will have the ability to approve plugins for end users, and in turn, end users can enable which plugins are most relevant for their work in productivity apps.

:::image type="content" source="media/isv-extensibility-plugin.png" alt-text="ISV App sends data and functionality to Microsoft Copilot with arrows indicating flow from ISV Data to Microsoft Copilot." border="false" :::

### Make ISV data available for Microsoft Copilot experiences

Another way that ISVs can extend Microsoft Copilot is by using [Graph connectors](/graph/connecting-external-content-connectors-overview) to bring their data into the Microsoft 365 Semantic Index. The Microsoft 365 Semantic Index is a unified index of data and knowledge across Microsoft 365 and external line of business and third-party services that the Copilot reasons across when responding to Copilot users. Graph connectors are components that enable ISVs to connect their data sources to the Microsoft 365 Semantic Index, and make their data searchable, discoverable, and actionable for users. Graph connectors can be built using the Microsoft Graph connectors API, and can support various types of data sources, such as databases, file systems, web pages, enterprise applications, and more. Graph connectors can also enrich the data with AI-powered capabilities, such as natural language processing, entity extraction, and image analysis.

:::image type="content" source="media/isv-extensibility-plugin-graph.png" alt-text="ISV App sends data and functionality to Microsoft Graph with arrows from ISV Data to Microsoft Graph Data." border="false" :::

### Microsoft Fabric

ISVs can extend Microsoft copilot by extending their apps and data via plugins. ISVs can easily create plugins and API-end points for their data via [Microsoft Fabric](/fabric/get-started/microsoft-fabric-overview), a complete, unified, data platform, delivered as a SaaS product that brings together all the tools that organizations need to run their data estate. ISVs can store data and run key analytics in Fabric, such as user transaction history paired with data from operational databases like CosmosDB, to drive insights for their products. ISVs can also bring this data into Microsoft Copilots via Microsoft Copilot studio to create solutions that can provide users with personalized and secure experiences, and that can leverage the data and knowledge from Microsoft 365 and Microsoft Fabric.

:::image type="content" source="media/isv-extensibility-plugin-fabric.png" alt-text="ISV data, analytics, and APIs built in Fabric with arrows connecting ISV App, ISV Data, Microsoft Graph Data, and Fabric." border="false" :::

## Enhance users’ experiences into ISV surfaces, using Microsoft data and tools

### Office Semantic Index and Graph API

Another way ISVs can leverage the Microsoft Copilot Stack when building their own copilots is by accessing the Microsoft 365 Semantic Index directly through the [Graph APIs](/graph/use-the-api) and leveraging the knowledge and insights from Microsoft 365. The Microsoft 365 Semantic Index includes all the data and knowledge from Office applications, such as Word, Excel, PowerPoint, and Outlook. The Microsoft 365 Semantic Index contains information about the documents, emails, contacts, events, tasks, and more, that users create and use in Office. By connecting their products with Microsoft data, ISVs can enable their customers to infuse their experiences with enterprise-specific context, without having to build their own vector index and host Microsoft 365 customer content.

:::image type="content" source="media/isv-extensibility-pull-graph.png" alt-text="ISV pulls Microsoft Graph Data into its apps with arrows showing bidirectional flow between ISV Data and Microsoft Graph Data.":::

## Build new GenAI experiences and insights

### Apps with custom Copilots (built on Azure OpenAI) and integrating with Microsoft 365

ISVs can leverage the Microsoft Copilot Stack by creating their own copilots or intelligent assistants. They can also choose to integrate these new experiences with Teams using the [Teams AI library](/microsoftteams/platform/bots/how-to/teams%20conversational%20ai/teams-conversation-ai-overview) or publish to Teams through Copilot Studio. This library allows the ISVs to have their own copilots also surface in Teams as a bot. ISV apps can leverage LLMs to facilitate more natural conversational interactions with users in Teams, guiding that conversation into their app. ISVs can focus on writing their business logic, and publish bots in Teams to handle the complexities of conversational bots so that you can easily extract and utilize user intent within their apps.

:::image type="content" source="media/isv-extensibility-build-your-own.png" alt-text="Integration of ISV App with Microsoft Copilot and Microsoft Graph Data using Azure AI Studio and Copilot Studio.":::

## Conclusion

Microsoft Cloud is a platform that offers a rich set of capabilities and services that can be customized and extended by ISVs to create value-added solutions for their customers. Across almost every industry, our ISV partners are putting Generative AI to work on complex topics like data exchange in financial services, code generation and debugging in manufacturing simulation, patient data analysis in healthcare, or consumer shopping insights in retail.

ISVs can tap into a spectrum of extensibility options to achieve their goals, whether it is to reach millions of users, enrich their existing apps, or completely reimagine how work is done by creating new experiences and insights built with GenAI. They can publish these in their own ecosystems as well as in a wide number of Microsoft surfaces, whether Teams, M365, or all of Microsoft Copilot.  

:::image type="content" source="media/isv-extensibility-conclusion.png" alt-text="Three-step flowchart showing Copilot experiences goals and surfaces, including embedding ISV data into Copilot, bringing data into ISV surfaces with Copilot Studio, and building new Generative AI experiences.":::
