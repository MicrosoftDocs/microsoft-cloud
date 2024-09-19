---
title: Choosing an approach to AI-enabled application development with Capability Envisioning
description: "After identifying a top AI use case, Capability Envisioning helps you plan how to develop the application."
author: miglaros
ms.author: miglaros
ms.service: cloud-for-industries
ms.topic: tutorial 
ms.date: 09/16/2024

#CustomerIntent: As a <type of user>, I want <what?> so that <why?>.
---

# Choosing an approach to AI-enabled application development with Capability Envisioning

- [Introduction](#introduction)
- [Capability Envisioning](#capability-envisioning)
  - [Approach considerations](#considerations-for-your-approach)
    - [Example scenario](#example-scenario-contoso-shopping-application)
    - [Microsoft development tools](#microsoft-development-tools)
- [The ISV Journey](#the-approaches-across-the-isv-journey)

---

This article enables Independent Software Vendors (ISVs) to:

- Learn how to align selected AI use cases with an approach to application development
- Understand the ISV Journey and how it can help ISVs develop high-quality applications

## Introduction

After using the [business envisioning](Business-Envisioning.md) framework to prioritize your top generative AI use case, the next step is to plan how to build the solution.  

On this page, we guide you through choosing the best approach for executing your use case. We outline key considerations, provide resources to help you think through them, and explore the tools Microsoft offers for building your application based on your chosen approach. To help illustrate this process, we walk you through an example scenario, demonstrating how an ISV might evaluate their options and choose the most effective approach. We also provide an overview of the wider development journey, and how it can differ depending on the approach you select.

## Capability Envisioning

After selecting a prioritized use case, it's time to decide how to develop it. ISVs can choose from three main approaches: extend or adopt a Microsoft Copilot, build a custom copilot, or create an application on Fabric.

<!-- Graphic: 01_Approaches_Graphic 
Image description: There is a bar across the top of the image with Low-code on the left and Pro Code on the right with a line 1/3 of the way across the bar starting from the left. There are three boxes below this bar titled, from right to left: "Adopt/Extend a Microsoft Copilot, Build a custom Copilot, and Build an application on Fabric." Low-Code rests above the first of these boxes while pro-code rests above both of the second boxes. Within each of the boxes is the following text: 
Adopt/Extend: For ISVs developing solutions where end users perform work in Microsoft applications such as Teams, Word, or Outlook. This approach typically involves low- code development, templates, and rapid time-to-value.  
Buid a custom Copilot: For ISVs who aim to enrich their applications with Microsoft data and tools, or who want to create their own AI assistants with Azure. This approach can span from low-code to pro-code development, but typically makes use of the Microsoft Graph API, Copilot Studio plugins, and/or Azure OpenAI. 
Build an app on Fabric: For ISVs developing comprehensive core applications. Building on Fabric typically involves substantive pro-code development but empowers ISVs to manage data within Fabric Onelake and create highly customized workloads within the Fabric ecosystem.  
-->

These approaches are not mutually exclusive, but rather offer flexibility through broad spectrum of tools for developing your application. In the following section we explore how each approach fits into the development process, along with tools and considerations for choosing the best fit for your application. Our example scenario demonstrates the range of effort and resources involved in application development, while highlighting how your choice of approach can impact the development process.

### Considerations for your approach

To identify the best approach for development, we explore six primary considerations. These considerations help you evaluate the key features of the intended solution and make an informed decision on the approach that best aligns with your goals.

- **Data**: what types and sources of data and information does the application need to engage with?
- **Customization**: how should customers interact with the application and what control do you need over its outputs?
- **Development complexity**: how challenging is it to build the application, and are the necessary resources and expertise available?
- **End-user**: who is the end user and how much technical understanding do they have?
- **Business value**: how does this application provide customers with value and what is the profitability potential?
- **Risk and compliance**: what regulatory requirements and security concerns are relevant for this application?

The following graphic demonstrates how the answers to these questions can help you align with the different approaches to application development. If your use case has a limited scope and minimal need for customizability, extending an existing Microsoft Copilot using low-code tools may be the best approach. Alternatively, as we explore in a moment, a more complex and customizable solution might be better suited to building the application on Fabric.

<!-- Graphic: 02_Approaches_Considerations_Graphic 
Image Description: This is a table focused on considerations for each of the three approaches discussed on this page: Adopt/extend a Microsoft Copilot, Build a custom Copilot, and Build an app on Fabric-- these are the three data columns across the top. The consideration rows in the left most column, from top to bottom, are: Data, Customization, Development Complexity, End-user, Business Value, and Risk and compliance. The first four are technical considerations, the last two are business considerations
Here is the text from left to right for the data row: 
- Adopt/Extend: Multiple external data sources surfaced via plugin or connectors usually through Microsoft Graph
- Custom Copilot: Multiple data sources surfaced via API
- Fabric: Complex data infrastructure 
Here is the text from left to right for the customization row: 
- Adopt/Extend: Limited customizability, fully managed by Microsoft
- Custom Copilot: Moderate to full customizability and control
- Fabric: Full customizability, shared management with Microsoft
Here is the text from left to right for the development complexity row: 
- Adopt/Extend: No/low code
- Custom Copilot: Pro-code (moderate to complex)
- Fabric: Pro-code (complex)
Here is the text from left to right for the end-user row: 
- Adopt/Extend: Nontechnical end-user and/or works in a Microsoft product
- Custom Copilot: Nontechnical end-user and/or works in ISV software
- Fabric: Natural language and/or analytics use case targeting a technical or nontechnical user 
The business value row has one box spanning all of the approaches with the following text: "projected app profitability justifies the investment required to build it" 
The Risk and Compliance row has one box spanning all of the approaches with the following text: "Consideration for relevant regulatory requirements and protected information" 
-->

This use case was prioritized for development using the business, experience, technology framework to evaluate and compare its viability with other potential use cases. Explore how you can prioritize your own use cases in more detail [here](Business-Envisioning.md).

## Example scenario: Contoso Shopping application

In our scenario, Contoso partners with retailer AdventureWorks to develop a solution that provides a virtual storefront and systems to improve employee communication, inventory planning, and data insights across operations. Let's review how this use case aligns with our considerations for choosing a development approach.

### Considerations

#### Contoso store operations assistant

- **Data**: The application requires distributed data from a diverse set of sources, including non-Microsoft cloud applications, surfaced via APIs.
- **Customization**: The application is custom built for AdventureWorks with extensive customizability and control over individual features. The application needs to have multiple potentially complex components to support different stakeholders and tasks.
- **Development complexity**: Development of the application's capabilities require substantial resources, time, and human capital, including professional software developers.
- **End-user**: The end-user can vary across AdventureWorks operations but includes technical data scientists and nontechnical frontline workers.
- **Business value**: This application serves as a core feature of AdventureWorks operations by enabling a virtual storefront with substantive revenue potential along with data-driven insights on company operations. For Contoso, this offering represents a major business opportunity.
- **Risk and compliance**: This solution interacts with protected financial data to complete transactions for AdventureWorks customers, necessitating significant security and regulatory compliance components.

Contoso then used these considerations to assess how their development approach aligns with their overall strategy, business value, and technical capabilities, recognizing that alignment in these areas is critical for a successful application. Let's review how each of these considerations impacted their decision.

- **Strategy**: Given the use case prioritized in their business envisioning session, Contoso needed an application with expansive, and modular capabilities that could handle complex and highly customized functions. The Build on Fabric approach is ideal for this scenario because it helps Contoso teams learn new tools and technology, and the application needs features that require extensive development.
- **Business**: In addition to the larger revenue generating potential of this complex application, building the Contoso Shopping application on Fabric means Contoso can also templatize components of the application for reuse. With these templates, Contoso can accelerate future development efforts, potentially reducing costs and improving time to value.
- **Technology**: Lastly, and most obviously, the technical parameters of Contoso's use case and AdventureWorks' problems mean the Contoso Shopping application requires substantial pro-code development, along with more advanced data infrastructure and customization. Building an application on Fabric is the best path for Contoso to be sure they have the tools they need to execute on this use case.

In this scenario, Contoso decided to develop the Shopping application using pro-code given the need for extensive customization and the variety of complex features. Low-code development does not enable the capabilities identified in this use case. Within the pro-code journey, Contoso chose to develop this application from the ground up, on Fabric, to ensure a solid foundational data infrastructure on which to build its various features. They were able to make this decision confidently because this approach to development aligned with their use case strategically, commercially, and technically.

Let's also briefly examine why Contoso didn't choose the adopt/extend a Microsoft Copilot, or build your own copilot approaches. First, adopting and/or extending a Microsoft Copilot limits the application to a conversational assistant, which does not satisfy the requirements of the Contoso use case or meet AdventureWorks' needs. Similarly, while building a custom copilot enables greater customization and complex interactions, the technical data functionality required for a Shopping application extends beyond the capabilities of a copilot on its own. With these considerations, building an application on Fabric is the best choice for this scenario.

<!-- There is potential for additional links to learn pages for each of the development tools mentioned in the above and below paragraphs. These were not requested to be added, but if they should be, here are links for rapid access: 
-https://learn.microsoft.com/en-us/microsoft-365-copilot/extensibility/
-https://learn.microsoft.com/en-us/fabric/
-https://learn.microsoft.com/en-us/azure/ai-studio/what-is-ai-studio -->

Given the application's variety of interfaces and capabilities, Contoso ultimately used multiple tools, including Microsoft Fabric, Microsoft Azure, and Azure AI Studio, to complete development. It's important to note that the three approaches are not mutually exclusive. A custom copilot, built with low-code in Copilot Studio, using the Store Operations copilot template, could easily be integrated into this Shopping application. Selecting an approach is not a walled highway, it is a tool to help think through the requirements of your application, the capabilities you envision for it, and the resources you need to develop it. Let's dig a little bit deeper into how Microsoft solutions fit into these approaches.

## Microsoft Development Tools

Microsoft offers various development tools to meet ISVs' application development needs. Each of these approaches can involve a mixed array of individual Microsoft tools. We've summarized the main tools. You can also review a [detailed guide for ISVs on building GenAI experiences](isv-extensibility-story.md) that explains the possibilities of each approach.

<!-- Graphic: 03_Development_Tools_Graphic 
Image description: This graphic has five boxes above a long arrow along the bottom that goes from left to right with the word "Complexity" on it. The five boxes, from left to right, read: 
- Microsoft Teams: customize conversational AI assistants embedded within Microsoft Teams through Teams AI Library
- Copilot for Microsoft 365: Customize or build your own assistants that use data from integrated sources to enable dynamic conversational AI in natural language
- Copilot Studio: Design intelligent, actionable, and connected AI assistants for employees and customers on one connected platform
- Azure AI Studio: Build cutting-edge, market-ready AI applications with out-of-the-box and customizable APIs and models 
- Retail Data solutions in Microsoft Fabric: Innovate with AI on an end-to-end platform that empowers the ingestion, preparation, curation, and analysis of data-->

You can see how these solutions map to the approaches in the graphic that follows. We use the store operations application, which we compared to our Shopping application on the business envisioning page as an example use case.

<!-- Graphic: 04_Development_Tools_Example_Graphic 
Image description: Along the top line, from left to right, are the Microsoft Teams and Copilot for Microsoft 365 Logos, then a column "Adopt or Extend a Microsoft Copilot" then the Copilot Studio logo, then "Build a custom Copilot" then the Azure AI Studio logo, then "Build an application on Fabric" then the Microsoft Fabric logo. The logos are in circles between each of the three labeled columns. In the left column reads: "Store Operations Teams agent" which connects to "M365 Plug-ins" and "Copilot Studio" below it. Below these two are greyed out boxes for "Retail data solutions in Fabric" and "Azure". In the cetner column reads: "Store Operations AI Assistant" which connects to "Copilot Studio" and "Azure AI Studio" below it. Below these two are greyed out boxes for "Retail data solutions in Fabric" and "Azure".
Lastly, in the right column reads: "Custom store operations AI application" which connects farthest down to "Azure ML" with "Retail data solutions in Fabric" and "Azure" not greyed out.-->

The store operations use case involves an AI assistant that enables rapid access to store procedures, policies, and data in natural language. This use case could be developed using any of the three approaches, to different levels of complexity. An ISV could rapidly develop an assistant to fulfill this use case using the [Copilot Studio Store operations template](/microsoft-copilot-studio/template-store-ops). This effort would require minimal coding and involve interfacing at the surface of the stack, with M365 plug-ins or Copilot Studio, as shown in the left column.

Alternatively, an ISV could undertake a more complex development process and utilize Azure AI studio or Fabric to develop a more comprehensive store operations application, including data infrastructure and technical user assistants. This approach, as in the Shopping application scenario, would involve substantial pro-code development with developers interfacing with more of the full stack, as shown in the right column.

Your specific needs and circumstances determine the best development approach. These tools and methods support you regardless of how you develop your application, but the path varies based on your choice. You can use this ![template](media/ISV-capability-envisioning-template.pdf) to walk through the GenAI considerations for your use case and determine the best path forward.

:::image type="content" source="media/05_Blank_Template_Graphic.jpg" alt-text="Which generative AI approach should you pursue" border="false" :::

Now that you have identified and evaluated your use case and chosen the best development approach, let's look at the path ahead.

## The approaches across the ISV journey

The three approaches to building AI and GenAI applications on Microsoft tools can be split into two primary development journeys: a low-code journey and a pro-code journey. The low-code journey is characterized by rapid time to value and light application development lift while the pro-code journey powers more application customizability and complexity.

Our experience working with ISVs has led Microsoft to develop the ISV Journey Map, a consistent, and systematic process to develop applications for the Microsoft Cloud. This framework is designed to help you reduce costs, and efficiently develop the best possible solution. If you'd like to be evaluated using this framework, contact your partner development manager.

The ISV journey provides a broad overview of the different phases you'll go through to develop your application. Your chosen development approach will shape your experience in each phase, but the overall structure remains consistent across development lifecycles. This applies whether you are extending a Microsoft Copilot with low-code or building an application from scratch with pro-code.

:::image type="content" source="media/06_ISV_Journey_Graphic.jpg" alt-text="The ISV Journey. Below are two rectangles titled, top to bottom, Low-code and Pro-code." border="false" :::

Microsoft continues to build out content to provide holistic guidance for building AI and GenAI applications for the Microsoft Cloud. You can find more content and resources at [this Microsoft Copilot for ISVs Collection](/collections/7rmiyjy2006wj). This page is regularly updated with newly developed content.

## Next steps

Selecting the appropriate approach to your applications development is a critical stage in this process. Whether you need rapid time to value or complex customization, Microsoft is here to help you bring value to your customers. Once you've selected the right approach for your application, then comes the question of choosing the appropriate path for implementation. Each approach includes several potential tools, or patterns, you can use to develop your application. Here are pages where you can find more information about [low-code patterns](Low-Code-Patterns.md) and [pro-code patterns](Choosing-a-pro-code-pattern.md).

<!-- Immediately update this next steps section with links to pattern selection pages when they become available. When more ISV Journey pages have been built out, a table with the pages organized under each stage of the journey can also be added to the 'ISV Journey' section of this page. -->

## Related Content

- [Official Collection | Microsoft Copilot for an Independent Software Vendor (ISV)](/collections/7rmiyjy2006wj)
- [Creating Generative AI Experiences with the Microsoft Cloud: A Guide for ISVs](isv-extensibility-story.md)
- [Building AI solutions with partners: Empowering transformation with copilots | The Microsoft Cloud Blog](https://www.microsoft.com/microsoft-cloud/blog/2024/05/15/building-ai-solutions-with-partners-empowering-transformation-with-copilots/)

<!-- add additional links here as they are published -->
