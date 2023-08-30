<!-- markdownlint-disable MD041 -->

In this exercise, you'll create an Azure Machine Learning Workspace to be able to provision your own machine learning artifacts.

An Azure Machine Learning Workspace is a top-level resource providing a centralized place for your resources and assets, including Models, Endpoints, Compute, and many more.

In this exercise, you will:

    - Open Azure portal.
    - Create an Azure Machine Learning Workspace.

Let's get started by opening Azure!

1. Visit the [Azure portal](https://portal.azure.com) in your browser and sign in.

2. Type *Azure Machine Learning* in the **search bar** at the top of the portal page and select **Azure Machine Learning** from the options that appear.

    :::image type="content" source="../media/search-azureml-portal.png" alt-text="Azure Machine Learning Service in the Azure portal":::

3. Select **Create** from the toolbar. Select **New workspace** to create a new machine learning workspace.

    :::image type="content" source="../media/create-azureml-workspace.png" alt-text="Creating an Azure Machine Learning Workspace in the Azure portal":::

4. Perform the following tasks:
    - Select your **Subscription**.
    - Select the **Resource group** to use (create a new one if needed).
    - Enter the **Workspace name**. It must be a unique value.
    - Select the **Region** you'd like to use.

    :::image type="content" source="../media/azureml-workspace-metrics.png" alt-text="Configuration of an Azure Machine Learning Workspace in the Azure portal":::

    > [!NOTE]
    > While populating your **Workspace details**, all the other options including: **Storage account**, **Key vault**, **Application insights**, and **Container registry**, will be set automatically.

5. Select **Review + create**. Select **Create**.

6. Wait for the deployment to finish. Select **Go to resource**.

    :::image type="content" source="../media/azureml-workspace-deployment.png" alt-text="Deployment in Progress Azure Machine Learning Workspace in the Azure portal":::
