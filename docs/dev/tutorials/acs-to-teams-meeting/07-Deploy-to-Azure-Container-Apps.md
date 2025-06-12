---
title: Deploy the App to Azure Functions and Azure Container Apps
description: Learn how to deploy the Microsoft Graph and ACS functions to Azure Functions and the React app to Azure Container Apps. This module covers containerization and cloud deployment.
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

#customer intent: As a developer, I want to deploy my application to Azure Functions and Azure Container Apps.
---

<!-- markdownlint-disable MD041 -->

# Deploy the App to Azure Functions and Azure Container Apps

> [!IMPORTANT]
> In addition to the [pre-requisites listed for this tutorial](/microsoft-cloud/dev/tutorials/acs-to-teams-meeting), you'll also need to install the following tools on your machine to complete this exercise.
>
> - [Azure CLI](https://learn.microsoft.com/cli/azure/install-azure-cli)
> - If you're using VS Code, install the [Azure Functions extension](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions)
>

In this exercise you'll learn how to deploy the Microsoft Graph and ACS functions discussed in earlier exercises to Azure Functions. You'll also build a container image and deploy it to Azure Container Apps.

:::image type="content" source="./media/6-deploy-container-apps.png" alt-text="Azure Container Apps":::

### Deploy to Azure Functions

# [C#](#tab/csharp)

[!INCLUDE [CSharp](./includes/07-deploy-to-azure-container-app-CS.md)]

# [TypeScript](#tab/typescript)

[!INCLUDE [TypeScript](./includes/07-deploy-to-azure-container-app-TS.md)]

---

### Deploy to Azure Container Apps

1. The first task you'll perform is to create a new [Azure Container Registry (ACR)](https://learn.microsoft.com/azure/container-registry/container-registry-get-started-azure-cli) resource. Once the registry is created, you'll build an image and push it to the registry.

1. Open a command window and run the following command to login to your Azure subscription:

    ```console
    az login
    ```

1. Add the following shell variables substituting your values for the placeholders as appropriate. Add your *<GITHUB_USERNAME>* as a lowercase value and substitute your Azure Functions domain for the *<AZURE_FUNCTIONS_DOMAIN>* value (include the `https://` in the domain value).

    # [Bash](#tab/bash)

    ```bash
    GITHUB_USERNAME="<YOUR_GITHUB_USERNAME>"
    RESOURCE_GROUP="<YOUR_RESOURCE_GROUP_NAME>"
    ACR_NAME="aca"$GITHUB_USERNAME
    AZURE_FUNCTIONS_DOMAIN="<YOUR_AZURE_FUNCTIONS_URL>"
    ```


    # [PowerShell](#tab/powershell)

    ```powershell
    $GITHUB_USERNAME="<YOUR_GITHUB_USERNAME>"
    $RESOURCE_GROUP="<YOUR_RESOURCE_GROUP_NAME>"
    $ACR_NAME="aca"+$GITHUB_USERNAME
    $AZURE_FUNCTIONS_DOMAIN="<YOUR_AZURE_FUNCTIONS_URL>"
    ```

1. Create a new *Azure Container Registry* resource by running the following command:

    # [Bash](#tab/bash)

    ```bash
    az acr create \
        --resource-group $RESOURCE_GROUP \
        --name $ACR_NAME \
        --sku Basic \
        --admin-enabled true
    ```

    # [PowerShell](#tab/powershell)

    ```powershell
    az acr create `
        --resource-group $RESOURCE_GROUP `
        --name $ACR_NAME `
        --sku Basic `
        --admin-enabled true
    ```

1. Open the *client/react/Dockerfile* file in your editor and notice that the following tasks are performed:

    - The React application is built and assigned to the *build* stage.
    - The nginx server is configured and the output of the *build* stage is copied into the nginx server image.

1. Build the container image in Azure by running the following command from the root of the *client/react* folder. Replace *<YOUR_FUNCTIONS_DOMAIN>* with your Azure Functions domain that you copied to a local file earlier in this exercise.

    # [Bash](#tab/bash)

    ```bash
    az acr build --registry $ACR_NAME --image acs-to-teams-meeting \
      --build-arg REACT_APP_ACS_USER_FUNCTION=$AZURE_FUNCTIONS_DOMAIN/api/httpTriggerAcsToken \
      --build-arg REACT_APP_TEAMS_MEETING_FUNCTION=$AZURE_FUNCTIONS_DOMAIN/api/httpTriggerTeamsUrl .
    ```

    # [PowerShell](#tab/powershell)

    ```powershell
    az acr build --registry $ACR_NAME --image acs-to-teams-meeting `
      --build-arg REACT_APP_ACS_USER_FUNCTION=+$AZURE_FUNCTIONS_DOMAIN+/api/httpTriggerAcsToken `
      --build-arg REACT_APP_TEAMS_MEETING_FUNCTION=+$AZURE_FUNCTIONS_DOMAIN+/api/httpTriggerTeamsUrl .
    ```

1. Run the following command to list the images in your registry. You should see your new image listed.
    
    ```console
    az acr repository list --name $ACR_NAME --output table
    ```

1. Now that the image is deployed, you need to create an Azure Container App that can run the container.

1. Visit the [Azure portal](https://portal.azure.com) in your browser and sign in.

1. Type *container apps* in the top search bar and select **Container Apps** from the options that appear.

1. Select **Create** in the toolbar.

    > [!NOTE]
    > Although you're using the Azure portal, a Container App can also be created by using the Azure CLI. For more information, see [Quickstart: Deploy your first container app](https://learn.microsoft.com/azure/container-apps/get-started). You'll see an example of how the Azure CLI can be used at the end of this exercise as well.

1. Perform the following tasks:
    - Select your subscription.
    - Select the resource group to use (create a new one if needed). You can use the same resource group that you used for your ACS resource if you'd like. Copy your resource group name to the same local file where you stored your Azure Functions domain.
    - Enter a Container app name of **acs-to-teams-meeting**.
    - Select a region.
    - Select **Create new** in the **Container Apps Environment** section.
    - Enter an **Environment name** of **acs-to-teams-meeting-env**.
    - Select the **Create** button.
    - Select **Next: App settings >**.

1. Enter the following values in the **Create Container App** screen:

    - Deselect the **Use quickstart image** checkbox.
    - Name: **acs-to-teams-meeting**
    - Image source: **Azure Container Registry**
    - Registry: **<YOUR_ACR_REGISTRY_NAME>.azurecr.io**
    - Image: **acs-to-teams-meeting**
    - Image tag: **latest**
    - CPU and Memory: **0.25 CPU cores, -.5 Gi memory**

1. In the **Application ingress settings** section, do the following:

    - Select the **Enabled** checkbox.
    - Select the **Accepting traffic from anywhere** radio button.

    This will create an entry point (ingress) for your React application and allow it to be called from anywhere. Azure Container Apps redirects all traffic to HTTPS.

    - Target Port: **80**

1. Select **Review + create**. Once validation passes, select the **Create** button.

   If you get an error it may be due to your container apps environment being inactive for too long. The simplest solution will be to go through the
   process of creating the container app again. Alternatively, you can run the following command to create the container app using the Azure CLI:

    # [Bash](#tab/bash)

    ```bash
    az containerapp create --name acs-to-teams-meeting --resource-group $RESOURCE_GROUP \
        --location westus --image acs-to-teams-meeting \
        --cpu 0.25 --memory 0.5 --environment-name acs-to-teams-meeting-env \
        --ingress-enabled true --ingress-target-port 80 --ingress-type External \
        --ingress-protocol Https --ingress-traffic Anywhere
    ```

    # [PowerShell](#tab/powershell)

    ```powershell
    az containerapp create --name acs-to-teams-meeting --resource-group $RESOURCE_GROUP `
        --location westus --image acs-to-teams-meeting `
        --cpu 0.25 --memory 0.5 --environment-name acs-to-teams-meeting-env `
        --ingress-enabled true --ingress-target-port 80 --ingress-type External `
        --ingress-protocol Https --ingress-traffic Anywhere
    ```

1. Once your container app deployment completes, navigate to it in the Azure portal and select the **Application Url** on the **Overview** screen to view the application running in the nginx container!

## Next Step

> [!div class="nextstepaction"]
> [Congratulations!](08-Congratulations.md)




