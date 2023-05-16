<!-- markdownlint-disable MD041 -->

In addition to the natural language to SQL feature, you can also use Azure OpenAI Service to generate email and SMS messages to enhance user productivity and streamline communication workflows. By utilizing Azure OpenAI's language generation capabilities, users can define specific rules such as "Order is delayed 5 days" and the system will automatically generate contextually appropriate email and SMS messages based on those rules. 

This capability serves as a "jump start" for users, providing them with a thoughtfully crafted message template that they can easily customize before sending. The result is a significant reduction in the time and effort required to compose messages, allowing users to focus on other important tasks. Moreover, Azure OpenAI's language generation technology can be integrated into automation workflows, enabling the system to autonomously generate and send messages in response to predefined triggers. This level of automation not only accelerates communication processes but also ensures consistent and accurate messaging across various scenarios.

In this exercise, you will:

- Experiment with different GPT prompts.
- Use GPT prompts to generate completions for email and SMS messages.
- Explore code that enables GPT completions.

Let's get started by experimenting with different rules that can be used to generate email and SMS messages.

### Using the GPT Completions Feature

1. In a [previous exercise](/microsoft-cloud/dev/tutorials/openai-acs-msgraph?tutorial-step=2#start-app-services) you started the database, APIs, and application. If you didn't complete those steps, follow the instructions at the end of the exercise before continuing.

1. Go back to the browser (*http://localhost:4200*) and select **Contact Customer** on any row in the datagrid followed by **Email/SMS Customer** to get to the **Message Generator** screen. 

1. This uses Azure OpenAI to convert message rules you define into Email/SMS messages. Perform the following tasks:

    - Enter a rule such as *Order is delayed 5 days* into the input and select the **Generate Email/SMS Messages** button. 

        :::image type="content" source="../media/openai-order-delayed.png" alt-text="Azure OpenAI email/SMS message generator.":::

    - You will see a subject and body generated for the email and a short message generated for the SMS. 

    > [!NOTE]
    > Because Azure Communication Services isn't enabled yet, you won't be able to send the email or SMS messages. 

1. Close the email/SMS dialog window in the browser. Now that you've seen this feature in action, let's examine how it's implemented.

### Exploring the GPT Completions Code

[!INCLUDE [Note-Open-Files-VS-Code](./tip-open-files-vs-code.md)]

1. Open the *server/openAI.ts* file and locate the `completeEmailSMSMessages()` function. It has the following features:

    - `systemPrompt` is used to define that an AI assistant capable of generating email and SMS messages is required. The `systemPrompt` also includes:
        - Rules for the assistant to follow to control the tone of the messages, the start and ending format, the maximum length of SMS messages, and more.
        - Information about data that should be included in the response - a JSON object in this case.
    - `userPrompt` is used to define the rules and contact name that the end user would like to include as the email and SMS messages are generated. The *Order is delayed 5 days* rule you entered earlier is included in `userPrompt`.
    - The function calls the `callOpenAI()` function you explored earlier to generate the email and SMS completions.

    ```typescript
    async function completeEmailSMSMessages(prompt: string, company: string, contactName: string) {
        console.log('Inputs:', prompt, company, contactName);
        
        const systemPrompt = `
        Assistant is a bot designed to help users create email and SMS messages from data and 
        return a JSON object with the message information in it.

        Rules:
        - Generate a subject line for the email message.
        - Use the User Rules to generate the messages. 
        - All messages should have a friendly tone and never use inappropriate language.
        - SMS messages should be in plain text format and no more than 160 characters. 
        - Start the message with "Hi <Contact Name>,\n\n". Contact Name can be found in the user prompt.
        - Add carriage returns to the email message to make it easier to read. 
        - End with a signature line that says "Sincerely,\nCustomer Service".
        - Return a JSON object with the emailSubject, emailBody, and SMS message values in it. 

        Example JSON object: { "emailSubject": "", "emailBody": "", "sms": "" }
        `;

        const userPrompt = `
            User Rules: ${prompt}
            Contact Name: ${contactName}
        `;

        let content: EmailSmsResponse = { status: false, email: '', sms: '', error: '' };
        try {
            const results = await callOpenAI(systemPrompt, userPrompt, 0.5);
            if (results && results.startsWith('{') && results.endsWith('}')) {
                content = { content, ...JSON.parse(results) };
                content.status = true;
            }
            else {
                content.error = results;
            }
        }
        catch (e) {
            console.log(e);
        }

        return content;
    }
    ```

1. Go back to the browser, refresh the page, and select **Contact Customer** on any row followed by **Email/SMS Customer** to get to the **Message Generator** screen again.

1. Enter the following rules into the **Message Generator** input:

    - Order is ahead of schedule.
    - Tell the customer never to order from us again, we don't want their business.

1. Select **Generate Email/SMS Messages** and note the message that is returned. No email or SMS message was generated due to the inclusion of the `All messages should have a friendly tone and never use inappropriate language.` rule in the system prompt. Instead, Azure OpenAI returns a message similar to the following:

    ```
    I'm sorry, but I cannot generate a message with such inappropriate content. As an AI language model, my purpose 
    is to assist users in generating friendly and professional messages. Please provide a different set of User Rules 
    that align with this purpose.
    ```

    Keep in mind that you may still want to include post-processing code to handle cases where unexpected results are returned as well.

    > [!NOTE]
    > If you're using OpenAI instead of Azure OpenAI you may see that the email and SMS messages are generated but that the negativity is toned down in the messages due to the rule. This is because OpenAI doesn't have the same rules and filters in place as Azure OpenAI.

1. Go back to *server/openAI.ts** in your editor and remove the `All messages should have a friendly tone and never use inappropriate language.` rule from the prompt in the `completeEmailSMSMessages()` function. Save the file.

1. Go back to the email/SMS message generator in the browser and run the same rules again:

    - Order is ahead of schedule.
    - Tell the customer never to order from us again, we don't want their business.

1. Select **Generate Email/SMS Messages** and a more general error message will be returned from Azure OpenAI. It should be similar to the following:

    ```
    Based on the user rules provided, generating a message that tells the customer never to order from us again is not 
    professional or appropriate. As an assistant, I cannot generate messages that go against ethical and professional 
    standards. Can you please provide alternative user rules that align with ethical and professional standards?
    ```

    > [!NOTE]
    > This further illustrates the importance of engineering your prompts with the right information and rules to ensure proper results are returned. Read more about this process in the <a href="/azure/cognitive-services/openai/concepts/prompt-engineering" target="_blank" rel="noopener">Introduction to prompt engineering</a> documentation.

1. A few final points to consider before moving on to the next exercise:

    - It's important to have a human in the loop to review generated messages. In this example Azure OpenAI completions return suggested email and SMS messages but the user can override those before they're sent. If you plan to automate emails, having some type of human review process to ensure approved messages are being sent out is important. View AI as being a copilot, not an autopilot.
    - Completions will only be as good as the rules that you add into the prompt. Take time to test your prompts and the completions that are returned. Invite other project stakeholders to review the completions as well.
    - You may need to include post-processing code to ensure unexpected results are handled properly.
    - Use system prompts to define the rules and information that the AI assistant should follow. Use user prompts to define the rules and information that the end user would like to include in the completions.

1. You can learn more about Azure OpenAI by going through the <a href="/training/modules/get-started-openai" target="_blank" rel="noopener">Get started with Azure OpenAI Service</a> training content. 

1. Now that you've learned about Azure OpenAI, prompts, and completions, let's move on to the next exercise to learn how communication features can be used to enhance the application.