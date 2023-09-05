<!-- markdownlint-disable MD041 -->

In this exercise, you will open the Power Automate Cloud Flow and add the secrets that were created when you deployed the Large Language Model (LLM) to enable communication between the Power App and the deployed Model.

A Power Automate Cloud Flow is an automation capability that can be integrated with other services to perform an action when triggered. For example, it can send our text to the deployed model, receive the response, and send it to Power Apps with a click of a button.

In this exercise, you will:

- Open a Canvas App.
- Get secrets from an Azure Machine Learning endpoint.
- Edit a Power Automate Cloud Flow.

Let's get started by opening the Solution!

1. Open the solution by selecting it's **Display name**.

2. Select the canvas app **Help Me Write Display name** to open the application in edit mode.

    > [!NOTE]
    > You can edit the application if you want to change colors, fonts, and backgrounds or leave it as it is.

    :::image type="content" source="../media/powerapps-open-canvas-app.png" alt-text="Open Canvas Application in Power Apps":::

3. Select the **Power Automate** Icon from the left side menu.

    :::image type="content" source="../media/powerapps-select-power-automate.png" alt-text="Select Power Automate from Power Apps":::

4. Hover over the flow name **HelpMeWriteFlow** and select the **three dots**. Select **Edit**.

    :::image type="content" source="../media/power-automate-flow-edit.png" alt-text="Edit Power Automate Flow from Power Apps":::

5. Go back to [Azure Machine Learning Studio](https://ml.azure.com) to get the secrets.

6. In the **endpoints** tab you should see that the deployment has completed:

    :::image type="content" source="../media/azureml-studio-deployed-endpoint.png" alt-text="Deployed Endpoint in Azure ML Studio":::

7. Perform the following tasks:
    - Select **Consume**.
    - Copy the **REST endpoint**.

    > [!NOTE]
    > This is the **ML Endpoint** variable in the Power Automate Flow.

    - Copy the **Primary** authentication key.

    > [!NOTE]
    > This is the **ML API Key** variable in the Power Automate Flow.

    :::image type="content" source="../media/azureml-studio-consume-endpoint.png" alt-text="Consume Endpoint in Azure ML Studio":::

    > [!TIP]
    > If you scroll down inside the **Consume** tab, you will find a code sample ready to use in three different programming languages: **Python**, **C#**, and **R**.
    > You can copy and paste it anywhere, and it will send a request to the deployed model and get a response back from it. For example, it could be used in your website or within a serverless function.

8. Go back to **Power Automate** and add the copied values in the places shown below:

    :::image type="content" source="../media/power-automate-flow.png" alt-text="Adding Secrets in Power Automate Cloud Flow":::

9. Select **Save** and close the flow using the **X** button.
