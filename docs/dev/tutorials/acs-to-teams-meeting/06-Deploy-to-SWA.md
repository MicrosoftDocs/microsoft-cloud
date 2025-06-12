---
title: Deploy the App to Azure Static Web Apps
description: In this tutorial you'll learn how Azure Communication Services can be used in a custom React application to allow a user to make an audio/video call into a Microsoft Teams meeting. You'll learn about the different building blocks that can be used to make this scenario possible and be provided with hands-on steps to walk you through the different Microsoft Cloud services involved.
author: DanWahlin
ms.author: dwahlin
ms.date: 06/12/2025
ms.topic: tutorial
ms.service: microsoft-cloud-for-developers

categories:
  - developer-tools
products:
  - azure
  - github
ms.custom:
  - fcp
  - team=cloud_advocates

#customer intent: As a developer, I want to deploy my application to Azure Static Web Apps.
---

<!-- markdownlint-disable MD041 -->

# Deploy the App to Azure Static Web Apps

In this exercise you'll learn how to deploy the ACS React app and the Azure Functions to the cloud using Azure Static Web Apps.

:::image type="content" source="./media/5-deploy-swa.png" alt-text="Azure Static Web Apps":::

1. Visit the [Azure portal](https://portal.azure.com) in your browser and sign in.

1. Type *static web apps* in the top search bar and select **Static Web Apps** from the options that appear.

    :::image type="content" source="./media/search-swa-portal.png" alt-text="Azure Static Web Apps":::

1. Select **Create** in the toolbar.

1. Perform the following tasks:
    - Select your subscription.
    - Select the resource group to use (create a new one if needed). You can use the same resource group that you used for ACS if you'd like.
    - Enter an Azure Static Web Apps name of *acs-to-teams-meeting*.
    - Select the **Free** plan type.
    - Select a region.

1. Select the GitHub radio button and sign in to your GitHub account.

1. After signing in, select your GitHub:
    - Organization
    - Repository: Select the **MicrosoftCloud** repository you forked earlier in this tutorial
    - Branch: Select **main**

1. In the **Build Details** section perform the following tasks:
    - Build Presets: **React**
    - App location: **/samples/acs-to-teams-meeting/client/react**
    - Api location: 
    
    # [TypeScript](#tab/typescript)

    **/samples/acs-to-teams-meeting/server/typescript**

    # [C#](#tab/csharp)

    **/samples/acs-to-teams-meeting/server/csharp**

    ---
    
    - Output location: **build**

1. Select **Review + create**.

1. Review the details and select **Create**.

1. Notice the URL that is created for your static web app. Copy the URL shown on the Overview screen to a file. You'll need it later in this exercise.

1. Select **Settings --> Configuration** for your new static web app.

1. Add all of the following key/value pairs into the **Application settings** by selecting the **+ Add** button. Get the values from your *local.settings.json* in the *server/typescript* folder.

    ```text
    # Get the values from your local.settings.json file
    TENANT_ID: <YOUR_VALUE>
    CLIENT_ID: <YOUR_VALUE>
    CLIENT_SECRET: <YOUR_VALUE>
    USER_ID: <YOUR_VALUE>
    ACS_CONNECTION_STRING: <YOUR_VALUE>
    ```

1. Select the **Save** button at the top of the Configuration screen in the Azure portal.

1. Now that you've finished setting up the Azure Static Web App, go back to your GitHub repository (the one you forked earlier) in the browser and notice a *.yml* file has been added into the *.github/workflows* folder. 

1. Pull the latest changes from your GitHub repository to your machine by running the following command from the root of your local *MicrosoftCloud* repository:

    ```bash
    git pull
    ```

1. Open the *.yml* file in the *.github/workflows* folder in VS Code and add the following YAML immediately after the `###### End of Repository/Build Configurations ######` comment. Replace the *<YOUR_AZURE_SWA_DOMAIN>* placeholders with your Azure Static Web Apps URL value. 

    > IMPORTANT: Ensure that the `env:` property is indented properly. It should match up with the indentation of the `with:` property above it.

    ```yaml
    env: # Add environment variables here
        REACT_APP_ACS_USER_FUNCTION: https://<YOUR_AZURE_SWA_DOMAIN>/api/httpTriggerAcsToken
        REACT_APP_TEAMS_MEETING_FUNCTION: https://<YOUR_AZURE_SWA_DOMAIN>/api/httpTriggerTeamsUrl
    ```

    > [!NOTE]
    > This will add environment variables into the build process for the React app so that it knows what APIs to call to get the ACS user token as well as to create a Teams meeting.

1. Save the *.yml* file and push the changes up to your GitHub repository. This will trigger a new build of the React application and then deploy it to your Azure Static Web App. 

1. Once the build process completes, visit the URL for your Azure Static Web App and you should see the application running.

1. You've successfully deployed your application using Azure Static Web Apps!

## Next Step

> [!div class="nextstepaction"]
> [Deploy the App to Azure Functions and Azure Container Apps](07-Deploy-to-Azure-Container-Apps.md)
