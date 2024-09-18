---
title: Evaluating and prioritizing an AI use case with ISV business envisioning
description: "Business envisioning is a simple, two-step process that helps partners build out and prioritize business use cases, then create clear business requirements that map into solution architecture design patterns."
author: miglaros
ms.author: miglaros
ms.service: cloud-for-industries
ms.topic: tutorial
ms.date: 09/16/2024

#CustomerIntent: As a <type of user>, I want <what?> so that <why?>.
---

# Evaluate and Prioritize an AI Use Case with Business Envisioning

- [AI is transforming how industries operate](#ai-is-transforming-how-industries-operate)
- [Business Envisioning](#business-envisioning)
  - [Identify potential use cases](#identify-potential-use-cases)
  - [Example scenarios](#example-scenarios)
  - [Business, experience, technology framework](#apply-the-business-experience-technology-framework)
  - [Evaluate and prioritize use cases](#evaluate-the-viability-of-your-use-case)
- [Next Steps](#next-steps)

## Introduction

Artificial intelligence (AI) is revolutionizing industries, prompting businesses to embrace its potential like never before. It has the potential to unlock new opportunities and enable transformative use cases, reshaping the way businesses interact with and provide products and services to their customers. As AI becomes increasingly embedded in everyday applications, we must invest time and attention to ensure planned AI use cases align with business needs, fulfill real user demand, and are technically feasible with available resources.

This article enables Independent Software Vendors (ISVs) to:

- Learn how to identify and refine potential use cases
- Evaluate the viability of use cases
- Understand how to approach use case prioritization

## AI is Transforming How Industries Operate

AI is breaking down barriers across industries, from enhancing manufacturing productivity to minimizing the environmental impact of supply chains. At Microsoft, we believe in providing you with the tools to build the best solution you can, whether that means using traditional AI and machine learning (ML) or cutting-edge generative AI (GenAI) models.

Our partners are increasingly looking to develop new solutions and features that leverage this advanced technology and deliver greater value to customers. Many of our partners are focused on the potential of generative AI and it's potential to transform the way work is being done. While traditional AI and ML certainly have their place, GenAI offers a step forward in the progression of artificial intelligence. While traditional AI and chatbots have existed for some time now, generative AI represents a major step forward in its capabilities and potential.

<!-- Graphic: 01_AI_to_GenAI
Image description: Indicates the transformation "from conversational experiences using trained natural language models..." to "...To conversational assistants powered by large language models". The graphic is split into two columns, left for traditional AI, right for LLM/GenAI, with an arrow from left to right in each of the rows. These rows read as follows: 
Text based interfaces to natural multimodal interfaces; API-based data queries to Unified data sources and models; Prebuilt content and topics to AI-generated content and topics; Context-holding to contextual interactions; Embedded and siloed to Adaptive and responsive.
-->

The possibilities of this transformational technology means that effectively applying the value of AI can be a major undertaking, even for large companies. While this page, and subsequent ones, are designed to help ISVs identify, build, and deploy a viable generative AI use case, there are more resources available. We developed the Microsoft AI Value Accelerator (MAIVA) Playbook to support enterprise customers with foundational knowledge and guidance for enabling AI at scale across their organization. Though not designed as a tactical guide for ISVs, MAIVA can provide valuable insights into the diverse perspectives of the different roles involved in bringing a GenAI application to life. This [open-source framework](https://microsoft.github.io/dstoolkit-maiva/MAIVA_Chapter_1.html#economics-of-governance-framework-or-the-opportunity-cost) can help you understand what successful deployment and operation look like, and why it’s so hard to achieve, as well as guidance around executive roles, developing a data-driven culture, and how to approach change management.

While this development in AI technology brings huge potential for new applications across industries, each opportunity should still be evaluated for its viability. The remainder of this page is devoted to helping ISV business decision makers identify and evaluate the potential business value, user demand, and technical feasibility of proposed AI use cases.

## Business Envisioning

### Identify Potential Use Cases

This page provides a consistent and comprehensive framework to help you evaluate the viability of potential AI and GenAI use cases. Ideas for use cases can come from anywhere, but starting with brainstorming, researching use cases online, and reviewing [Microsoft customer success stories](https://www.microsoft.com/en-us/ai/ai-customer-stories?msockid=31b1a14fb0b467c31d90b594b1196688) can help get the process started.

Microsoft possesses a deep well of experience in this space and collaborates with partners across industries to develop innovative AI use cases. Here are some examples from across industries:

| Industry         | Use Case              | Description and Benefits                                                                 |
|------------------|-----------------------|------------------------------------------------------------------------------------------|
| Manufacturing    | Factory assistant     | Supports factory workers with rapid identification of equipment malfunctions, remediation and resolution options, on-boarding and upskilling of frontline workers, and generation of Programmable Logic Controller (PLC) code using natural language inputs with translation capabilities. |
| Healthcare       | Claims management     | Offers a conversational AI assistant experience that can help claims agents complete authorization and administrative tasks quickly. It is also capable of creating comprehensive authorization strategies based on claims data and improving visibility into the status of claims and authorizations. |
| Financial services | Corporate banking   | Helps bankers forecast client needs based on historical data and interactions, automate routine administrative tasks, and monitor client flows in real time. |
| Sustainability   | Supply chain characterization | Enables sustainability practitioners to review supplier data to ensure compliance with environmental standards, track product lifecycles, and optimize supply chain operations. |
| Retail           | Store operations assistant | Empowers store employees to query operating policies and procedures, accelerate onboarding and upskilling, and review FAQs through an interactive conversational assistant. |

As you develop use cases, it can be helpful to consider the following questions:

- **Problem**: What is the problem to be solved? What are the underlying root causes? How does the current process work?
- **Opportunity**: What is your idea to capture the opportunity or solve the problem? Why do you believe this is a good approach?
- **Business objective**: What business objective is being achieved by enabling this use case?
- **Measurement of success**: How do you define success? How will success be measured? What are the measurable key results that show progress?
- **Accountability**: Who is the executive sponsor for the solution? Who is accountable for the Objectives and Key Results (OKRs)?

You’ll probably be able to think of a variety of AI solutions that you could develop to address a variety of use cases. Business envisioning helps you evaluate and prioritize the most viable use cases for development and execution. You can use the below template <!--link to template:https://microsoft.sharepoint.com/:p:/r/teams/IndustryAI992/Shared%20Documents/LLM/Operations/Content%20Management/Works%20in%20progress/Generative%20AI%20for%20ISVs%20Learn%20Pages%20Project/Publication/Business%20Envisioning/Business%20Envisioning%20Template.pptx?d=w366cb1950a9a4fa3aa5e6603a57b873d&csf=1&web=1&e=KxWTVy --> to make the case for your solution, starting with a description of the problem and your use case. Analyze factors such as the business objective, key results, and primary stakeholders to determine a strategic fit score for your application, 1 to 5. This can serve as your starting point for bringing these various use cases to stakeholders who are needed for development and execution.

<!-- Graphic: 02_Business_Envisioning_Template
Image description: A chart with which to input details about your use case. Areas for the ISV to fill in include: use case name, department, internal or external use, Problem to solve, use case description, business objective, key results, and primary stakeholder. There is a final area with a chart from 1-5 with 1=low and 5=high for strategic fit.
-->

In the following section, we will review how our ISV Contoso used this framework to assign two different use cases strategic scores when we do so for our example scenarios. For now, set this strategic fit score aside. We’ll return to it when we discuss use case prioritization, after evaluating the business value, user experience, and technical feasibility of your use case.

### Example Scenarios

To help guide you through this business envisioning process, we’ll review two potential use cases that ISV Contoso analyzed with their customer AdventureWorks, a large retailer. We examine each use case in-depth and compare them to determine which presents greater value and viability for development.

#### Use Case 1: Contoso Store Operations Assistant

In this scenario, AdventureWorks was struggling with the following problems:

- High rate of employee turnover
- Slow and inefficient employee onboarding
- Low productivity of frontline workers
- Store managers lack effective tools to manage operations

To address these issues, Contoso and AdventureWorks developed a Store Operations AI assistant use case. This assistant would be capable of providing employees with guidance on onboarding materials, store policies and procedures, and task creation. To begin their evaluation, Contoso calculated the strategic fit of this use case using the three factors discussed: business objective, key results, and primary stakeholders.

- **Business objective**: Contoso aims to empower AdventureWorks to quickly onboard new employees, improve employee efficiency, and increase store productivity.
- **Key results**: The Store Operations assistant targets improved Net Promoter Score, increased employee retention, and adherence to compliance and policy requirements.
- **Primary stakeholders**: Contoso identified engineering and product leads to collaborate with their development team on this application.

Based on these factors, Contoso assigned this use case a strategic fit of 4 given that its business impact was not necessarily a revenue-generating opportunity. This high score suggests that there is a strong fit for this use case to help AdventureWorks with the current difficulties they’re facing.

#### Use Case 2: Contoso Shopping Application

In this scenario, Contoso worked with AdventureWorks to identify the following key business and technical problems to address with this use case:

- A lack of a digital storefront
- Constrained employee communication
- Difficulties in planning for inventory allocation
- No automated way of tracking insights such as popular items and shopper behavior

To address the situation, Contoso proposed developing the Contoso Shopping application. A comprehensive solution capable of supporting a digital storefront, connecting employees, and tracking and analyzing data. Using these components, Contoso evaluated the strategic fit of this use case.

- **Business objective**: Contoso sought to solve AdventureWorks struggles with a lack of a digital storefront, deploying an application capable of supporting customer transactions, inventory management, and comprehensive communications.
- **Key results**: Contoso and AdventureWorks identified the successful deployment of this application, specifically focusing on many end-users per day and improved revenue as key performance indicators, as their primary key result.
- **Primary stakeholders**: Contoso has a full development team assigned to the project, with extra subject matter expertise from AdventureWorks ready to go.

Based on these factors, Contoso assigned their use case a strategic fit of 5. This high score suggests a strong fit for Contoso and AdventureWorks and brings us to the next step in the business envisioning process.

After examining the details of each use case and assigning them a strategic fit score, let’s evaluate how they measure up in terms of business value, user experience and demand, and technical viability using the business, experience, technology (BXT) framework.

## Apply the Business, Experience, Technology Framework

The Business, Experience, Technology (BXT) framework enables ISVs to evaluate the potential of their use cases. This exercise is designed to comprehensively walk through the details of each of your use cases and evaluate their viability. This is where you can introduce your use cases to the wider ecosystem of stakeholders who might be involved in their development, from security and compliance experts to technical development teams. Applying the BXT with a wider group not only helps drive clarity on the realistic implementation of each use case, it also helps ensure buy-in from stakeholders whose effort will be required to make this vision a reality. The high-level process includes the following:

- Three high-level focus-areas: business viability, experience and desirability, and technological feasibility.
- Within each of these areas, there are three subcomponents, each with their own score.
- Following this, you’ll rank and visualize the viability of your use cases using a combination of these components.

We walk you through this framework step-by-step using the two example use cases we introduced earlier: Contoso store management assistant and Contoso Shopping application.

<!-- Graphic: 03_BXT_Framework
Image description: On the right, there is a title: "The BXT Framework", on the left, there is a three-way venn diagram. The circles, from top right, to top left, to bottom center read as follows: Business (Viability), Experience (desirability), and Technology (feasibility). In the overlapping space between all three circles reads: Product Success. -->

## Business

First, it’s important to ensure that the prioritized use case aligns with your overall strategy and has real opportunities for revenue. The primary consideration under Business is the financial viability of the solution. To fully answer this question, we’ve broken this part of the framework down into three components: executive strategy alignment, business value, and change management timeframe.

<!-- this below paragraph should be formatted as a purple "note" call-out box on the Learn page-->
>If you’d like to dig deeper into how to drive internal alignment around your use cases and the value they provide, the [Microsoft AI Value Accelerator (MAIVA) framework](https://microsoft.github.io/dstoolkit-maiva/MAIVA_Chapter_1.html) can help. This framework provides comprehensive guidance including AI strategy planning, experimentation cycles, data platforms, and deployment. 

### Executive Strategy Alignment

Your selected use case should align with your organization’s overall mission and teams. For a use case to be successfully developed, key stakeholders and impacted users should be clearly identified and ready to engage on the effort. This helps avoid inefficiencies by aligning teams, resources, objectives, and development efforts and avoiding redundant or nonproductive work.
<!-- Graphic: 04_Executive_Strategy_Alignment
Image description: The center of the image reads: Executive Strategy Alignment, Does the use case align with the overall business strategy and goals?" Below this is a spectrum from 1, no alignment, on the left to 5, very aligned with board-level support, on the right. -->

### Business Value

A successful use case is one that generates value for the business. While this can often take the form of generating value for end-users, and thus revenue opportunities, value can also look like improved productivity and cost-effectiveness for the customer internally. Regardless of the value generated, one of the major things to consider is your approach to monetization; for example, will this be a core product or a free add-on? Four primary approaches have begun to materialize on this front:

- **Core**: the application is mission critical for core product experience, a must-have.
- **Extend**: the application enhances product experience but may not be mission critical for all users, a nice-to-have.
- **Add-on**: the application adds significant value to a particular subset of users for the present.

<!-- Graphic: 05_Business_Value
Image description: The center of the image reads: Business value, How does the use case generate business value?" Below this is a spectrum from 1, value not understood yet, on the left to 5, value very clear and significant, on the right. -->

Considerations around commercial strategy and monetization are critical to determining the business value of your application. You can learn more about what goes into this decision on the Commercial Strategy and Monetization for ISV Applications page, coming soon. 
<!-- insert link to ISV commercialization page when it is available> --> 

### Change Management Timeframe

The development timeline is an important factor in the business viability of your use case. Key milestones to consider are when your application will be ready for internal testing, when it is ready for initial pilot users and when it can begin generating revenue. Inaccurate time estimates could lead to costly and inefficient delays, which can after impact overall business health and pull valuable resources away from more effective or productive efforts. In addition, the implementation and maintenance of product changes can extend beyond release. Use cases that require less fundamental change and have lower impact on operations can serve as achievable quick wins. To help mitigate the potential drawbacks of complex development projects, it’s critical to be realistic with how you approach timeline estimates.

<!-- Graphic: 06_Change_Management_Timeframe
Image description: The center of the image reads: "Change management timeframe, What is the expected time required to implement the use case and manage any changes?" Below this is a spectrum from 1, Long roll-out and high user impact, on the left to 5, short roll-out and low user impact, on the right. -->

It’s important to note that a more complex use case with a longer roll-out and higher user-impact is not necessarily an unviable use case but will likely require more resources and time to develop and deploy, and potentially introduce new obstacles. For example, an application with a longer roll-out potentially has a longer path to profitability. This does not preclude the application from development, but it’s important that ISVs make these decisions with eyes wide open, and balance considerations across the BXT to ensure the intended use case fits their needs, circumstances, and available resources.

### Example Use Cases Business Evaluation

Let’s examine how the example Contoso use cases we introduced land with each of these subcomponents and assign them a score. We use each of these scores, together with the strategic fit score assigned earlier, to prioritize which use case to develop. You’ll score each subcomponent separately, and then take the average of those three scores to find this component’s overall score.

<!-- Graphic: 07_Business_Viability_Assessment 
Image description: The title of the chart is: "Business Viability - Strategic business impact assessment" the top row is for the store operations assistant and the bottom row is for the shopping application. There are four boxes in each row with the same title across both of the assessed use cases. From left to right, those titles read: "How does the use case align to the organization's executive strategy? How does the use case generate business value? Business change management timeframe. Business Impact."
For the Store operations assistant, the content in the four boxes from left to right read: 
- "Modernizes employee experiences and tools
Drives improved productivity and efficiency 
Aligns with AdventureWorks’ executive strategy
Score: 3.
- Reduces task completion time and errors
Optimizes staff resources to help improve worker and store productivity
Score: 2
- Presents a minor change to employees’ experience
Deploys over a short roll-out period
Creates generally minimal impact
Score: 4
- The store operations assistant provides rapidly accessible, but minimal revenue opportunity and thus minor business value. Overall score: 3" 
For the Shopping application, the content in the four boxes from left to right read: 
- "Modernizes employee experiences and tools 
Drives improved productivity and efficiency
Aligns with AdventureWorks’ executive strategy
Score: 5
-Reduces task completion time and errors 
Optimizes staff resources to help improve worker and store productivity 
Score: 5
- Presents a minor change to employees’ experience 
Deploys over a short roll-out period
Creates generally minimal impact 
Score: 3
- The shopping application creates potentially huge value however it requires a longer change management timeframe. Overall score: 4.3"
-->

## Experience

Once you’ve identified a use case’s business value, you need to ensure there is demand for the solution. The experience component of the BXT framework focuses on the needs and preferences of the user, and if the solution and experience is desirable to them. To best answer these questions, we’ve broken down Experience into key personas, value proposition, and change resistance.

### Key Personas

Understanding the key personas relevant to your use cases is critical to identifying existing demand. This includes both ultimate end-users but also stakeholders involved in building and maintaining the application. Having internal resources prepared and committed will help drive an efficient development process. Understanding these personas enables developers to prioritize use cases that address the specific needs, challenges, and goals of these critical groups, ensuring the application delivers maximum impact and value to those who matter most.

<!-- Graphic: 08_Key_Personas
Image description: The center of the image reads: "Key pesonas, Who are the key stakeholders and users who will be impacted by the use case?" Below this is a spectrum from 1, Long roll-out and high user impact, on the left to 5, short roll-out and low user impact, on the right. -->

### Value Proposition

The value proposition of a use case outlines the benefits and advantages that users gain from using the application. This includes aspects like increased efficiency, cost savings, improved productivity, or enhanced end-user experience. Understanding the value proposition is vital as it determines the potential appeal and adoption of the use case. A compelling value proposition can help ensure that the application not only meets end-user needs but also stands out in a competitive market, enhancing revenue opportunities.

<!-- Graphic: 09_Value_Proposition
Image description: The center of the image reads: "Value proposition, What is the value proposition for the user? What is the potential appeal and adoption of the use case?" Below this is a spectrum from 1, low appeal or desire for a user, on the left to 5, highly desired by users, on the right. -->

### Change Resistance

Change resistance is another factor to consider when prioritizing use cases as it reflects the users' willingness to adopt new technologies and processes. High levels of change resistance can hinder the successful implementation and adoption of an application. Understanding the level of change resistance enables developers to devise strategies to manage and mitigate resistance, such as enhanced training and usage-guidance. Addressing change resistance effectively can help ensure smoother transitions and higher acceptance rates, leading to more successful and sustained application usage.

<!-- Graphic: 10_Change_resistance
Image description: The center of the image reads: "Change resistance, What is the level of change resistance for the use case? How can change be managed" Below this is a spectrum from 1,Expected high resistance by users, on the left to 5, Expected very low resistance to change, on the right. -->

### Example Use Cases Experience Evaluation

Let’s examine how our example uses cases align with each of these subcomponents.

<!-- Graphic: 11_Experience_Value_Assessment
Image description: The title of the chart is: "Experience Value - user desirability impact assessment" the top row is for the store operations assistant and the bottom row is for the shopping application. There are four boxes in each row with the same title across both of the assessed use cases. From left to right, those titles read: "Who are the key personas? How appealing is the overall value proposition for the user? What is the change resistance? User desirability."
For the Store operations assistant, the content in the four boxes from left to right read: 
- "Will directly impact: 
Store associates
Managers
Demand planners
Score: 4
- Improves productivity for managers 
Enhances onboarding experience for employees
Score: 3
- Presents low change resistance given the prevalence of natural language assistants 
Score: 4
- This use case doesn’t reach consumers directly, and presents minor value, but presents low potential change resistance. Overall score: 3.7" 
For the Shopping application, the content in the four boxes from left to right read: 
- "Improves access for customers 
Empowers store managers and corporate teams with new data-driven capabilities
Score: 4
-Provides huge value to AdventureWorks by expanding digital access 
Expands market share and revenue opportunities  
Score: 5
- Creates a new system and interface for employees to learn to use
Presenting a medium to large potential for change resistance 
Score: 3
- This application exponentially increases access to AdventureWorks, presenting enormous value, but the solution’s complexity presents potential change resistance. Overall score: 4"
-->

## Technology

Lastly, when prioritizing use cases, considering technical factors such as technical feasibility, integration challenges, and the accuracy of AI models is essential for the ultimate efficacy of the application, and the development process. To comprehensively review the technical feasibility of the use case, consider the following three topics: implementation and operation risks, sufficient safeguards, and the fit of AI and Large Language Models (LLMs).

### Implementation and Operations Risks

Identifying and taking steps to mitigate implementation and operational risks can help developers avoid delays, bugs, and cost overruns, ultimately helping to drive the success of the application. Potential risks can include technical issues, resource constraints, and data security concerns. Mitigation strategies can involve thorough testing, contingency planning, and ensuring robust support and maintenance strategies. By addressing risks early, developers can prevent disruptions and ensure a smoother implementation process, leading to more reliable and sustainable application development and deployment.

<!-- Graphic: 12_Implementation_Risks
Image description: The center of the image reads: "Implementation and operation risks, What are the potential risks associated with the use case and plans for mitigation?" Below this is a spectrum from 1, unknown currently, on the left to 5, well understood and addressable, on the right. -->

### Sufficient Safeguards

Managing risk and compliance, especially with regards to AI, is crucial to the successful development and deployment of your application. Proactive measures such as robust security and compliance measures, data protection and access controls, and responsible and safe AI standards, can help protect users and their data. This also includes ensuring compliance with all relevant legal and regulatory requirements. By prioritizing use cases with well-established safeguards, developers can enhance the reliability and security of their applications, fostering greater user confidence and minimizing potential liabilities.

<!-- Graphic: 13_Sufficient_Safeguards
Image description: The center of the image reads: "Sufficient Safeguards, Are the appropriate safeguards in place to manage risk and ensure compliance?" Below this is a spectrum from 1, unknown currently, on the left to 5, Mature RAI and compliance practices in place, on the right. -->

### AI/LLM Fit

In order to effectively implement AI into a use case, the fit of AI and LLMs with the use case must be evaluated. Use cases that align well with AI and LLM capabilities can benefit from enhanced automation, improved decision-making, and personalized user experiences. Understanding how these technologies can be integrated effectively helps maximize their potential and address specific user needs more efficiently. By prioritizing use cases that are well-suited for AI and LLM implementation, developers can create innovative applications that offer significant competitive advantages and drive substantial value for users.

<!-- Graphic: 14_AI_Fit
Image description: The center of the image reads: "AI/LLM Fit, What is the fit of the use case with AI and LLM technologies?" Below this is a spectrum from 1, type of AI and why is not clear, on the left to 5, need and benefit of AI is clear and compelling, on the right. -->

### Example Use Cases Technology Evaluation

Lastly, let’s look at how our example uses cases align with each of the technology subcomponents.

<!-- Graphic: 15_Technical_Value_Assessment
Image description: The title of the chart is: "Technical value - Feasibility impact assessment" the top row is for the store operations assistant and the bottom row is for the shopping application. There are four boxes in each row with the same title across both of the assessed use cases. From left to right, those titles read: "What are the implementation and operational risks? Are sufficient safeguards available? How is the AI/LLM Fit? Technical feasibility."
For the Store operations assistant, the content in the four boxes from left to right read: 
- "Presents potentially minimal operational risks such as: 
Employees not using or adopting the solution
Integration difficulties integrating existing systems
Score: 5
- Includes safeguards such as:
Access controls
Security audits
Data governance strategy
Score: 4
- Converses in natural language 
Responds to a variety of queries with answers based on customer data. 
Score: 4
- The simplicity of the store operations assistant makes it very feasible with low risks. Overall score: 4.3" 
For the Shopping application, the content in the four boxes from left to right read: 
- "Presents greater implementation risks given its complexity
Contoso’s development experience helps mitigates this risk
Score: 4
-Includes:
Built-in regulatory compliance
Data access controls and governance strategies
Comprehensive security features
Score: 4
- Leverages AI and GenAI in multiple ways including: 
Conversational assistants 
Machine learning-based data analytics
Score: 5
- The complexity of the application presents greater risk, but Contoso’s expertise and the AI fit present huge opportunity. Overall score: 4.3"
-->

Now that you’ve considered each of these factors regarding the viability of your use cases, how do we evaluate the results?

## Evaluate the Viability of Your Use Case

Once you’ve examined your use cases using the BXT framework, the next step is to evaluate which use case to prioritize for development. At this stage, each use case you’re examining should have three scores, one for business, experience, and technology each. You can use this template to consolidate the BXT and subsequent scores for your use cases:

<!-- Graphic: 16_Use_Case_Template
Image description: This blank use case template includes all of the subcomponents of the BXT framework: business: executive strategy alignment, business value, change management timeframe; Experience: Key personas, appeal of value proposition, change resistance; Technical value: implementation and operational risks, sufficient safeguards, AI/LLM Fit. At the end of each of these is an all-up for each of the primary BXT components: Business Impact, User desirability, and Technical feasibility. -->

Now let’s return to the strategic fit score we calculated earlier, when we identified these use cases. Combining the strategic fit score with the business viability, experience value, and technical feasibility for each of your use cases yields the metrics we use to determine which use case is worth prioritizing. We use two metrics as axis to visualize the viability of these use cases:

1. Degree of strategic business impact = mean of strategic fit and business impact
2. Degree of executional fit = mean of user desirability and technical feasibility

Using these two values, you can graph the results of your use case prioritization:

<!-- Graphic: 17_Blank_Prioritization 
Image description: This a chart with four equally sized rectangles making up each of the quadrants. The X-Axis reads: "Degree of executional fit", and goes from 1 (low) to 5 (high). The Y-axis reads: "Degree of strategic business impact", and goes from 1 (low) to 5 (high). The quadrants, clockwise starting from top left read: Research, Accelerate to MVP, Incubate, Shelve.-->

Where each use case falls can help you determine the best path forward for its development.

- **Shelve**: Low business impact and value, and potential difficulty with development and/or demand, suggesting that this use case is not viable and should be shelved until circumstances change.
- **Research**: Potentially high business impact but currently unfeasible or have low demand. These use cases are ripe for research into factors that may impact the executional fit of this solution such as problem space, user needs, and/or market conditions.
- **Incubate**: Technically feasible and/or have existing user demand, however with low business impact or strategic fit. Use cases falling into this category are ripe for prototyping and testing in controlled environments to help ensure the use case is refined to align with your strategic fit and business goals.
- **Accelerate to MVP**: High strategic business impact, technically feasible, and meet existing demand. These are use cases ready to be invested in and developed.

### Example Scenarios: Use Case Prioritization

Besides the two analyzed use cases, another team completed a business envisioning exercise for a third use case: an AI-powered inventory management solution integrated with inventory databases. The table that follows shows its BXT scores. Let's review the results for all use cases, including their initial strategic fit scores and BXT component scores, and visualize their positions on our prioritization chart.

| BXT Component | Store Operations Assistant     | Shopping Application | Inventory Manager |
|---------------|----------------------|-------------------|---------|
| Strategic fit | 4 | 5 | 3 |
| Business impact | 3 | 4.3 | 3 |
| User Experience | 3.7 | 4 | 3 |
| Technical feasibility | 4.3 | 4.3 | 4 |
| Degree of strategic business impact | (4+3)/2 = **3.5** | (5+4.3)/2 = **4.7** | (3+3)/2 = **3** |
| Degree of executional fit | (3.7+4.3)/2 = **4** | (4+4.3)/2 = **4.2** | (3+4)/2 = **3.5** |

Using our two equations, we can see where each of these use cases fall on this chart.

<!-- Graphic: 18_Use_Case_Prioritization
Image description: This a chart with four equally sized rectangles making up each of the quadrants. The X-Axis reads: "Degree of executional fit", and goes from 1 (low) to 5 (high). The Y-axis reads: "Degree of strategic business impact", and goes from 1 (low) to 5 (high). The quadrants, clockwise starting from top left read: "Research, Accelerate to MVP, Incubate, Shelve."
On the right side of the chart are three circles labeled: 1. Contoso Shopping application, 2. Contoso Store Operations assistant, and 3. Contoso inventory manager. On the chart, there are three circles with numbers aligning to the three use cases on the left side. Circle 1 is farthest up and to the right, with 2 and 3 subsequently lower and more to the left. 1 and 2 both fall into the "accelerate to MVP" quadrant, while 3 falls in the "incubate" quadrant. 
-->

We can see that Contoso shopping application rests squarely in the "Accelerate to MVP" quadrant as shown above, suggesting this use case is best suited for development, both technically and strategically. This corner represents the meeting point of the best value and technical feasibility. In contrast, the inventory manager doesn’t provide adequate strategic business impact to develop and should be incubated for more testing and revision.

Notably, the Store Operations assistant also falls in this quadrant, suggesting it would also be fit for development. However, the business value and technical potential of the Shopping application are clearly greater. Additionally, the Shopping application provides large learning potential surrounding building larger, more complex applications. On top of the obvious revenue potential of standalone commercial application, this effort presents long-term strategic and business value through learnings and templatized reusable features.

Given the strong viability of the Shopping application, as illustrated through this exercise, Contoso can proceed confidently into the development stage. The next step in this process is to determine what the best approach would be to developing this solution, and what that journey would look like. Over the course of this ISV application development journey, we return to this example scenario, and others, to give you a roadmap to developing your applications from start to finish.

## Next Steps

Now that you identified a priority use case, it’s time to start thinking about how to design and build your solution. The next step in this path is to review the Capability Envisioning and ISV Journey page <!-- insert link to capability envisioning page when it is published --> where you can learn about how to decide the best development approach for your selected use case.

## Related Links

- [AI Customer Stories | Microsoft AI](https://www.microsoft.com/ai/ai-customer-stories)
- [Creating GenAI Experiences with the Microsoft Cloud: A Guide for ISVs](isv-extensibility-story.md)
<!-- insert links to additional relevant pages as they are published -->
