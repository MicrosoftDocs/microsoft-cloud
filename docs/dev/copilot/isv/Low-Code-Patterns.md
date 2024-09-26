---
title: "Choosing the best low-code pattern to create your generative AI solution"
description: Guidance for choosing the most applicable pattern to help ISVs in their journey to build low-code AI solutions 
author: miglaros
ms.author: miglaros
ms.service: cloud-for-industries
ms.topic: tutorial 
ms.date: 09/16/2024

#CustomerIntent: As an ISV, I want build an generative AI app with low-code and need to know which Microsoft Cloud tools to use.
---

# Choosing the best low-code pattern to create your generative AI solution

## Overview

To help Independent Software Vendors (ISVs) choose the best way to build their generative AI solution, Microsoft created guidance on the benefits of low code and pro code options.

Within this low-code journey, there is one main approach: extending a copilot. This approach is composed of multiple patterns, which are specific paths you can take to building your solution.

This page helps you choose the best pattern for your use case if you have already chosen a low-code journey based on your needs and capabilities. If you haven't chosen a journey yet, visit the [capability envisioning page](Capability-Envisioning.md) to find the best approach for your use case.

## Considerations for a low-code journey

The low-code approach is recommended for ISVs looking to quickly develop solutions, especially when the goal is for end users to perform work in Microsoft applications - such as Teams, Word, or Outlook. This approach typically involves limited code development, the utilization of templates, and a more rapid time-to-value than pro-code approaches.  

The low-code approach may be the best option for you if you:
*Need a quick turnaround
*Have limited resources to devote to software development and coding professionals
*Want to integrate your solution with Microsoft 365 productivity tools

For full customization and ongoing control, a [pro-code journey](Choosing-a-pro-code-pattern.md) might be better, even though it is more complex.

If you think the low-code approach is best for your use case, the pattern options in this article will help you to find the best tools for your needs.

There are three pattern options within the low-code approach:
*Create plugins to enhance an existing Copilot's functionality
*Use Microsoft Graph connectors to bring in your data to Copilot experiences
*Use Power Platform connectors to give end users an opportunity to customize their copilot experience

### Pattern selection in the ISV Journey

Within each approach, choosing a pattern is the last step an ISV takes before beginning to build a solution.

The pattern you select:

* __Impacts the capabilities of your solution.__ Choosing the correct pattern for your situation enables you to match your solution to your customers' needs. Select the pattern with capabilities which align to your intended outcome  

* __Affects the developmental cost of the project.__ While these low code patterns deliver rapid results, some may require more lift during development. The investment of time and money needed should not outweigh the potential value to your use case

* __Enables you to work within different interfaces.__ Some patterns are designed to integrate with existing applications or platforms, while others are intended to serve as building blocks for new software

* __Changes data, infrastructure, and other back-end considerations.__ If you are using multiple external data sources surfaced via plugin or connectors, ensure that the plugins and connectors you choose can handle the volume of your data. Start with what's available in the [Microsoft Graph connectors gallery](/microsoftsearch/connectors-gallery), but more data-heavy solutions might need some code changes and back-end design to meet your needs.

### Multiple pattern options

Some ISVs may choose to follow multiple patterns, either to build multiple solutions or to integrate capabilities from multiple patterns into a single solution.

Each pattern offers unique capabilities to address different aspects of AI support, and utilizing multiple options can empower you to create a robust and comprehensive generative AI application.  Examples of composite patterns range from data integration and automation to advanced reporting and AI insights.

Some ISVs even combine these low-code patterns with the pro-code approach to give customers multiple opportunities to engage with the software at varying altitudes. Combining both approaches can allow you to take advantage of the ease of use and quick deployment of low code along with the flexibility and power of pro-code approaches.

Whether you choose one pattern or combine options, it's important to consider the situation you are in and choose the platform that works best for you.

## Pattern A: Create Plugins

ISVs looking to surface their existing services, data, and processes into Microsoft's Copilots or Microsoft 365 applications can do so by building plugins and connectors.

ISVs can create plugins using various tools, including Power Platform plugins through Copilot Studio and Teams Message Extensions. New plugins can be published to Microsoft's Copilot ecosystem via Partner Center, where IT admins can approve them for use by end users. This approach enables Microsoft 365 Copilot to interact with APIs from other software and services, surface up-to-date information, execute actions, and perform new types of computations.

You might be interested in this option if you:

* Want to bring your apps or services to Microsoft 365 with Microsoft Copilot
* Prefer to use tools like Teams Message Extension and Copilot Studio plugins
* Need to increase your solution's visibility and discoverability through Partner Center
The key benefits of this pattern are:
* Streamlining user experiences through continuity across Microsoft 365 apps, eliminating the need for users to navigate between multiple apps
* Increasing visibility for your solution service by meeting users where they already work

### Scenario

Let's imagine the hypothetical software vendor called Contoso that specializes in generative AI solutions. In this scenario, Contoso and AdventureWorks have teamed up to quickly create a solution for their virtual storefront challenges. AdventureWorks' existing systems force employees to context switch across multiple platforms, resulting in disjointed communication and lapses in inventory management. AdventureWorks wants to improve their overall insights to promote ongoing growth.

To address these challenges, Contoso utilizes the Store Operations copilot template from Copilot Studio to create an AI assistant in AdventureWorks' existing shopping application. This plugin integrates store procedures, policies, and data into the shopping app quickly and with minimal use of resources.

Powered by AI, the assistant helps:

* Enhance employee communication by providing contextual suggestions and other relevant information from store procedures and policies in the apps where employees are already working
* Optimize inventory planning with automated alerts when stock reaches certain thresholds
* Reveal data insights by analyzing past sales performance and integrating this information into a unified view

Without having to devote significant resources, AdventureWorks was able to find a solution to their challenges. The AI assistant created by Contoso empowered them to maximize efficiency in their virtual storefront and benefit from improved insights and streamlined data.

## Pattern B: Power Platform Connectors

Creating plugins is great if you want to surface your data in Microsoft 365 applications. Copilot Studio and Power Platform connectors can effectively add copilot functionality to your app. You can create Power Platform connectors in Copilot Studio to allow copilot to retrieve data from many sources including your applications. By creating these connectors, you can give your end users an opportunity to have a copilot experience based on your data and services.

You might be interested in this pattern if you:

* Have end-users interested in using Microsoft Copilot's capabilities within your application
* Require a lower coding lift but still want to create a copilot experience
* Want a copilot that can access a wide variety of data sources

The key benefits of this pattern are:

* Enriching your application with the power of existing Microsoft and non-Microsoft connectors
* Expediting plugin development through the low-code capabilities of Copilot Studio

### Power Platform Connector Scenario

Contoso is partnering with AdventureWorks again because their increase in online traffic and sales last year has caused customer service challenges. They are struggling to keep up with the surge in customer inquiries and want to quickly create a specialized copilot to help manage customer service on their website.

To address these requirements efficiently, Contoso decides to equip AdventureWorks with Microsoft Copilot in their existing application using Copilot Studio and Power Platform connectors.

The customized copilot is able to:

* Integrate with AdventureWork's product database to provide real-time status updates, and assist with returns
* Connect data from their CRM, sales, and inventory to keep management informed in a streamlined manner

Since Contoso already had the data sources available, they easily created this solution using Copilot Studio and Power Platform connectors to create the right copilot experience for their client's needs.

## Pattern C: Microsoft Graph connectors

Microsoft Graph Connectors facilitate the integration of external data from various sources into Microsoft 365, providing a unified and secure experience for users. These connectors enhance data accessibility, streamline development, and improve the contextual relevance of AI-generated content, leading to more powerful and integrated solutions.

You might be interested in this pattern if you:

* Need a way to integrate your enterprise application or other on-premises and SaaS cloud software with Microsoft 365 productivity tools.
* Want to enable your end-users working in Microsoft 365 to draw insights from your data sources combined with user centric Microsoft Graph data.
The key benefits of this pattern are:
* Enabling the already existing Microsoft 365 client application user base to access your data and services, showcasing your offerings and potentially increasing your own user base
* Enriching insights and achieving universal integration with Microsoft 365 apps by combining ISV data with Microsoft Graph data

### Microsoft Graph Connector Scenario

Contoso is working to create a solution for one of their larger clients, a multinational corporation. The client is dealing with challenges in managing and collaboration on documents across departments in offices around the globe. They use Microsoft 365 productivity applications but need a more streamlined and intelligent system for document management and to facilitate better collaboration.

To meet this need, Contoso uses Microsoft Graph connectors and Microsoft Copilot's AI capabilities to seamlessly integrate the client's various data sources into Microsoft 365 applications for centralized access and analysis.

This solution helps enhance the corporation's workflows by

* Tagging documents automatically based on context pulled from SharePoint to organize files across the global organization
* Suggesting optimal times for meetings or document reviews by integrating directly with Outlook for insights across teams and time-zones
* Utilizing Microsoft Copilot's smart summarization to help the teams quickly understand key points for improved collaboration and decision-making

By leveraging Microsoft Graph connectors for data integration and Microsoft Copilot's AI capabilities for enhanced functionality and analysis, Contoso was able to develop a comprehensive solution to the client's challenges. This solution improved their operational efficiency and gave them the tools to facilitate better teamwork and information sharing.

### Conclusion

Using one of these pattern options will help you start developing your generative AI solution with low-code. If these patterns don't provide the capabilities needed for your intended use case, you may want to explore [pro-code patters](Choosing-a-pro-code-pattern.md) for more control to customize your application.

Check out these resources to learn about the tools for your chosen low-code pattern and the next steps for activation and monetization after building your generative AI experience.

* [Build message extensions for Microsoft Copilot for Microsoft 365](/microsoft-365-copilot/extensibility/overview-message-extension-bot)
* [Build Microsoft Graph connectors for Microsoft Copilot for Microsoft 365](/microsoft-365-copilot/extensibility/overview-graph-connector)
* [Embed a Power Virtual Agents control using chatbot control](/power-platform/release-plan/2023wave1/power-apps/maker-embed-pva-control-power-apps-via-chatbot-control)

## Related links

[Creating Generative AI Experiences with the Microsoft Cloud - A Guide for ISVs | Microsoft Learn](isv-extensibility-story.md)
[Plugin architecture - Microsoft Copilot Studio | Microsoft Learn](/microsoft-copilot-studio/copilot-plugins-architecture)
[Build and publish with ISV Success - Partner Center | Microsoft Learn](/partner-center/membership/isv-success)