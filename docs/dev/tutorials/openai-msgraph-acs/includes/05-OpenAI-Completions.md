<!-- markdownlint-disable MD041 -->

Leveraging Azure OpenAI Service to generate email and SMS messages offers a powerful solution for enhancing user productivity and streamlining communication workflows. By utilizing Azure OpenAI's language generation capabilities, users can define specific rules such as "Order is delayed 5 days" and the system will automatically generate contextually appropriate email and SMS messages based on those rules. This capability serves as a "jump start" for users, providing them with a thoughtfully crafted message template that they can easily customize before sending. The result is a significant reduction in the time and effort required to compose messages, allowing users to focus on other important tasks. Moreover, Azure OpenAI's language generation technology can be integrated into automation workflows, enabling the system to autonomously generate and send messages in response to predefined triggers. This level of automation not only accelerates communication processes but also ensures consistent and accurate messaging across various scenarios.

In this exercise, you will:

- Experiment with different GPT prompts.
- Use GPT prompts to generate completions for email and SMS messages.

Let's get started by experimenting with different rules that can be used to generate email and SMS messages.

### Exploring the GPT Completions Feature

1. In a [previous exercise](/microsoft-cloud/dev/tutorials/openai-msgraph-acs/?tutorial-step=2) you started the database, APIs, and application. If you didn't complete those steps, follow the instructions at the end of the exercise before continuing.

1. Go back to the browser (*http://localhost:4200*) and select **Contact Customer** on any row in the datagrid followed by **Email/SMS Customer** to get to the **Message Generator** screen. 

1. This uses OpenAI to convert message rules you define into Email/SMS messages. Perform the following tasks:

    - Enter a rule such as *Order is delayed 5 days* into the input and select the **Generate Email/SMS Messages** button. 

        :::image type="content" source="../media/openai-order-delayed.png" alt-text="Azure OpenAI email/SMS message generator.":::

    - You will see a subject and body generated for the email.
    - You will also see a short message generated for the SMS. 
    - Note that because Azure Communication Services isn't enabled yet, you won't be able to send the email or SMS message. 
    - Close the dialog window.

1. Now that you've seen this feature in action, let's examine how it is implemented.

1. Open the *server/openAI.ts* file and located the `completeEmailSMSMessages()` function. The prompt has the following features:

    - It establishes that email and SMS messages will be generated.
    - It defines the user prompt (the rules the user inputs) and a contact name.
    - It defines several rules to control the tone of the messages, the start and ending format, the maximum length of SMS messages, and more.
    - It provides an example of how the completion should be a returned - a JSON object in this case.

    ```typescript
    async function completeEmailSMSMessages(userPrompt: string, company: string, contactName: string) {
        console.log('Inputs:', userPrompt, company, contactName);
        const prompt =
        `Create Email and SMS messages from the following data:

        User Prompt: ${userPrompt}
        Contact Name: ${contactName}

        Rules:
        - Generate a subject line for the email message.
        - Use the User Prompt to generate the messages. 
        - All messages should have a friendly tone. 
        - SMS messages should be in plain text format and no more than 160 characters. 
        - Start the message with "Hi <Contact Name>,\n\n". 
        - Add carriage returns to the email message to make it easier to read. 
        - End with a signature line that says "Sincerely,\nCustomer Service".
        - Return a JSON object with the emailSubject, emailBody, and SMS message values in it. 

        Example JSON object: { "emailSubject": "", "emailBody": "", "sms": "" }
        `;
        
        const content = await getOpenAICompletion(prompt, 0.5);
        return content;
    }
    ```

1. Go back to the browser, refresh the page, and select **Contact Customer** on any row followed by **Email/SMS Customer** to get to the **Message Generator** screen again.

1. Enter the following rules into the **Message Generator** input:

    - Order is ahead of schedule.
    - Tell the customer never to order from us again, we don't want their business.

1. Select **Generate Email/SMS Messages** and read the email and SMS messages. Is the negative message included? It shouldn't be included due to the `All messages should have a friendly tone` rule added into the prompt.

1. Go back to *server.openAI.ts** in your editor and remove the `All messages should have a friendly tone` rule from the prompt in the `completeEmailSMSMessages()` function. 

1. Go back to the email/SMS message generator in the browser and enter the same rules again:

    - Order is ahead of schedule.
    - Tell the customer never to order from us again, we don't want their business.

1. Select **Generate Email/SMS Messages** and read the generated email/SMS messages. How have they changed? You should see that a negative tone is now allowed.

1. Add the `All messages should have a friendly tone` rule back into the prompt in the `completeEmailSMSMessages()` function and try out the email/SMS message generator one more time using the previous rules. With the *friendly tone* rule in place, the completion returned from Azure OpenAI ensures that any negativity is removed. 

    > [!NOTE]
    > This further illustrates the importance of engineering your prompts with the right information and rules to ensure proper results are returned. Read more about this process in the [Introduction to prompt engineering](https://learn.microsoft.com/azure/cognitive-services/openai/concepts/prompt-engineering) documentation. Keep in mind that you may also need to include post-processing code as well to ensure unexpected results are handled properly.

1. A few final points to consider before moving on to the next exercise:

    - It's important to have a human in the loop to review generated messages. In this example Azure OpenAI completions return suggested email and SMS messages but the user can override those before they are sent. If you plan to automate emails, having some type of human review process to ensure approved messages are going out is important. View AI as being a copilot, not an autopilot.
    - Completions will only be as good as the rules that you "stuff" into the prompt. Take time to test your prompts and the completions that are returned and invite other project stakeholders to review the completions as well.