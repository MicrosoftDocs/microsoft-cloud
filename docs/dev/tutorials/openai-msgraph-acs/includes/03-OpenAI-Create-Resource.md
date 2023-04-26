<!-- markdownlint-disable MD041 -->

In this exercise you will:

- Create an Azure OpenAI Service resource.
- Deploy a model.
- Update the `.env` file with values from your Azure OpenAI Service resource.

Perform the following tasks:

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

1. Open the *tutorials/openai-msgraph-acs/.env* file in Visual Studio Code.

1. Copy the **KEY 1** value and assign it to `OPENAI_API_KEY` in the *.env* file located in the root of the *tutorials/openai-msgraph-acs* folder:

    ```
    OPENAI_API_KEY=<KEY_1_VALUE>
    ```

1. Copy the **Endpoint* value and assign it to `OPENAI_ENDPOINT` in the *.env* file:

    ```
    OPENAI_ENDPOINT=<ENDPOINT_VALUE>
    ```

1. Save the *.env* file.

1. In the following steps you'll create three terminal windows in Visual Studio Code as shown next.

    :::image type="content" source="../media/vs-code-terminals.png" alt-text="Three terminal windows in Visual Studio Code":::

1. Right-click on the *.env* file in the Visual Studio Code file list and select **Open in Integrated Terminal**. Ensure that your terminal is at the root of the project before continuing.

1. Run `docker-compose up` in the window and press <kbd>Enter</kbd> to start the Postgresql server.

1. Press the **+** icon in the Visual Studio Code **Terminal toolbar** to create a 2nd terminal window. 

    :::image type="content" source="../media/vs-code-terminal-plus-icon.png" alt-text="Visual Studio Code + icon in the terminal toolbar.":::

1. `cd` into the *server/typescript* folder and run the following commands to install the dependencies and start the API server.

    - `npm install`
    - `npm start`

1. Press the **+** icon again in the Visual Studio Code **Terminal toolbar** to create a 3rd terminal window. `cd` into the *client* folder and run the following commands to install the dependencies and start the client application.

    - `npm install`
    - `npm start` 

1. A browser will launch and you'll be taken to *http://localhost:4200*. 

    :::image type="content" source="../media/openai-enabled-screenshot.png" alt-text="Application screenshot with Azure OpenAI enabled":::

1. Now that you have the application up and running, let's try out some of the application features and learn how they're built.



