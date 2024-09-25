---
title: Developing a commercial strategy for generative AI applications
description: This article helps you decide your application's commercialization strategy by giving an overview of important factors and real-world examples.   
author: miglaros
ms.author: miglaros
ms.service: cloud-for-industries
ms.topic: overview #Required; leave this attribute/value as-is.
ms.date: 09/24/2024

#CustomerIntent: As a <type of user>, I want <what?> so that <why?>.
---

# Developing a commercial strategy for generative AI applications

---

This article enables Independent Software Vendors (ISVs) and Software Development Companies (SDCs) to:

- Learn about different approaches to commercial strategy for generative AI applications
- Understand how other ISVs successfully designed and implemented a commercial strategy for their applications

## Introduction

As ISVs consider generative AI use cases and how to develop them, it's critical to think about the commercial strategy of your solution from the very beginning. Aligning your development and commercial approaches from the outset can help you establish a more cohesive, and successful overall product strategy.

This article is intended to help you determine your application's commercialization strategy by providing a high-level view of the considerations that can impact your commercial model and real-world examples of commercial applications. This commercialization page can be useful as you evaluate use cases with the [business envisioning](Business-Envisioning.md) page and select a technical approach in the [capability envisioning](Capability-Envisioning.md) page. While there's no one-size-fits-all approach to commercialization, understanding the strategies that successfully worked for others can help guide you in successfully monetizing your investments.

Let's walk through some initial challenges and considerations that your commercial strategy must address.

## Commercialization challenges and considerations

When you align on the commercial strategy and monetization approach for your solution, there are several challenges to navigate:

- **Volatile demand and value perceptions** that can shift quickly as new products are brought to market
- **Highly variable and dynamic costs**, including development, deployment, and hosting costs
- **Market differentiation** that aligns firm profitability with customer affordability and keeps a path open for future scalability

As with any solution an ISV builds, it's important to establish a clear business plan and understand the potential revenue opportunity. These initial steps help ensure clear value alignment and drive a successful approach to commercialization.

To plan a successful commercial strategy, it can be helpful to examine your solution and context through multiple lenses. As you think about your solution's commercial strategy, there are three primary things to consider: value determination, costs, and your go-to-market approach. We discuss each of these in-depth later using a series of ISV example scenarios. The following considerations can help you think through the factors that go into choosing your commercialization strategy.  

:::image type="content" source="media/01_Consideration_Questions_Graphic.png" alt-text="a table with three columns, from left to right: focus area, consideration, and ask yourself" border="false" :::

Thinking through these considerations can help you address the challenges that many commercial efforts face. Let's briefly discuss how each of these focus areas can impact your commercial strategy.

### Value determination

As you develop your commercialization strategy, it's critical to identify the value your application creates for you and your customer. Consider your objective as you identify your use case and build your application. Are you building brand awareness in a new target market or exploring a supplemental revenue opportunity in an existing audience? Having a clear goal and success metric for your solution can help align your commercial strategy with your application. Through our conversations with ISVs, we find that many ISVs seek to achieve the following value from their offerings:  

- **Revenue** - the solution is offered at a charge to customers and intended to generate ISV revenue streams
- **Market share** - the solution is intended to build brand awareness and increase ISV market share in a specific area

While solutions that focus on generating revenue provide obvious benefit for ISVs, there's still value in developing applications with the principal intention of expanding market share and brand awareness. For example, an ISV entering a new market may want to publish an initial series of solutions or features for free to generate interest and trust in their brand before a larger, revenue-focused release. However, regardless of your objective, aligning your solution with customer demands is critical for a successful commercial strategy.

#### Consider customer demand

Whether you're focusing on increasing market share or driving revenue, ISV solutions perform better with customers when they provide a clear value proposition to the customer. To effectively decide on a monetization and pricing strategy, it's essential to evaluate how much value your solution holds to your customer.

Consider how your solution helps the customer-what value does it provide for them? Typically, solutions provide value to customers in at least one of three ways:

- **Increase effectiveness** - enable customers with new capabilities that were previously not available
- **Improve efficiency** - reduce the time, labor, or resources required to complete tasks or operations  
- **Generate revenue** - generate revenue directly by allowing your customer to market and/or monetize their own products

If your end customers find your solution valuable, they may be willing to pay more for the product. Solutions that enable customers to generate revenue can often be more in-demand for customers than solutions that support internal, non-revenue-generating functions. However, solutions that improve the efficiency and effectiveness of their customers' operations can still be successful, as we see in the following example scenarios.

This page and the wider journey are meant to provide ISVs with step-by-step guidance to ideate, develop, and publish new applications. However, there are other Microsoft resources available that can help you drive organizational buy-in around your use case. The [Microsoft AI Value Accelerator (MAIVA) Playbook](https://microsoft.github.io/dstoolkit-maiva/) provides foundational guidance to enterprise customers for industrializing AI. This resource can help you align your use case with overall business goals, standardize your AI strategy, and effectively scale the value your solution provides.

### Costs

Accurately determining the costs of developing and operating a generative AI application is challenging because of the diversity of variables at play. Individual applications involve different tools and architectures in their development, creating a wide variety of possible initial and ongoing costs for solutions. However, there are tools that can help you estimate these costs. You can use the [Azure pricing calculator](https://azure.microsoft.com/pricing/calculator/) to input your own information and receive a customized pricing estimate online. You can also find out more about costs associated with specific development tools such as the [services within Azure AI Studio](/azure/ai-studio/how-to/costs-plan-manage) and [Copilot Studio](/microsoft-copilot-studio/requirements-quotas).

Generally, as you assess the costs involved with your application, there are multiple elements to plan for. Generative AI models typically require up to six primary components:

- **Database or storage**: Managing costs for storing your application's data is crucial, especially when handling both structured and unstructured data across multiple locations to ensure availability and redundancy.
- **Index and search engine**: Investing in an efficient index and search engine is essential to making your data accessible and useful, and enabling users to quickly retrieve the information they need.
- **Model development and training**: Budgeting for the development, training, and refinement of your AI or machine learning models is critical, as these models form the foundation of your application and influence its effectiveness.
- **Application hosting environment**: Selecting a cost-effective hosting environment, whether it's Azure Kubernetes Service (AKS), Virtual Machines (VMs), or AppService, is vital, as it directly impacts your application's performance, scalability, and security.
- **Cache**: Implementing caching solutions, though technically optional, can lower operational costs by reducing database strain and improving response times.
- **Potential security or monitoring service**: Prioritizing expenditures on security and monitoring services is crucial to safeguarding your application. It helps ensure compliance, and maintain optimal performance, preventing costly disruptions or breaches.

It's important to remember that your application development process and internal functions can heavily impact costs. Thinking about your application holistically and considering all relevant factors can help drive a successful commercial strategy.

### Go-to-market (GTM) approaches

After considering the value your solution provides, along with the costs involved with its development, deployment, and operation, you're ready to determine the approach that fits your scenario. Through our work with ISVs, we've identified four primary go-to-market approaches for generative AI applications. While not mutually exclusive or comprehensive, these approaches provide a tangible representation of strategies successfully applied by ISVs.

- **Free** - the solution is offered to end-users freely, either through a stand-alone download, or as an update to an existing free solution.
- **Freemium** - the solution includes a free base product with extra, widely applicable features through a paid premium version or license.
- **Paid Feature** - the solution is a separate feature on a larger platform or solution with its own charge but is narrowly applicable to a select user base with specific needs.
- **New Solution** - the solution offers clear value to the customer and reaches the level of sophistication that warrants it being monetized and sold as a separate, new solution. This solution can potentially serve as a platform for future offerings delivered through the other approaches.

When choosing your go-to-market strategy, it's important to weigh the cost and benefits of each approach against the investment required and the value of your desired outcome. For example, if you're thinking about developing a free solution, you need to fully consider the solution's purpose and benefits for it to pay off. To that end, our experience with ISVs suggests that aligning to one of the following approaches can help drive a successful and consistent commercial strategy. Here is a summary of these approaches:

:::image type="content" source="media/02_GTM_Approaches_Table_Graphic.png" alt-text="goal of your solution, target audience, intended value for the end-user, ISV investment" border="false" :::

To explain these scenarios, we examine several examples of applications using these go-to-market approaches, including how they selected their commercial strategy, and the benefits they experienced from using this approach. To thoroughly examine each of these case studies, we use the set of considerations we discussed earlier:

- **End user** - the personas that use the solution
- **Business value** - how the solution provides value to customers
- **Technical approach**  - how the solution is built
- **Costs** - the initial and ongoing costs of running the solution

Let's look at how real-world ISVs successfully put these go-to-market approaches into action for their AI and generative AI solutions.

## Commercialization example scenarios

ISVs can adopt various commercial strategies for their solutions, depending on their product's strategic goals. Based on our experiences with ISVs, we've put together four generalized scenarios to explain each GTM strategy. For the purposes of this article, we refer to our example ISV as Contoso, a fictional software developer.

### Free scenario: Invoice management copilot feature

Contoso, seeking to expand the reach of their data management software, conducted a survey to identify a need they could solve to attract their target audience. They learned that many of their executive customers were frustrated with the amount of time lost to convoluted administrative processes like invoice approvals. From an initial email notification, it could take up to a few days for decision makers to just approve an invoice for payment. This involved locating the required information to confirm across multiple departments if contract terms were fulfilled.

To drive demand for their core application among potential customers, Contoso built a copilot feature that helps improve efficiency within the customers' administrative operations. They designed this copilot feature to help reduce time that decision makers spend approving invoices for payments and focuses on the interface where users collaborate the most: Microsoft Teams. Contoso created this copilot to complement its larger data management program and showcase the full capabilities of the generative AI solution. Contoso offered this copilot feature from their overall solution to users for free to enhance the value for their target audience and drive demand towards the larger platform. They plan to sell the larger solution as a fully SaaS (Software-as-a-Service) based offering, once the user base increases.

Let's examine why Contoso chose this commercial approach:

- **End users**: Decision makers seeking operational improvements through more efficient administrative operations. Contoso is specifically targeting users interested in joining their larger data management platform for its productivity benefits.
- **Business value**: The copilot feature improves customer efficiency by quickly finding contract data and contacts relevant to each invoice. It also saves time by simplifying collaboration across departments to verify milestone completion status within the Teams UX.
- **Technical approach**: Contoso extended some capabilities from the core application using Pro-Code and Teams Message Extensions, Teams Copilot for UX, and Azure SQL database.  
- **Costs**: Broadly low building and testing costs including development resources and cloud services costs but applying investment in core solution.

With this copilot feature offered at no cost, executive decision-makers can access actionable information easily in Teams Copilot, streamlining the approval process and helping improve efficiency without a significant monetary investment.

By bundling the Teams Copilot capability as a part of the larger Copilot solution, Contoso:

- Showcased their software's capabilities to the target user base.
- Enhanced the value of the core Gen AI solution for a new set of target users, leading to greater success later for the overall solution.

Releasing this copilot for free enabled Contoso to provide an efficient solution to their users' frustrations and bring their software and brand to executives with purchasing power. This approach empowered Contoso to position themselves as leaders in the AI space, building brand reputation and boosting market share.

### Freemium scenario: AI-enabled photo editing features

After identifying a need for faster and easier photo editing software, Contoso developed image editing software that uses AI to enhance professional level photographs in addition to providing other standard editing features. The solution is marketed to individuals and enterprise organizations as a SaaS platform, enabling photography professionals to quickly adjust photos whether they work on their own or in a group. 

Customers can either subscribe to a free base version of the application, which includes a daily limit on certain features and fewer capabilities, or a premium version. The premium version provides users with more complex capabilities, such as adding in objects or changing poses in photos, and removes usage limits.  

Let's examine why Contoso chose this commercial approach:

- **End user**: Contoso needed their application to be widely accessible to a wide range of individuals and groups. Though their software was designed with photography professionals in mind, they knew hobbyists and student groups would also make up a large percentage of end users.
- **Business value**: The solution's AI features hasten the photo editing process, saving customers time, and increasing efficiency for application users. The premium version also includes capabilities to improve the effectiveness of the underlying platform, increasing the product's value to customers.
- **Technical approach**: Contoso developed the full platform using elements of Microsoft Fabric and Azure AI Studio. While the larger platform demands continuous support, the free version was developed using low code and required minimal development lift.
- **Costs**: While designing the app, Contoso considered costs for GPU utilization, development, and continued application maintenance. They also noted a high storage cost because their application requires a large model and quantity of data.

Given their application's wide applicability and breadth of end users, Contoso knew they would benefit from a large-scale, highly accessible platform. By providing their feature for free to a large customer base, Contoso increased productivity and reduced costs for a larger number of customers. Offering a free option also made their application accessible to more customers, which greatly increased name recognition and the popularity of their product. Because of the free version's popularity and it allowing users to experience some of the solutions benefits, more users subscribed to the paid model.

During the first few months of their application's deployment, Contoso:

- Enhanced brand recognition and market presence by providing a free option available to a wide range of customers.
- Increased revenue by driving adoption of the paid subscription among professional and enterprise customers.
- Positioned their application as a clear choice for users and industry standard by being accessible to both professionals and students.

Choosing a freemium approach enabled Contoso to expand its customer base quickly and draw in new users. Despite high costs to develop and maintain their solution, Contoso quickly drew in many paying subscribers, which increased overall revenue.

### Paid feature scenario: AI industry marketing assistant

Contoso offers a comprehensive marketing resource management platform that makes it easy for companies to organize their digital assets and create impactful, omnichannel experiences. While the platform already offers a selection of core marketing automation and digital asset management capabilities, Contoso noticed an opportunity. They could help their customers optimize marketing efforts at scale by embedding a new generative AI-powered, copilot-like assistant directly in their platform.

In addition to a generic assistant, Contoso developed a suite of industry-tailored assistants for customers with specific needs. These industry marketing assistants have specialized knowledge and capabilities to support industry-specific functions. They are available to customers at an additional charge over the generic assistant.

Let's examine why Contoso chose this commercial approach:

- **End users**: Contoso wanted to target power users who are interested in applying new technologies to optimize their workflows. They also wanted to broaden their market share by offering next-gen, industry-leading features on their core platform.
- **Business value**: The copilot helps increase customer effectiveness and efficiency by making it easy for users to quickly create high-quality content to meet their industry's needs.  
- **Technical approach**: Contoso developed the copilot with Azure OpenAI Assistants and Azure AI Studio, investing in top-line models like GPT-4.0 to help drive complex and accurate assistant engagement.
- **Costs**: The copilot's costs included development costs, ongoing cloud service charges to run the application in production, and Azure OpenAI subscription costs, warranted by the feature's demand in the market.

Offering these industry-tailored copilots as a paid feature enabled Contoso to appeal to specific audiences within existing and net-new customers. At the same time, Contoso limited costs and development times by using a low-code approach, ensuring their investment aligned with the revenue generated by the feature.

In doing so, Contoso:

- Increased their market share by enhancing the value of their existing platform and driving adoption among new customers.
- Unlocked new revenue streams from power users that want to experiment with new technologies without alienating their core customer base.
- Implemented a quick-win, generative AI feature without incurring significant development costs.

Adding this paid feature to their existing platform enabled Contoso to experiment with a new generative AI capability without incurring high costs or alienating core components of their customer base. This approach helped them establish a solid generative AI foundation on which to expand with future services and features in the future.

### New solution scenario: Contact center agents

Contoso identified an opportunity for increased AI automation in contact centers. Many customer service teams were overwhelmed by the volume of calls, but Contoso realized that many contact requests were for simple tasks or information that an AI assistant could provide. These AI agents could alleviate the loads on human representatives, enabling them to deal with the most complex issues, dramatically increase the efficiency of closed tickets, and reduce overall costs for the contact center.

To meet this demand, Contoso developed a generative AI contact center agent that can answer questions, initiate tasks, and close tickets for customers over the phone or through a text-based interface. Given the market demand and disruption potential of this technology, Contoso built this application as a new, SaaS-based solution, charging a base subscription fee along with a small additional usage charge. Let's take a look at the factors that went into

Contoso developing their application with this approach:

- **End user**: This application would be targeted towards customers with queries or issues that would contact companies' call centers. The application's UI, in the case of chat interfaces, prioritizes simplicity to enable ease of use by the customer.
- **Business value**: This solution improves both the efficiency and effectiveness of contact centers by reducing the cost-per-ticket-closed and empowering contact center workers with further capabilities and support.  
- **Technical approach**: Contoso built the solution using pro-code development with Microsoft Fabric, Azure AI Studio, and Azure OpenAI Assistants. The value this application provides warranted the substantial development lift and continuous support.
- **Costs**: Contoso's costs included development resources, cloud-service costs during development, testing, and production, and continuous support, justified by the core generative AI feature.

Offering their solution as a SaaS model enabled Contoso to manage their application's model usage to flexibly meet demand while simultaneously layering on top of existing legacy contact center systems. These AI agents can intake data from various sources and pass it through multiple levels of automated decision-making to rapidly identify and address operational tasks. As users become more accustomed to engaging with AI-powered interfaces, the underlying core system can become commoditized-valued more as data servers than core systems. This change offers a major potential for disruption in the industry, justifying Contoso's decision to develop their application as a new solution.

With this solution, Contoso:

- Deployed another revenue opportunity through a core solution platform that improves customer efficiency.
- Developed a new, disruptive interface for contact centers that transforms how customers engage with customer service.
- Positioned growth opportunities through future agents, features, and SaaS-based capabilities that can be offered through the platform.

Carefully considering the elements of their solution helped Contoso craft a commercial approach that aligned with their goals and application platform as a whole. The SaaS model that Contoso used for this solution is widely applicable and offers a transformative component to the software that ISVs develop.

### Learn more about SaaS

While developing your solution in a SaaS format can present as a larger initial lift, it ultimately enables greater simplicity and scalability with customers. It may seem simpler to avoid building a SaaS solution from the ground up, but adopting an on-premises or packaged software approach can quickly become costly and inefficient as your customer base expands.
Resources and guidance from Microsoft can help enable your SaaS journey. For more information on developing a SaaS solution, see the following articles:  

- [Foundations of SaaS](/training/saas/saas-foundations/) - Microsoft Learn training focused on the characteristics and benefits of a SaaS model  
- [Overview of the journey for designing SaaS and multitenant solutions](/azure/architecture/guide/saas/plan-journey-saas) - Holistic documentation on the overall journey of developing a SaaS solution
- [Starter web app for SaaS development](/azure/architecture/example-scenario/apps/saas-starter-web-app) - Initial architecture guidance for SaaS applications

### Publishing and sales enablement

Once you establish your commercial strategy and go-to-market approach, your next step is to choose where you want to publish your solution for customers. Microsoft offers two distinct storefronts that allow partners to list offers, enable trials, and transact directly with customers and the Microsoft ecosystem: [Azure Marketplace](https://azuremarketplace.microsoft.com/sell) and [AppSource](https://appsource.microsoft.com/). Azure Marketplace offers a more technical and infrastructurally based storefront through which to engage with customers. You can find comprehensive [guidance around publishing on Azure Marketplace here](/partner-center/marketplace-offers/). The AppSource storefront offers a place for applications oriented towards business decision makers, including Dynamics 365 and Power Platform applications. You can find [guidance for publishing on AppSource here](/marketplace/appsource-overview). These storefronts are differentiated by target audience and product to help customers find quickly find what they need. 

|   | Azure Marketplace | AppSource |
|---| --- | --- |
| **Target audience** | IT professionals and developers | Business decision-makers |
| **Built to extend** | Azure | Azure, Dynamics 365, Office 365, PowerBI, Power Apps |
| **Types of solutions** | Infrastructure solutions and IT-focused professional services | Finished line of business apps and professional services | 
| **Publishing options** | Contact me, consulting services offer, trial, virtual machine, solution templates, and managed apps | Contact me, consulting services offer, and trial |
| **In-app experience** | Azure Portal and CLI | Office 365, Dynamics 365, PowerBI, Office client apps|

Once you've developed your application and aligned on your approach to commercialization, consider how to align your publication strategy with your solution to drive maximum adoption and impact.

## Conclusion

As you consider the commercial strategy for your solution, it's important to remember that there is no one-size-fits-all approach. Holistically considering each of the different factors at play-from potential value and costs, to go-to-market approach-is critical as you determine the best way to monetize your product. In addition to your overall commercial strategy, it's also important to consider how your product is priced. You can find more information on [pricing models for a multitenant solution here](/azure/architecture/guide/multitenant/considerations/pricing-models).

Another resource that can support you as you consider and develop your application's commercial strategy is the [ISV App Advisor](https://www.microsoft.com/isv/app-advisor?msockid=316ebbfcd786677209e7af61d6ec6618). This self-guided experience can surface the latest resources and recommendations based on your current development stage.

Understanding and planning your commercial strategy is a key step towards success as you [evaluate and prioritize a use case](Business-Envisioning.md) and determine the best approach to [developing your solution](Capability-Envisioning.md). Across the ISV Journey, aligning your application's development and positioning with your commercial strategy helps drive a successful deployment and product offering.

Once you establish your use case, how you plan to build it, and your commercial strategy, the next step is determining the specific [low-code](Low-Code-Patterns.md) or [pro-code](Choosing-a-pro-code-pattern.md) pattern to follow as you develop your solution.
