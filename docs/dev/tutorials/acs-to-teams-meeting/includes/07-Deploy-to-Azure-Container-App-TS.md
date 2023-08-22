<!-- markdownlint-disable MD041 -->

Let's get started by using VS Code to deploy the functions code to Azure Functions.

1. Open the *samples/acs-to-teams-meeting/server/typescript* project folder in Visual Studio Code.

1. You should have already run `npm install` and see a *node_modules* folder in the project root. If not, open a command window and run `npm install` to install the dependencies.

1. Open the VS Code command pallet (<kbd>Shift+Cmd+P</kbd> on Mac | <kbd>Shift+Ctrl+P</kbd> on Windows), and select **Azure Functions: Create Function App in Azure**.

    :::image type="content" source="../media/create-function-app-in-azure.png" alt-text="Create Function App in Azure":::

1. You'll be prompted to enter the following information:

    - Your Azure subscription name.
    - The function name: **acsFunctions<YOUR_LAST_NAME>**.

    > [!NOTE]
    > A globally unique name is required. You can make the name more unique by adding a number or your last name to the end of the name.

    - The runtime stack - Select the latest **Node.js LTS** version.
    - The region (select any region you'd like).

1. Once the Azure Function App is created, you'll see a message about viewing the details. 

1. Go back to the command pallet in VS Code and select **Azure Functions: Deploy to Function App**. You'll be asked to select your subscription and the Function App name you created earlier.

1. Once the function is deployed to Azure, do the following:

    - Select the Azure extension in VS Code (click the Azure icon in the sidebar).
    - Expand your subscription.
    - Expand your Function App.
    - Right-click on the function and select **Browse Website**. Ensure the function app is working correctly before proceeding.

1. Copy the Azure Function domain to a local file. You'll use the value later in this exercise.