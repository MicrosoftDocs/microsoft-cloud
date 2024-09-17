---
title: "Choosing the best low-code pattern to create your generative AI solution"
description: Guidance for choosing the most applicable pattern to use in the low-code ISV journey 
author: miglaros
ms.author: miglaros
ms.service: cloud-for-industries
ms.topic: tutorial 
ms.date: 09/16/2024

#CustomerIntent: As an ISV, I want build an generative AI app with low-code and need to know which Microsoft Cloud tools to use.
---

<!--
Remove all the comments in this template before you sign-off or merge to the main branch.

-->

<!-- 1. H1 -----------------------------------------------------------------------------

-->

# Choosing the best low-code pattern to create your generative AI solution 

<!-- 2. Introductory paragraph ----------------------------------------------------------

Required: Lead with a light intro that describes, in customer-friendly language, what common scenario the 
customer will accomplish in the Tutorial. Answer the fundamental “why would I want to do this?” question. Keep it short.

Readers should have a clear idea of what they will do in this article after reading the introduction.

* Introduction immediately follows the H1 text.
* Introduction section should be between 1-3 paragraphs.
* Don’t link away from the article to other content.
* Don't use a bulleted list of article H2 sections.

Example: Azure Monitor alerts proactively notify you when important conditions are found in your monitoring data. Metric alert rules create an alert when a metric value from an Azure resource exceeds a threshold.

-->

In this article:
* Considerations for the low-code approach 
  * Pattern selection in the ISV Journey
  * Multiple pattern option
* Explore each pattern 
  * Pattern A: Create Plugins
  * Pattern B: Power Platform connectors
  * Pattern C: Graph connectors

To help ISVs select the best path to building their generative AI solution, Microsoft has created guidance outlining the unique benefits of low code and pro code journeys. 

Within this low-code journey, there is one main approach: extending a copilot. This approach is comprised of multiple patterns, which are specific paths you can take to building your solution. 

This page focuses on helping you choose the best pattern for your use case if you have already elected for a low-code journey based on your specific needs and capabilities. If you haven’t yet selected a journey, visit the capability envisioning page <!--[link to capability page once published]--> to help determine which approach may be best suited to your use case.

## Considerations for a low-code journey
The low-code approach is recommended for ISVs looking to quickly develop solutions, especially when the goal is for end users to perform work in Microsoft applications - such as Teams, Word, or Outlook. This approach typically involves limited code development, the utilization of templates, and a more rapid time-to-value than pro-code approaches.  

The low-code approach may be the best option for you if you: 
*Need a quick turnaround
*Have limited resources to devote to software development and coding professionals
*Want to integrate your solution with Microsoft 365 productivity tools

Those seeking full customization and ongoing control may be better served by a pro-code journey that can allow for greater control at the expense of increased complexity <!--[link to pro code pattens page once published].-->

If you’ve identified that the low-code approach may be best suited to your use case, dive further into the pattern options below to determine which tools will serve your purpose best.

There are three pattern options within the low-code approach:
*Create plugins to enhance an existing Copilot’s functionality
*Use Microsoft Graph connectors to bring in your data to Copilot experiences 
*Leverage Power Platform connectors to give end users an opportunity to customize their copilot experience 

### Pattern selection in the ISV Journey

Within each approach, choosing a pattern is the last step an ISV takes before beginning to build a solution.

The pattern you select:
* __Impacts the capabilities of your solution.__ Choosing the correct pattern for your situation will enable you to match your solution to your customers’ needs. Select the pattern with capabilities which align to your intended outcome  

* __Affects the developmental cost of the project.__ While these low code patterns deliver rapid results, some may require more lift during development. The investment of time and money needed should not outweigh the potential value to your use case 

* __Enables you to work within different interfaces.__ Some patterns are designed to integrate with existing applications or platforms, while others are intended to serve as building blocks for new software

* __Changes data, infrastructure, and other back-end considerations.__ If you are using multiple external data sources surfaced via plugin or connectors, ensure that the plugins and connectors you choose can handle the volume of your data. Begin with what is already available in the [Microsoft Graph connectors gallery](https://learn.microsoft.com/en-us/microsoftsearch/connectors-gallery), though more data heavy solutions may entail some code modification and back-end design to meet your specifications.

###  Multiple pattern options

Some ISVs may choose to follow multiple patterns, either to build multiple solutions or to integrate capabilities from multiple patterns into a single solution. 

Each pattern offers unique capabilities to address different aspects of AI support, and utilizing multiple options can empower you to create a robust and comprehensive generative AI application.  Examples of this could range from data integration and automation to advanced reporting and AI insights.

Some ISVs even combine these low-code patterns with the pro-code approach to give customers multiple opportunities to engage with the software at varying altitudes. Combining both approaches can allow you to take advantage of the ease of use and quick deployment of low code along with the flexibility and power of pro-code approaches. 

Whether you choose one pattern or combine options, it’s important to consider the situation you are in and choose the platform that works best for you. 

## Pattern A: Create Plugins

ISVs looking to surface their existing services, data, and processes into Microsoft's Copilots or Microsoft 365 applications can do so by building plugins and connectors.

ISVs can create plugins using various tools, including Power Platform plugins through Copilot Studio and Teams Message Extensions  . New plugins can be published to Microsoft’s Copilot ecosystem via Partner Center, where IT admins can approve them for use by end users. This approach enables Microsoft 365 Copilot to interact with APIs from other software and services, surface up-to-date information, execute actions, and perform new types of computations.

You might be interested in this option if you:

* Want to bring your apps or services to Microsoft 365 in connection with Microsoft Copilot
*	Prefer to use tools like Teams Message Extension and Copilot Studio plugins
*	Need to increase your solution’s visibility and discoverability through Partner Center 
The key benefits of this pattern are:
*	Streamlining user experiences through continuity across Microsoft 365 apps, eliminating the need for users to navigate between multiple apps
*	Increasing visibility for your solution service by meeting users where they already work 

#### Scenario
Let’s imagine the hypothetical software vendor called Contoso who specialize in generative AI solutions. In this scenario, Contoso has partnered with retailer AdventureWorks to rapidly develop a solution for the challenges they face in their virtual storefront. AdventureWorks’ existing systems force employees to context switch across multiple platforms, resulting in disjointed communication and lapses in inventory management. AdventureWorks wants to improve their overall insights to promote ongoing growth.

To address these challenges, Contoso utilizes the Store Operations copilot template from Copilot Studio to create an AI assistant in AdventureWorks’ existing shopping application. This plugin integrates store procedures, policies, and data into the shopping app quickly and with minimal use of resources. 

Powered by AI, the assistant helps: 
*	Enhance employee communication by providing contextual suggestions and other relevant information from store procedures and policies in the apps where employees are already working
*	Optimize inventory planning with automated alerts when stock reaches certain thresholds
*	Reveal data insights by analyzing past sales performance and integrating this information into a unified view 

Without having to devote significant resources, AdventureWorks was able to find a solution to their challenges. The AI assistant created by Contoso empowered them to maximize efficiency in their virtual storefront and benefit from improved insights and streamlined data. 

## Pattern B: Power Platform connectors

Creating plugins is great if you want to surface your app in Microsoft 365 applications, but if you want to go the other direction and surface copilot in your app you’ll want to use Power Platform connectors. You can create Power Platform connectors in Copilot Studio to allow copilot to retrieve data from a number of sources including your applications. By creating these connectors, you can give your end users an opportunity to have a copilot experience based on your data and services. 

You might be interested in this pattern if you:
*	Have end-users interested in using Microsoft Copilot’s capabilities within your application
*	Require a lower coding lift but still want to create a copilot experience
*	Want a copilot that can access a wide variety of data sources

The key benefits of this pattern are: 
*	Enriching your application with the power of existing Microsoft and non-Microsoft connectors 
*	Expediting plugin development through the low-code capabilities of Copilot Studio

#### Scenario:
Contoso is working with AdventureWorks again since they have seen a significant increase in online traffic and sales in the last year leading to several challenges with customer service. They are struggling to keep up with the surge in customer inquiries and want to quickly create a specialized copilot to help manage customer service on their website.

To enable them to do this efficiently and effectively, Contoso decides to equip AdventureWorks with the power of Microsoft Copilot in their already existing application using Copilot Studio and Power Platform connectors. 

The customized copilot is able to:
*	Integrate with AdventureWork’s system and product database to handle common customer inquires, provide real time order status updates, and assist with returns
*	Connect data from their CRM, sales, and inventory to keep management informed in a streamlined manner

Since Contoso already had the data sources available, they easily created this solution using Copilot Studio and Power Platform connectors to create the right copilot experience for their client’s needs. 

## Pattern C: Microsoft Graph connectors

Microsoft Graph Connectors facilitate the integration of external data from various sources into Microsoft 365, providing a unified and secure experience for users. These connectors enhance data accessibility, streamline development, and improve the contextual relevance of AI-generated content, leading to more powerful and integrated solutions.

You might be interested in this pattern if you:
*	Need a way to integrate your enterprise application or other on-premises and SaaS cloud software with Microsoft 365 productivity tools. 
*	Want to enable your end-users working in Microsoft 365 to draw insights from your data sources combined with user centric Microsoft Graph data.
The key benefits of this pattern are: 
*	Enabling the already existing Microsoft 365 client application user base to access your data and services, showcasing your offerings and potentially increasing your own user base
*	Enriching insights and achieving universal integration with Microsoft 365 apps by combining ISV data with Microsoft Graph data

#### Scenario
Contoso is working to create a solution for one of their larger clients, a multinational corporation. The client is dealing with challenges in managing and collaboration on documents across departments in offices around the globe. They use Microsoft 365 productivity applications but need a more streamlined and intelligent system for document management and to facilitate better collaboration.

To meet this need, Contoso leverages Microsoft Graph connectors and Microsoft Copilot’s AI capabilities to seamlessly integrate the client’s various data sources into Microsoft 365 applications for centralized access and analysis.

This solution helps enhance the corporation's workflows by
*	Tagging documents automatically based on context pulled from SharePoint to organize files across the global organization 
*	Suggesting optimal times for meetings or document reviews by integrating directly with Outlook for insights across teams and time-zones
*	Utilizing Microsoft Copilot’s smart summarization to help the teams quickly understand key points for improved collaboration and decision-making

By leveraging Microsoft Graph connectors for data integration and Microsoft Copilot’s AI capabilities for enhanced functionality and analysis, Contoso was able to develop a comprehensive solution to the client’s challenges. This solution improved their operational efficiency and gave them the tools to facilitate better teamwork and information sharing.

### Conclusion
Leveraging one of these pattern options will equip you to begin developing your generative AI solution with low-code. If these patterns don’t provide the capabilities needed for your intended use case, you may want to explore pro-code patters <!--[link to pro-code patterns page once published]--> that offer more control to customize your application. 

Explore the resources below to learn more about the tools needed for your chosen low-code pattern as well as next steps for activation and monetization once you’ve built your generative AI experience. 
*	[Build message extensions for Microsoft Copilot for Microsoft 365](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/overview-message-extension-bot) 
*	[Build Microsoft Graph connectors for Microsoft Copilot for Microsoft 365](https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/overview-graph-connector)
*	[Embed a Power Virtual Agents control using chatbot control](https://learn.microsoft.com/en-us/power-platform/release-plan/2023wave1/power-apps/maker-embed-pva-control-power-apps-via-chatbot-control) 

## Related links
[Creating Generative AI Experiences with the Microsoft Cloud - A Guide for ISVs | Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-cloud/dev/copilot/isv-extensibility-story?ns-enrollment-type=Collection&ns-enrollment-id=7rmiyjy2006wj)
[Plugin architecture - Microsoft Copilot Studio | Microsoft Learn](https://learn.microsoft.com/en-us/microsoft-copilot-studio/copilot-plugins-architecture)
[Build and publish with ISV Success - Partner Center | Microsoft Learn](https://learn.microsoft.com/en-us/partner-center/membership/isv-success)

<!--
Remove or replace all the comments in this template before you sign-off or merge to the main branch.
-->
