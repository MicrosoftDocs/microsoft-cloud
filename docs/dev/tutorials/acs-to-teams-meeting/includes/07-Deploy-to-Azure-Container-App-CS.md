<!-- markdownlint-disable MD041 -->

### Deploy to Azure Functions

1. Open the `samples/acs-video-to-teams-meeting/server/csharp/GraphACSFunctions.sln` project in Visual Studio.

1. Right-click on the `GraphACSFunctions` project and select **Publish**.

1. Select **Azure** in the target section, then click **Next**.

1. Select **Azure Function App (Windows)**, then click **Next**.

1. Select your subscription, then select **+ Create New**.

1. Enter the following information:

    - Function name: A globally unique name is required. Example: **acsFunctions<YOUR_LAST_NAME>**.
    - Subscription: Select your subscription.
    - Resource Group: Select a resource group you created earlier in this tutorial, or you can also create a new one.
    - Hosting Plan: Consumption plan.
    - Location: Select the region you'd like to deploy to.
    - Azure Storage: Create a new one. (You can also select an existing storage account.)
    - Azure Insights: Create a new one. (You can also select an existing storage account.)

    > [!NOTE]
    > A globally unique name is required. You can make the name more unique by adding a number or your last name to the end of the name.

1. Once the Azure Function App is created you'll see a confirmation screen. Make sure the **Run from package** option is selected, then select **Finish**.

1. Select **Publish** to deploy the function to Azure.

1. Once the function is deployed to Azure, go to the Azure portal and select the Function App you created.

1. Copy the URL for the function you deployed. You'll use the value later in this exercise.

1. Select **Settings --> Configuration** in the left menu.

1. Select **+ New application setting** button and add the following keys and values in the **Application settings**. You can retreive these values from `local.settings.json` in the `GraphACSFunctions` project.

    ```
    # Retrieve these values from local.settings.json
    TENANT_ID: <YOUR_VALUE>
    CLIENT_ID: <YOUR_VALUE>
    CLIENT_SECRET: <YOUR_VALUE>
    USER_ID: <YOUR_VALUE>
    ACS_CONNECTION_STRING: <YOUR_VALUE>
    ```

1. Select the **Save** button to save the settings.

1. Finally, you need to enable CORS (Cross-Origin Resource Sharing) for the function app to make the function app's APIs accessible from outside of your domain. Select **Settings --> CORS** in the left menu.

1. Enter `*` (accessible from any domains) in the **Allowed Origins** textbox, then select the **Save** button.