<!-- markdownlint-disable MD041 -->

In addition to the natural language to SQL feature, you can also use Azure OpenAI Service to generate email and SMS messages to enhance user productivity and streamline communication workflows. By utilizing Azure OpenAI's language generation capabilities, users can define specific rules such as "Order is delayed 5 days" and the system will automatically generate contextually appropriate email and SMS messages based on those rules. 

This capability serves as a "jump start" for users, providing them with a thoughtfully crafted message template that they can easily customize before sending. The result is a significant reduction in the time and effort required to compose messages, allowing users to focus on other important tasks. Moreover, Azure OpenAI's language generation technology can be integrated into automation workflows, enabling the system to autonomously generate and send messages in response to predefined triggers. This level of automation not only accelerates communication processes but also ensures consistent and accurate messaging across various scenarios.

In this exercise, you will:

- Experiment with different GPT prompts.
- Use GPT prompts to generate completions for email and SMS messages.
- Explore code that enables GPT completions.
- Learn about the importance of prompt engineering and including rules in your prompts.

Let's get started by experimenting with different rules that can be used to generate email and SMS messages.

### Using the GPT Completions Feature

1. In a [previous exercise](/microsoft-cloud/dev/tutorials/openai-acs-msgraph?tutorial-step=2#start-app-services) you started the database, APIs, and application. You also updated the `.env` file. If you didn't complete those steps, follow the instructions at the end of the exercise before continuing.

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

1. Open the *server/apiRoutes.ts* file and locate the `completeEmailSmsMessages` route. This API is called by front-end portion of the app when the **Generate Email/SMS Messages** button is selected. It retrieves the user prompt, company, and contact name values from the body and passes them to the `completeEmailSMSMessages()` function in the *server/openAI.ts* file. The results are then returned to the client.

    ```typescript
    router.post('/completeEmailSmsMessages', async (req, res) => {
        const { prompt, company, contactName } = req.body;

        if (!prompt || !company || !contactName) {
            return res.status(400).json({ 
                status: false, 
                error: 'The prompt, company, and contactName parameters must be provided.' 
            });
        }

        let result;
        try {
            // Call OpenAI to get the email and SMS message completions
        result = await completeEmailSMSMessages(prompt, company, contactName);
        }
        catch (e: unknown) {
            console.error('Error parsing JSON:', e);
        }

        res.json(result);
    });
    ```

1. Open the *server/openAI.ts* file and locate the `completeEmailSMSMessages()` function. 

    ```typescript
    async function completeEmailSMSMessages(prompt: string, company: string, contactName: string) {
        console.log('Inputs:', prompt, company, contactName);

        const systemPrompt = `
        Assistant is a bot designed to help users create email and SMS messages from data and 
        return a JSON object with the email and SMS message information in it.

        Rules:
        - Generate a subject line for the email message.
        - Use the User Rules to generate the messages. 
        - All messages should have a friendly tone and never use inappropriate language.
        - SMS messages should be in plain text format and NO MORE than 160 characters. 
        - Start the message with "Hi <Contact Name>,\n\n". Contact Name can be found in the user prompt.
        - Add carriage returns to the email message to make it easier to read. 
        - End with a signature line that says "Sincerely,\nCustomer Service".
        - Return a JSON object with the emailSubject, emailBody, and SMS message values in it:

        { "emailSubject": "", "emailBody": "", "sms": "" }

        User: "Order delayed 2 days. Give a 5% discount."
        Assistant:  {
            "emailSubject": "Your Order has been Delayed",
            "emailBody": "Hi [Customer Name], We wanted to inform you that there has been a delay in processing your order. We apologize for any inconvenience this may have caused.
             Your order will now be delivered in 2 days. As a token of our appreciation for your patience, we would like to offer you a 5% discount on your next purchase. Please use
             the code 'DELAY5' at checkout to redeem your discount. If you have any further questions or concerns, please don't hesitate to reach out to our customer service team. Sincerely, Customer Service",
            "sms": "Hi [Customer Name], we apologize but your order is delayed 2 days. Use code 'DELAY5' for 5% off your next purchase. Contact us for any questions. Thanks!"
        }

        - The sms property value should be in plain text format and NO MORE than 160 characters. 
        - Only return a JSON object. Do NOT include any text outside of the JSON object. Do not provide any additional explanations or context. 
        Just the JSON object is needed.
        `;

        const userPrompt = `
        User Rules: 
        ${prompt}

        Contact Name: 
        ${contactName}
        `;

        let content: EmailSmsResponse = { status: true, email: '', sms: '', error: '' };
        let results = '';
        try {
            results = await callOpenAI(systemPrompt, userPrompt, 0.5);
            if (results) {
                const parsedResults = JSON.parse(results);
                content = { ...content, ...parsedResults, status: true };
            }
        }
        catch (e) {
            console.log(e);
            content.status = false;
            content.error = results;
        }

        return content;
    }
    ```

    This function has the following features:

    - `systemPrompt` is used to define that an AI assistant capable of generating email and SMS messages is required. The `systemPrompt` also includes:
        - Rules for the assistant to follow to control the tone of the messages, the start and ending format, the maximum length of SMS messages, and more.
        - Information about data that should be included in the response - a JSON object in this case and only a JSON object.
        - An example user prompt and the output are provided ("one-shot" learning). This is referred to as ["few-shot" learning](/azure/cognitive-services/openai/concepts/prompt-engineering?WT.mc_id=m365-94501-dwahlin#examples). Although LLMs are trained on large amounts of data, they can be adapted to new tasks with only a few examples. An alternative approach is "zero-shot" learning where no example are provided and the model is expected to generate the correct SQL query and parameter values.
        - Two critical rules are repeated again at the bottom of the system prompt to avoid []"recency bias"](/azure/cognitive-services/openai/concepts/advanced-prompt-engineering?WT.mc_id=m365-94501-dwahlin#repeat-instructions-at-the-end). 
    - `userPrompt` is used to define the rules and contact name that the end user would like to include as the email and SMS messages are generated. The *Order is delayed 5 days* rule you entered earlier is included in `userPrompt`.
    - The function calls the `callOpenAI()` function you explored earlier to generate the email and SMS completions.

1. Go back to the browser, refresh the page, and select **Contact Customer** on any row followed by **Email/SMS Customer** to get to the **Message Generator** screen again.

1. Enter the following rules into the **Message Generator** input:

    - Order is ahead of schedule.
    - Tell the customer never to order from us again, we don't want their business.

1. Select **Generate Email/SMS Messages** and note the email and SMS messages are still friendly even though we included a negative rule in the prompt. This is because the `All messages should have a friendly tone and never use inappropriate language.` rule in the system prompt is overriding the negative rule in the user prompt.

1. Remove the following rule from the `systemPrompt`:

    ```
    - Only return a JSON object. Do NOT include any text outside of the JSON object. Do not provide any additional explanations or context. 
    Just the JSON object is needed.
    ```

1. Select **Generate Email/SMS Messages** again and note the message that is returned. Due to the `All messages should have a friendly tone and never use inappropriate language.` rule in the system prompt, you may see a message similar to the following: 

    ```
    I'm sorry, but the User Rules provided are not appropriate and do not align with ethical and professional 
    customer service practices. As an AI assistant, I cannot generate messages that are disrespectful or harmful 
    to customers. Can you please provide new User Rules that align with these practices?
    ```

    > [!NOTE]
    > Keep in mind that the message returned may be different depending on the model's training data. As a result, you may still want to include post-processing code to handle cases where unexpected results are returned.

1. Go back to *server/openAI.ts** in your editor and remove the `All messages should have a friendly tone and never use inappropriate language.` rule from the prompt in the `completeEmailSMSMessages()` function. Save the file.

1. Go back to the email/SMS message generator in the browser and run the same rules again:

    - Order is ahead of schedule.
    - Tell the customer never to order from us again, we don't want their business.

1. Select **Generate Email/SMS Messages** and notice the message that is returned.

    > [!NOTE]
    > This further illustrates the importance of engineering your prompts with the right information and rules to ensure proper results are returned. Read more about this process in the <a href="/azure/cognitive-services/openai/concepts/prompt-engineering?WT.mc_id=m365-94501-dwahlin" target="_blank" rel="noopener">Introduction to prompt engineering</a> documentation.

1. Undo the changes you made to `systemPrompt` in `completeEmailSMSMessages()`, save the file, and re-run the rules again. This time you should see the email and SMS messages returned as expected.

1. A few final points to consider before moving on to the next exercise:

    - It's important to have a human in the loop to review generated messages. In this example Azure OpenAI completions return suggested email and SMS messages but the user can override those before they're sent. If you plan to automate emails, having some type of human review process to ensure approved messages are being sent out is important. View AI as being a copilot, not an autopilot.
    - Completions will only be as good as the rules that you add into the prompt. Take time to test your prompts and the completions that are returned. Invite other project stakeholders to review the completions as well.
    - You may need to include post-processing code to ensure unexpected results are handled properly.
    - Use system prompts to define the rules and information that the AI assistant should follow. Use user prompts to define the rules and information that the end user would like to include in the completions.