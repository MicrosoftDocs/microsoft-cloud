<!-- markdownlint-disable MD041 -->

In this exercise, you will deploy a Large Language Model (LLM) to a real-time endpoint in Azure Machine Learning Studio. This will allow you to consume the model in your application later.

A Large Language Model is a type of natural language processing model that is trained on a large amount of text data. These models can generate human-like text and code and are used for applications like text generation, translation, question answering, and more.

In this exercise, you will:

- Open Azure Machine Learning Studio.
- Configure an endpoint that can be used in your application.

Let's get started by opening Machine Learning Studio!

1. Open Azure Machine Learning Studio using the [**Studio web URL**](https://ml.azure.com).

    :::image type="content" source="../media/azureml-studio-url.png" alt-text="Azure Machine Learning Service in the Azure Portal":::

2. Select **All workspaces** to access the shared resources in your tenant.

    :::image type="content" source="../media/azureml-studio-workspace.png" alt-text="Azure Machine Learning Service in the Azure Portal":::

3. Select **Models** under the **Shared assets** from the left side menu to view the model template that you want to deploy.

    :::image type="content" source="../media/azureml-studio-model-registry.png" alt-text="Creating an Azure Machine Learning Workspace in the Azure Portal":::

    > [!NOTE]
    > Here you can find binary files for different machine learning models trained with specific algorithms on training datasets and then able to produce predictions and inferences on additional, larger datasets provided by AzureML and HuggingFace.
    >
    > In this tutorial, you choose GPT-2 for text generation. This model can effectively be used for different applications like sentiment analysis and many more.
    >
    > Feel free to try other models later.

4. Type *gpt2* in the **search bar** at the top of the models page followed by selecting the **gpt2** model provided by the **azureml** registry from the options that appear.

    :::image type="content" source="../media/search-azureml-registry.png" alt-text="Searching for GPT-2 model in the Azure Machine Learning Workspace Models Page":::

5. Select **Deploy** followed by **Real-time endpoint**.

    :::image type="content" source="../media/azureml-studio-gpt2-model.png" alt-text="Creating an Azure Machine Learning Workspace in the Azure Portal":::

6. Select your **Subscription** and your **Workspace**. Select **Proceed to workspace**.

    :::image type="content" source="../media/azureml-studio-select-workspace.png" alt-text="Creating an Azure Machine Learning Workspace in the Azure Portal":::

7. Perform the following tasks:
    - Select the **Virtual machine** you'd like to use.
    - Enter an **Instance count** of at least 3.

    > [!TIP]
    > For high availability, Microsoft recommends you set it to at least 3.

    - Enter an **Endpoint name**. Any unique name will work.
    - Enter a **Deployment name**.

    :::image type="content" source="../media/azureml-studio-deploy-gpt2.png" alt-text="Configuration of an Azure Machine Learning Workspace in the Azure Portal":::

    > [!WARNING]
    > Selecting a large **Virtual machine** will incur more charges and increasing the number of instances from the selected machine will also contribute to that.
    > This may increase the speed and/or availability of your model but at the cost of paying more money.

8. Select **Deploy**.

    > [!NOTE]
    > While the deployment is being created, you can proceed to the next step as it will take a couple of minutes.
    >
    > In addition to creating and using models in Azure Machine Learning Workspace, your application can also use models available in Azure OpenAI.
