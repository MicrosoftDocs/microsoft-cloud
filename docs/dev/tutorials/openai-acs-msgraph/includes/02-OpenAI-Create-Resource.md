<!-- markdownlint-disable MD041 -->

To get started using Azure OpenAI in your applications, you need to create an Azure OpenAI Service and deploy a model that can be used to perform tasks such as converting natural language to SQL, generating email/SMS message content, and more.

In this exercise you will:

- Create an Azure OpenAI Service resource.
- Deploy a model.
- Update the *.env* file with values from your Azure OpenAI service resource.

:::image type="content" source="../media/scenario-overview-no-title.png" alt-text="Microsoft Cloud scenario overview" border="false":::

### Create an Azure OpenAI Service Resource

1. Visit the [Azure portal](https://portal.azure.com) in your browser and sign in.

1. Enter *openai* in the **search bar** at the top of the portal page and select **Azure OpenAI** from the options that appear.

    :::image type="content" source="../media/search-openai-portal.png" alt-text="Azure OpenAI Service in the Azure portal":::

1. Select **Create** in the toolbar.

    > [!NOTE]
    > While this tutorial focuses on Azure OpenAI, if you have an OpenAI API key and would like to use it, you can skip this section and go directly to the <a id="update-env-file">**Update the Project's .env File**</a> section below. Assign your OpenAI API key to `OPENAI_API_KEY` in the *.env* file (you can ignore any other `.env` instructions related to OpenAI).

1. Azure OpenAI models are available in specific regions. Visit the [Azure OpenAI model availability](/azure/ai-services/openai/concepts/models?WT.mc_id=m365-94501-dwahlin#global-standard-model-availability) document to learn which regions support the gpt-4o model used in this tutorial. 

1. Perform the following tasks:
    - Select your Azure subscription.
    - Select the resource group to use (create a new one if needed).
    - Select a region where the gpt-4o model is supported based on the document you looked at earlier.
    - Enter the resource name. It must be a unique value.
    - Select the **Standard S0** pricing tier.

1. Select **Next** until you get to the **Review + submit** screen. Select **Create**.

1. Once your Azure OpenAI resource is created, navigate to it and select **Resource Management** --> **Keys and Endpoint** .

1. Locate the **KEY 1** and **Endpoint** values. You'll use both values in the next section so copy them to a local file.

    :::image type="content" source="../media/openai-keys-endpoint.png" alt-text="OpenAI Keys and Endpoint" border="false":::

1. Select **Resource Management** --> **Model deployments**. 

1. Select the **Manage Deployments** button to go to Azure OpenAI Studio.

1. Select **Deploy model** --> **Deploy base model** in the toolbar.

    :::image type="content" source="../media/aoai-deploy-model.png" alt-text="Azure OpenAI Deploy base model" border="true":::

1. Select **gpt-4o** from the list of models and select **Confirm**.

    > [!NOTE]
    > Azure OpenAI supports [several different types of models](/azure/ai-services/openai/concepts/models?WT.mc_id=m365-94501-dwahlin). Each model can be used to handle different scenarios.

1. The following dialog will display. Take a moment to examine the default values that are provided.

    :::image type="content" source="../media/aoai-studio-create-model-deployment.png" alt-text="Azure OpenAI Create Model Deployment" border="true":::

1. Change the **Tokens per Minute Rate Limit (thousands)** value to **100K**. This will allow you to make more requests to the model and avoid hitting the rate limit as you perform the steps that follow.

1. Select **Deploy**.

1. Once the model is deployed, select **Playgrounds** --> **Chat**.

1. The **Deployment** dropdown should display the **gpt-4o** model. 

1. Take a moment to read through the **System message** text that's provided. This tells the model how to act as the user interacts with it. 

1. Locate the textbox in the chat area and enter **Summarize what Generative AI is and how it can be used**. Select <kbd>Enter</kbd> to send the message to the model and have it generate a response.

1. Experiment with other prompts and responses. For example, enter **Provide a short history about the capital of France** and notice the response that's generated.

<a id="update-env-file"></a>
### Update the Project's `.env` File

1. Go back to Visual Studio Code and open the `.env` file at the root of the project.

1. Copy the **KEY 1** value from your Azure OpenAI resource and assign it to `OPENAI_API_KEY` in the *.env* file located in the root of the *openai-acs-msgraph* folder:

    ```
    OPENAI_API_KEY=<KEY_1_VALUE>
    ```

1. Copy the **Endpoint* value and assign it to `OPENAI_ENDPOINT` in the *.env* file. Remove the `/` character from the end of the value if it's present.

    ```
    OPENAI_ENDPOINT=<ENDPOINT_VALUE>
    ```

    > [!NOTE]
    > You'll see that values for `OPENAI_MODEL` and `OPENAI_API_VERSION` are already set in the *.env* file. The model value is set to **gpt-4o** which matches the model deployment name you created earlier in this exercise. The API version is set to a supported value defined in the [Azure OpenAI reference documentation](/azure/ai-services/openai/reference?WT.mc_id=m365-94501-dwahlin#chat-completions).

1. Save the *.env* file.

<a id="start-app-services"></a>
### Start the Application Services

It's time to start up your application services including the database, API server, and web server.

[!INCLUDE [Start-Services](./Start-Services.md)]



