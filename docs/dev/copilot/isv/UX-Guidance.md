---
title: "Creating a dynamic UX: guidance for generative AI applications"
description: UX tips and recomendations for ISVs building generative AI applications
author: miglaros
ms.author: miglaros
ms.service: cloud-for-industries
ms.topic: overview
ms.date: 09/16/2024

#CustomerIntent: As an ISV, I want to build effective UX design for my generative AI application so that my users have a positive copilot experience.
---

<!--
Remove/update all the comments in this template before you sign-off or merge to the main branch.
-->

# Creating a dynamic UX: guidance for generative AI applications

So you proposed a use case for your AI feature or application,  justified it with a business case, and received approval. Great job! Now what? Well, now your stakeholders are ready to see something.

When planning to build an application with generative AI-based features, it's important to consider the tips and guidance provided here to create the user experience (UX) you want. Understanding these key principles can help ensure you're building an engaging, effective application that best supports the needs of your users.

The following guidance walks you through the considerations to have in mind when developing the UX for your generative AI application, with a focus on considerations for building custom copilot experiences.

This article is intended to help you:

* Explore UX framework options and select the best approach for your use cases.
* Learn three foundational principles for developing a copilot, and guidelines for human-AI interaction.  
* Discover how to achieve a collaborative UX through strong input and output design

The following guidance will walk you through the considerations to have in mind when developing the UX for your generative AI application, with a focus on considerations for building custom copilot experiences.

This area is very much in flux and there's a lot to learn, so we've provided key resources for deeper diving. [Microsoft's responsible AI principles](https://www.microsoft.com/ai/principles-and-approach) and the [HAX (HumanAI Experience) Toolkit](https://aka.ms/haxtoolkit/) give some background on the research and real-world experiences behind this article.

## The right focus to get the job done

The following guidance will walk you through the considerations to have in mind when developing the UX for your generative AI application, with a focus on considerations for building custom copilot experiences.

There are three framework variations to consider for your UX:

* Immersive for a whole knowledge base focus
* Assistive in-app focus
* Embedded for single entity focus

<!-- Graphic: 01_UX_Focus-Framework
Image description: Shows three different display frameworks, from left to right they are: A full screen labelled as Immersive, Whole app, knowledge-base focus; a side-bar display labelled as Assistive, In-app focus; A pop-up sytle display labelled Embedded, Single-entity focus 
-->
Let's explore each of these frameworks in more detail

### Immersive focus for a whole knowledge base

<!-- Graphic: 02_UX_immersive
Image description: Shows a larger version of the full screen, immersive framework 
-->

A good rule to follow is the more important the task, the more real estate required.

An immersive environment provides a fully focused experience by utilizing the entire canvas to display relevant information, allowing for deeper insights and reducing distractions for the user. This level of focus is ideal for applications where you want to display information that is related to specific data sources. Examples of this include  AI-generated dashboard similar to [Microsoft's Project Sophia](https://projectsophia.microsoft.com/), or the way in which [Microsoft Copilot for Security](https://www.microsoft.com/security/business/ai-machine-learning/microsoft-copilot-security#modal-41) guides users through a comprehensive process. In an immersive space, complex data or information becomes easier for users to understand and analyze.

### Assistive focus for an in-app experience

<!-- Graphic: 03_UX_assistive
Image description: Shows a larger version of the side-bar, assistive framework 
-->

Give your users the ability to access AI powered assistance from within the applications that they already work in - like Microsoft Teams, Power BI, or your own apps - by integrating copilot as an assistant to extend existing functionality. 

By using in-app focus, users can avoid switching between tools or interfaces. This framework allows the copilot to seamlessly integrate into the user’s workflow, providing relevant suggestions, information, and support on demand without disrupting their current task. This view provides continuous access to tools, information, and assistance without obstructing the main content area. It is particularly effective for applications that require ongoing support or monitoring.  

### Embedded focus for a single entity

<!-- Graphic: 04_UX_embedded
Image description: Shows a larger version of the pop-up, embedded framework
-->

Embedding a single-entry point can simplify the integration of a copilot into your application, reducing complexity and allowing users to receive support on specific items or actions. This helps create a seamless copilot experience with context-aware assistance without occupying permanent screen space.

This option can be ideal for tasks that require only occasional guidance or interaction, though it might not be suitable for more complex or detailed interactions. Fully embedding copilot should align with common interaction patterns, such as highlighting a portion of code to invoke copilot to take action, or enabling users to dive deeper into a chart on an analytics dashboard.

#### Incorporate a secondary focus

In addition to using any of these three frameworks individually, you can create a more robust experience by complimenting your chosen focus with an additional framework option. We find incorporating an embedded option with an immersive or assistive copilot can provide further value for users.

Regardless of what level of focus you choose for your use case, your ultimate goal should be to give your user a copilot experience is positive and productive. The following guidelines are intended to help you maximize the success of your copilot through effective UX design.

## Three Foundational principles for copilot UX

AI driven experiences can be impressive and it's not uncommon for people to have an emotional, trusting response to what seems like conversational, original content. But a copilot simply uses the information it was trained on to predict a word-by-word response with no inherent understanding of truth. Therefore, it's important that you base your copilot on the following principles, setting appropriate expectations for your users.

### Principle 1: Human in control

All great copilot experiences are grounded in the following basic concept: a copilot is simply a tool to support the user. The human is the pilot. 

To set this expectation, position the user in the driver's seat. This means giving them the information they need while still providing transparency around how the copilot works. Communicate its abilities and limitations, and give clarity to the data that outputs are based on. Package this information in meaningful human controls to enable users to confidently and iteratively guide the copilot towards their goals.

For example, when introducing a copilot feature, don't lock up the word "copilot" with action words on the UI. Instead of "copilot, summarize," say "Summarize with copilot." This language reminds the user that the copilot is simply an assistant.

### Principle 2: Avoid anthropomorphizing copilot

Many of the generative AI experiences available today can closely imitate natural human language. Because the technology is so good at doing this, there's potential for users to develop inappropriately high expectations about its nature and abilities and therefore over rely on the copilot's responses.

There are a couple ways you can help prevent users from making these assumptions:

1. **Give copilot its voice.** To avoid the perception that copilot is human-like, teach it to use the right language and avoid certain words in its responses. For example, avoid words like "understand," think," or "feel" in any context as they may convey copilot is human-like. Instead, use words that relate to machines like "processing" and analyzing."  
However, allowing copilot to use first-person singular pronouns (I, me, mine, myself) in its responses works well because it is more conversational. And although using first-person plural pronouns (we, us, ours) to refer to your user and your copilot together is fine, don't use those pronouns to solely represent your company. Why? Because that gives copilot license to be your voice, and in some cases could appear to be speaking on behalf of your company.

2. **Go light on personality.** Consider the implications of what you call your copilot within the UI.  How do you introduce it to your users, and how do you mention it in your marketing and support materials? The more character you give it, the more you humanize it.

### Principle 3: Consider direct and indirect stakeholders

Like all technology, generative AI applications have impact that can reach beyond just the primary user. Throughout the design process, consider not just the immediate user but everyone the product might impact, especially the most vulnerable direct and indirect stakeholders. Make it a habit to design for both primary users and anyone else who might see the output. It's important to consider the wider consequences, including the possible unintended consequences, of your generative AI applications.

These considerations are different for every organization, so discuss it with your team and a few potential users, asking questions like:

* How might this output be used?
* Are users sharing it with anyone else?
* Should other teams or groups review our generative AI strategy?
* Who are the most vulnerable stakeholders and how might we protect them?
* Are we implementing meaningful human controls to empower users with different abilities?  
* What might be the unintended consequences if the technology fails or if it's misused?  

## Designing experiences for the application lifecycle

### First run experience

When your users first invoke your copilot, they should find it engaging enough to start a conversation. They should feel confident in what copilot can and can't do, so be sure to show new users the different ways in which they can use the AI.

Microsoft studies show that users prefer an experience that explains what the copilot can do and gives them suggestions on how to begin. There are many ways to create such an experience and we encourage you to try different approaches with your end users. The following considerations come from the [HAX toolkit](https://aka.ms/haxtoolkit), and have design patterns available which offer several techniques you can mix and match to set user expectations:

* **Make clear what the system can do.** Help the user understand what the AI system is capable of doing.
* **Make clear how well the system can do what it can do.** Help the user understand how often the AI system may make mistakes.

Remember, everyone's learning in this space, and to succeed in any copilot endeavor, you need to keep an open mind, and think creatively. Be ready to experiment, learn, discover, and even conduct your own research.

 Other guidelines should be considered over the course of the application's lifecycle. Some of the most relevant are listed here with links to their corresponding patterns. Learn more about Microsoft's other guidelines in the [HAX Design Library](https://aka.ms/haxtoolkit).

### During interaction

* **Match relevant social norms.** Ensure the experience is delivered in a way that users would expect, given their social and cultural context.
* **Mitigate social biases.** Ensure the AI system's language and behaviors don't reinforce undesirable and unfair stereotypes and biases.

### When it's wrong

* **Support efficient correction.** Make it easy to edit, refine, or recover when the AI system is wrong.
* **Make it clear why the system did what it did.** Enable the user to access an explanation of why the AI system behaved as it did.

### Over time

* **Encourage granular feedback.** Enable the user to provide feedback indicating their preferences during regular interaction with the AI system.
* **Provide global controls.** Allow the user to globally customize what the AI system monitors and how it behaves.

## Collaborative UX

Copilots can improve existing information by making changes or creating new examples without needing extra data. However, this capability also means that a copilot might sometimes generate wrong or unhelpful responses.

To reduce the likelihood of fabrications, one good practice is to enable your users to guide the copilot and move it towards their personal goals and objectives in what is called **collaborative UX**.

You can create a collaborative environment for the user with the following tips for input and output design. You may also find it helpful to align with these [best practices for building collaborative UX with human-AI partnership](/community/content/best-practices-ai-ux)

## Tips for input design

Effective input design forms the cornerstone of the collaborative experience. By guiding users to make well-structured inputs,  It lays the foundation for relevant and accurate responses.

### 1: Provide Suggestions to help users get going

Because generative AI is new tech, it's hard for many people to know what to do or type right away. Long-form natural language typing is still not a habit for many. To help users get going, offer clear suggestions and affordances like large input boxes and character counters that encourage them to form good inputs in addition to a pleasant onboarding experience.

For more specific needs, adding features like promptbooks to give the user specific, short queries that interact with custom data in predictable and repeatable ways may serve to yield useful information faster.

<!-- Graphic: 05_UX_Input-1
Image description: Example of a input box with selectable suggestions. Text in image reads: "Help users form good inputs by providing: Clear suggestions, Large input boxes and character counters"
-->

### 2: Encourage details

Another way to help users create good, detailed inputs is by designing an experience that uses a variety of elements.

For example, you can separate one general prompt into multiple input fields. Replace the question, "What would you like to blog about?" with four inputs, such as:

* Type a title
* Add some more details
* Include images
* Describe the tone

<!-- Graphic: 06_UX_Input-2
Image description: Example of a dialouge box with space to enter a link, a request, images, and overall tone. Text in this image reads: Guide users to enter the right level of detail by splitting up one general question into multiple, more specific questions.
-->

### 3: Allow customization of inputs using tone and other options

Speaking of tone, help users customize their inputs by providing predefined options at the start. Make sure the tone settings are clear to your users and let them know they can change tone settings anytime within a conversation.  

<!-- Graphic: 07_UX_Input-3
Image description: Shows tone setting options and prompts for ations copilot can take. Text in this image reads: Provide clear tone settings right at the start, making it clear that the user's choice can be changed at any time 
-->

### 4: Enhance user interaction and engagement with multimodal design

To enable users to effectively engage with a copilot through whatever device or method they prefer, offer multiple modalities in your input interface. This effort for inclusivity can mean adding both voice and text options and extends to allowing for multi-lingual inputs. Giving the user multiple options to create inputs enables them to more easily and collaboratively communicate.  

## Tips for output design

In a collaborative UX approach, your users need to guide the copilot with a continuous feedback loop between inputs and outputs to reach their goals. Output design creates avenues for the user to shape and influence the copilot's responses and drive towards the desired output.  

### 1: Show inputs and outputs together

This helps your users associate output quality with input choice, affording a tight feedback loop where users can continue to build inputs until the model produces the outputs they desire.

<!-- Graphic: 08_UX_Output-1
Image description: shows iterations of the prompt outputs based on user updates from the input
-->

### 2: Keep a history of outputs and prompts

Encouraging users to try various inputs to get to meaningful outputs is crucial; however, it is not always  a forward interaction with outputs getting better each time.  

Sometimes a new prompt may lead to a worse output. A timeline or history of outputs allow users to try new inputs confidently without fear of losing access to previous outputs that may be better, or even use parts of multiple outputs.  

Likewise, allowing users to make use of previous prompts is deeply valuable to the iterative process.

<!-- Graphic: 09_UX_Output-2
Image description: Shows recent copilot chat history. Text in this image reads: Provide a timeline or history of outputs so users can confidently try new inputs without fear of losing access to previous results
-->

### 3: Add appropriate friction (it's a good thing!)

We often want to remove friction from product experiences. But remember that copilot is an imprecise ("probabilistic") system that is likely to make mistakes. Because of that possibility, you need to add appropriate friction to help users build a new mental mode.  

The goal here is to slow down users and encourage them to review outputs throughout. Add friction at key moments like save, share, copy, and paste, and make clear to the user that they're about to take ownership of the content. Therefore, they take accountability for content that they use encouraging them to check it more thoroughly first.

Here it's recommended that you encourage users to edit content to provide more context or add a personal touch to it. Add AI notices and disclaimers with each output that clearly express AI-generated content may be incorrect.

<!-- Graphic: 10_UX_Output-3
Image description: Shows a dialouge box warning to "be sure to review carefully before inserting, responses may contain inaccuracies". Text in this image reads: Slow users down by adding friction at key moments like save, share, copy, and paste. This should remind them that they own this content so need to look at it closely before using it.
-->

### 4: Encourage fact-checking using citations and direct quotes

One specific way to encourage fact-checking is to have a copilot show references from the data it's referencing, making the AI more likely to use responses from existing resources instead of fabricating data and information.  These references also remind users to take accountability for the content they use by having a second look at copilot's output and verify it against its sources.  

By integrating direct quotes from the source and directing the user to the specific location of that information, your copilot can support more thorough fact-checking. These quotes help copilot to remain aligned with the training dataset and build suitable trust and appropriate reliance.

A final note on fact-checking: showing references doesn't completely prevent a copilot from making things up. Go one step further and design an experience that slows the user down (see "Add appropriate friction") and encourages them to review responses.

### 5: Allow user to edit outputs

A copilot can come close to a desired output but may not exactly match it. You may find that some context is missing. Perhaps the response sounds too general or doesn't match your usual personal tone as opposed to if you created similar content yourself.

 A key part of collaborative UX is letting the user intervene and modify the output. It also shows that copilot is a helper or assistant with the user as the pilot.

<!-- Graphic: 11_UX_Output-4
Image description: Image shows where to click to edit an output. Text in this image reads: Copilot gets the user started, but remember that collaborative experiences allow a user to modify the output, making it theirs. 
-->

### 6: Withhold outputs when necessary**

In some cases, it's better for a copilot to give no answer instead of outputting something potentially inappropriate. You may want the model to disengage and prompt the user to start a new chat with "Sorry, I can't chat about this topic. To save the chat and start a fresh one, select New Chat."

Sometimes there are error states and inappropriate inputs that necessitate you to create prewritten responses. For harmful or controversial topics like self-harm and elections, Microsoft recommends that the copilot not disengage but instead use predefined experiences. In cases where you don't want to disengage the user fully but simply want to redirect the conversation you could recommend that they "please try a different topic."

Every case is different, and it's best to tailor these responses to your application's purpose and expected use.  

### 7: Allow users to provide feedback about the output

Design mechanisms for users to evaluate your copilot's outputs through things like accuracy rating systems, options for users to ask the copilot to correct responses, mark responses as helpful or unhelpful, or leave comments on received outputs. You can also demonstrate how user feedback is helping improve their copilot outputs and experience to reinforce the value of their feedback.

## Next steps

Now that you know how to achieve your desired user experience, here are Microsoft's resources and tools to help you as you begin building your generative AI application.

* [Microsoft HAX Toolkit](https://www.microsoft.com/haxtoolkit/)
  * [HAX Design Library - Microsoft HAX Toolkit](https://www.microsoft.com/haxtoolkit/library/)
  * [Design patterns - Microsoft HAX Toolkit](https://www.microsoft.com/haxtoolkit/design-patterns/)
* [UX for AI: Design Practices for AI Developers | LinkedIn Learning](https://www.linkedin.com/learning/ux-for-ai-design-practices-for-ai-developers)
* [Learn how to use Microsoft Copilot | Microsoft Learn](/copilot)
* [Responsible AI Principles and Approach | Microsoft AI](https://www.microsoft.com/ai/principles-and-approach)
* [Accessibility - Fluent 2 Design System](https://fluent2.microsoft.design/accessibility)
