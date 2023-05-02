<!-- markdownlint-disable MD041 -->

To get started using OpenAI in your applications, you need to create an Azure OpenAI Service and deploy a model that can be used to perform tasks such as converting natural language to SQL, generating email/SMS message content, and more.

In this exercise you will:

- Create an Azure OpenAI Service resource.
- Deploy a model.
- Update the *.env* file with values from your Azure OpenAI Service resource.

### Creating an Azure OpenAI Service Resource

1. Visit the [Azure Portal](https://portal.azure.com) in your browser and sign in.

1. Type *openai* in the **search bar** at the top of the portal page and select **Azure OpenAI** from the options that appear.

    :::image type="content" source="../media/search-openai-portal.png" alt-text="Azure OpenAI Service in the Azure Portal":::

1. Select **Create** in the toolbar.

    > [!NOTE]
    > If you see a message about completing an application form to enable Azure OpenAI on your subscription, select the **Click here to request access to Azure OpenAI service** link and complete the form. Once you've completed the form, you'll need to wait for the Azure OpenAI team to approve your request. After receiving your approval notice, you can go back through this exercise and create the resource.

1. Perform the following tasks:
    - Select your Azure subscription.
    - Select the resource group to use (create a new one if needed).
    - Select the region you'd like to use.
    - Enter the resource name. It must be a unique value.
    - Select the **Standard S0** pricing tier.

1. Select **Next** until you get to the **Review + submit** screen. Select **Create**.

1. Once your Azure OpenAI Service resource is created, navigate to it, and select **Model deployments**.

1. Select **Create** in the toolbar.

1. Enter the following values:
    - Model deployment name: **gpt-35-turbo**.
    - Model: **gpt-35-turbo**.
    - Version: **1**

    > [!NOTE]
    > Azure OpenAI supports [several different types of models](https://learn.microsoft.com/azure/cognitive-services/openai/concepts/models) such as text-ada-001, text-curie-001, text-divinci-003, and gpt-35-turbo. Each model can be used to handle different scenarios.

1. Select **Save**.

1. Select **Keys and Endpoint**. Locate the **KEY 1** and **Endpoint** values.

    :::image type="content" source="../media/openai-keys-endpoint.png" alt-text="OpenAI Keys and Endpoint" border="false":::

### Update the Project's `.env` File

1. Open the `samples/openai-msgraph-acs/.env` file in Visual Studio Code.

1. Copy the **KEY 1** value and assign it to `OPENAI_API_KEY` in the *.env* file located in the root of the *tutorials/openai-msgraph-acs* folder:

    ```
    OPENAI_API_KEY=<KEY_1_VALUE>
    ```

1. Copy the **Endpoint* value and assign it to `OPENAI_ENDPOINT` in the *.env* file:

    ```
    OPENAI_ENDPOINT=<ENDPOINT_VALUE>
    ```

1. Save the *.env* file.

### Start the Application Services

It's time to start up your application services including the database, web server, and API server.

[!INCLUDE [Start-Services](./Start-Services.md)]



