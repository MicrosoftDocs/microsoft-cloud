<!-- markdownlint-disable MD041 -->

The integration of Azure OpenAI Natural Language Processing (NLP) and completion capabilities offers significant potential for enhancing user productivity. By leveraging appropriate prompts and rules, an AI assistant can efficiently generate various forms of communication, such as email messages, SMS messages, and more. This functionality leads to increased user efficiency and streamlined workflows.

While this feature is quite powerful on its own, there may be cases where users need to generate completions based on your company's custom data. For example, you might have a collection of product manuals that may be challenging for users to navigate when they're assisting customers with installation issues. Alternatively, you might maintain a comprehensive set of Frequently Asked Questions (FAQs) related to healthcare benefits that can prove challenging for users to read through and get the answers they need. In these cases and many others, Azure OpenAI Service enables you to leverage your own data to generate completions, ensuring a more tailored and contextually accurate response to user questions.

In this exercise, you will:

- Create a custom data source using Azure AI Studio.
- Start a chat session in the Chat playground to experiment with generating completions based upon your own data.
- Explore code that uses Azure Cognitive Search and Azure OpenAI to generate completions based upon your own data.

Let's get started by adding a custom data source to Azure AI Studio.

### Adding a Custom Data Source to Azure AI Studio

1. Navigate to [Azure OpenAI Studio](https://oai.azure.com/) and sign in with credentials that have access to your Azure OpenAI resource. 

1. Locate the **Bring your own data** tile on the welcome screen and select **Try it now**.

    :::image type="content" source="../media/aoai-studio-byod.png" alt-text="Azure OpenAI Studio Bring Your Own Data":::

1. Select **Upload files** from the **Select data source** dropdown.

1. Under the **Select Azure Blob storage resource** dropdown, select **Create a new Azure Blog storage resource**.

1. This will take you to the Azure Portal where you can perform the following tasks:

    - Enter a unique name for the storage account such as **byodstorage[Your Last Name]**.
    - Select a region that's close to your location.
    - Select **Review** followed by **Create**.

1. Once the blog storage resource is created, go back to the Azure AI Studio dialog and select your newly created blog storage resource from the **Select Azure Blob storage resource** dropdown. If you don't see it listed, refresh the page to reload the dialog in Azure AI Studio.

1. Cross-origin resource sharing (CORS) needs to be turned out in order for your storage account to be accessed. Select **Turn on CORS** in the Azure AI Studio dialog.

    :::image type="content" source="../media/aoai-studio-turn-on-cors.png" alt-text="Azure OpenAI Studio Bring Your Own Data Turn on CORS":::

1. Under the **Select Azure Cognitive Search resource** dropdown, select **Create a new Azure Cognitive Search resource**.

1. This will take you back to the Azure Portal where you can perform the following tasks:

    - Enter a unique name for the Cognitive Search resource such as **byodsearch[Your Last Name]**.
    - Select a region that's close to your location.
    - In the **Pricing tier** section, select **Change Pricing Tier** and select **Basic** followed by **Select**. The free tier will not work for this exercise, so you'll clean up the Cognitive Search resource at the end of this exercise.
    - Select **Review** followed by **Create**.

1. Once the Cognitive Search resource is created, go to the resource **Overview** page and copy the **Url** value to a local file.

1. Select **Keys** in the left navigation menu and copy the **Primary admin key** value to a local file. You'll need these values later in the exercise.

1.  go back to the Azure AI Studio dialog and select your newly created search resource from the **Select Azure Cognitive Search resource** dropdown. If you don't see it listed, refresh the page to reload the dialog in Azure AI Studio.

1. Enter **byod-search-index** for the **Enter the index name** value.

1. Select the **I acknowledge that connecting to an Azure Cognitive Search account will incur usage to my account**  checkbox.

1. Select **Next**.

1. In the **Upload files** section of the dialog, select **Browse for a file**.

1. Navigate to the project's **customer documents** folder (located at the root of the project) and select the following files:

    - *Clock A102 Installation Instructions.docx* 
    - *Company FAQs.docx*

    > [!NOTE]
    >  This feature currently supports the following file formats for local index creation: .txt, .md, .html, .pdf, .docx, and .pptx.

1. Select **Upload files**.

1. Select **Next**. Review the details and select **Save and close**. 

1. Now that your custom data has been uploaded in Azure AI Studio, the data will be indexed and made available to use in the **Chat playground**. This process may take a few minutes. Once it's completed, continue to the next section of this exercise.

## Using Your Custom Data Source in the Chat Playground

1. Locate the **Chat session** section and enter the following **User message**:

    ```text
    What safety rules are required to install a clock?
    ```

1. You should see results similar to the following displayed:

    :::image type="content" source="../media/aoai-studio-chat-session-clock.png" alt-text="Azure OpenAI Studio Chat Session":::

1. Expand the **1 references** section in the chat response and notice that the *Clock A102 Installation Instructions.docx* file is listed and that you can select it to view the document.

1. Enter the following **User message**:

    ```text
    What should I do to mount the clock on the wall?
    ```

1. You should see results similar to the following displayed:

    :::image type="content" source="../media/aoai-studio-chat-session-clock-2.png" alt-text="Azure OpenAI Studio Chat Session":::

1. Now let's experiment with the Company FAQs document. Enter the following text into the **User message** field:

    ```text
    What is the company's policy on vacation time?
    ```

1. You should see that no information was found for that request. Enter the following text into the **User message** field:

    ```text
    How should I handle refund requests?
    ```

1. You should see results similar to the following displayed:

    :::image type="content" source="../media/aoai-studio-chat-session-faq.png" alt-text="Azure OpenAI Studio Chat Session":::

1. Expand the **1 references** section in the chat response and notice that the *Company FAQs.docx* file is listed and that you can select it to view the document.

1. Select **View code** at the top of the **Chat session** section.

    :::image type="content" source="../media/aoai-studio-chat-session-view-code.png" alt-text="Azure OpenAI Studio Chat Session - View Code":::

1. In the dialog that appears, note that you can switch between different languages, view the endpoint, and access the endpoint's key. Close the **Sample Code** dialog window.

    :::image type="content" source="../media/aoai-studio-chat-session-sample-code.png" alt-text="Azure OpenAI Studio Chat Session - Sample Code":::

1. Turn on the **Show raw JSON** toggle in the **Chat session*. Notice the chat session starts with a message similar to the following:

    ```json
    {
        "role": "system",
        "content": "You are an AI assistant that helps people find information."
    }
    ```

1. Now that you've created a custom data source and experimented with it in the Chat playground, let's see how you can use it in the project's application.

### Using the Bring Your Own Data Feature in the Application

1. Open the *.env* file and update the following values with your Cognitive Services endpoint, key, and index name. You copied the endpoint and key to a local file earlier in this exercise.

    ```
    AZURE_COGNITIVE_SEARCH_ENDPOINT=<COGNITIVE_SERVICES_ENDPOINT_VALUE>
    AZURE_COGNITIVE_SEARCH_KEY=<COGNITIVE_SERVICES_KEY_VALUE>
    AZURE_COGNITIVE_SEARCH_INDEX=byod-search-index
    ```

1. In a [previous exercise](/microsoft-cloud/dev/tutorials/openai-acs-msgraph?tutorial-step=2#start-app-services) you started the database, APIs, and application. You also updated the `.env` file. If you didn't complete those steps, follow the instructions at the end of the exercise before continuing.

1. Once the application has loaded in the browser, select the **Chat Help** icon in the upper-right of the application.

    :::image type="content" source="../media/chat-help-icon.png" alt-text="Chat Help Icon":::

1. The following text should appear in the chat dialog:

    ```text
    How should I handle refund requests?
    ```

1.  Select the **Get Help** button. You should see results returned from the *Company FAQs.docx* document that you uploaded earlier in Azure AI Studio. If you'd like to read through the document, you can find it in the **customer documents** folder at the root of the project.

### Exploring the Code

[!INCLUDE [Note-Open-Files-VS-Code](./tip-open-files-vs-code.md)]

1. Open the *server/apiRoutes.ts* file and locate the `completeBYOD` route. This API is called by front-end portion of the app when the **Get Help** button is selected. It retrieves the user prompt from the body and passes it to the `completeBYOD()` function in the *server/openAI.ts* file. The results are then returned to the client.

    ```typescript
    router.post('/completeBYOD', async (req, res) => {
        const { prompt } = req.body;

        if (!prompt) {
            return res.status(400).json({ 
                status: false, 
                error: 'The prompt parameter must be provided.' 
            });
        }

        let result;
        try {
            // Call OpenAI to get custom "bring your own data" completion
        result = await completeBYOD(prompt);
        }
        catch (e: unknown) {
            console.error('Error parsing JSON:', e);
        }

        res.json(result);
    });
    ```

1. Open the *server/openAI.ts* file and locate the `completeBYOD()` function.

    ```typescript
    async function completeBYOD(userPrompt: string): Promise<string> {
        const systemPrompt = 'You are an AI assistant that helps people find information.';
        // Pass that we're using Cognitive Search along with Azure OpenAI.
        return await callOpenAI(systemPrompt, userPrompt, 0, true);
    }
    ```

    This function has the following features:
    
    - The `userPrompt` parameter contains the information the user typed into the chat help dialog.
    - the `systemPrompt` variable defines that an AI assistant designed to help people find information will be used.
    - `callOpenAI()` is used to call the Azure OpenAI API and return the results. It passes the `systemPrompt` and `userPrompt` values as well as the following parameters:
        - `temperature` - The amount of creativity to include in the response. The user needs consistent (less creative) answers in this case so the value is set to 0.
        - `useBYOD` - A boolean value that indicates whether or not to use Cognitive Search along with Azure OpenAI. In this case, it's set to `true` so Cognitive Search functionality will be used.

1.  Locate the `getAzureOpenAIBYODCompletion()` function in *server/openAI.ts*. It's very similar to the `getAzureOpenAICompletion()` function you looked at in an earlier exercise but is included as a separate function to highlight a few differences that are unique to the bring your own data scenario:

    - The `byodUrl` value includes an `extensions` segment in the URL:

        ```typescript
        const byodUrl = `${OPENAI_ENDPOINT}/openai/deployments/${OPENAI_MODEL}/extensions/chat/completions?api-version=${OPENAI_API_VERSION}`;
        ```

    - It adds a `dataSources` property to the message data that is sent to Azure OpenAI. The `dataSources` property contains the Cognitive Search resource's `endpoint`, `key`, and `indexName` values that were added to the `.env` file earlier in this exercise.

        ```typescript
        const messageData: ChatGPTData = {
            max_tokens: 1024,
            temperature,
            messages: [
                { role: 'system', content: systemPrompt },
                { role: 'user', content: userPrompt }
            ],
            // Adding BYOD data source so that Cognitive Search is used with Azure OpenAI
            dataSources: [
                {
                    type: 'AzureCognitiveSearch',
                    parameters: {
                        endpoint: AZURE_COGNITIVE_SEARCH_ENDPOINT,
                        key: AZURE_COGNITIVE_SEARCH_KEY,
                        indexName: AZURE_COGNITIVE_SEARCH_INDEX
                    }
                }
            ]
        };
        ```

    - The `headersBody` object includes `chatpgpt_url` and `chatgpt_key` properties that are used to call Azure OpenAI once the Cognitive Search results are returned. 

        ```typescript
        const headersBody: OpenAIHeadersBody = {
            method: 'POST',
            headers: {
                'Content-Type': 'application/json',
                'api-key': OPENAI_API_KEY,
                chatgpt_url: byodUrl.replace('extensions/', ''),
                chatgpt_key: OPENAI_API_KEY
            },
            body: JSON.stringify(messageData),
        };
        ```

    - The response returned by Azure OpenAI includes two messsages with roles of `tool` and `assistant`. The sample application uses the second message with a `role` of `assistant` to provide the user the information they requested. In cases where you want to provide the user with additional information about the documents used to create the response, you can use the first message which includes the `url` to the document(s).

        ```json
        {
            "id": "12345678-1a2b-3c4e5f-a123-12345678abcd",
            "model": "",
            "created": 1684304924,
            "object": "chat.completion",
            "choices": [
                {
                    "index": 0,
                    "messages": [
                        {
                            "role": "tool",
                            "content": "{\"citations\": [{\"content\": \"\\nCognitive Services are cloud-based artificial intelligence (AI) services...\", \"id\": null, \"title\": \"What is Cognitive Services\", \"filepath\": null, \"url\": null, \"metadata\": {\"chunking\": \"orignal document size=250. Scores=0.4314117431640625 and 1.72564697265625.Org Highlight count=4.\"}, \"chunk_id\": \"0\"}], \"intent\": \"[\\\"Learn about Azure Cognitive Services.\\\"]\"}",
                            "end_turn": false
                        },
                        {
                            "role": "assistant",
                            "content": " \nAzure Cognitive Services are cloud-based artificial intelligence (AI) services that help developers build cognitive intelligence into applications without having direct AI or data science skills or knowledge. [doc1]. Azure Machine Learning is a cloud service for accelerating and managing the machine learning project lifecycle. [doc1].",
                            "end_turn": true
                        }
                    ]
                }
            ]
        }
        ```

        The following code is used to access the messages. Although citations aren't being used here, they're logged to the console to enable you to see the type of data that's returned.

        ```typescript
        const response = await fetch(byodUrl, headersBody);
        const completion = await response.json();
        console.log(completion);

        if (completion.error) {
            return completion.error.message;
        }

        const citations = completion.choices[0].messages[0].content.trim();
        console.log('Azure OpenAI BYOD Citations: \n', citations);
        let content = completion.choices[0].messages[1].content.trim();
        console.log('Azure OpenAI BYOD Output: \n', content);
        return content;
        ```

## Clean Up Azure Resources

1. To cleanup your resources and avoid additional charges to your Azure account, go to the Azure portal and delete the following resources:

    - The Azure Cognitive Search resource
    - The Azure Storage resource

1. A few final points to consider before moving on to the next exercise:

    - The "bring your own data" feature of Azure OpenAI is currently in preview. It's not recommended to use it in production applications at this time.
    - The sample application uses a single index in Azure Cognitive Search. You can use multiple indexes and data sources with Azure OpenAI. The `dataSources` property in the `getAzureOpenAIBYODCompletion()` function can be updated to include multiple data sources as needed.
    - Security needs to be factored this type of scenario. Users should't be able to ask questions and get results from documents that they aren't able to access.

1. Now that you've learned about Azure OpenAI, prompts, completions, and how you can use your own data, let's move on to the next exercise to learn how communication features can be used to enhance the application. If you'd like to learn more about Azure OpenAI, view the <a href="/training/modules/get-started-openai?WT.mc_id=m365-94501-dwahlin" target="_blank" rel="noopener">Get started with Azure OpenAI Service</a> training content.