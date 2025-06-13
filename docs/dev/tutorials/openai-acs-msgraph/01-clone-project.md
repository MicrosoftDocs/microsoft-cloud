---
title: Clone the Project
description: Learn how to clone the GitHub repository and set up the project files needed for this tutorial on integrating Azure OpenAI, Azure Communication Services, and Microsoft Graph.
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

#customer intent: As a developer, I want to integrate Azure OpenAI, Azure Communication Services, and Microsoft Graph/Microsoft Graph Toolkit into a Line of Business application.

---

<!-- markdownlint-disable MD041 -->

# Clone the Project

The code project used in this tutorial is available at <a href="https://github.com/microsoft/MicrosoftCloud" target="_blank" rel="noopener">https://github.com/microsoft/MicrosoftCloud</a>. The project's repository includes both client-side and server-side code required to run the project, enabling you to explore the integrated features related to artificial intelligence (AI), communication, and organizational data. Additionally, the project serves as resource to guide you in incorporating similar features into your own applications.

In this exercise you will:

- Clone the GitHub repository.
- Add an *.env* file into the project and update it.

Before proceeding, ensure that you have all of the prerequisites installed and configured as outlined in the [Prerequisites](/microsoft-cloud/dev/tutorials/openai-acs-msgraph/#prerequisites) section of this tutorial.

### Clone the GitHub Repository and Create an `.env` File

1. Run the following command to clone the [Microsoft Cloud GitHub Repository](https://github.com/microsoft/MicrosoftCloud) to your machine.

    ```console
    git clone https://github.com/microsoft/MicrosoftCloud
    ```

1. Open the *MicrosoftCloud/samples/openai-acs-msgraph* folder in Visual Studio Code.

    > [!NOTE]
    > Although we'll use Visual Studio Code throughout this tutorial, any code editor can be used to work with the sample project.

1. Notice the following folders and files:

    - **client**: Client-side application code.
    - **server**: Server-side API code.
    - **docker-compose.yml**: Used to run a local PostgreSQL database.

1. Rename the *.env.example* in the root of the project to *.env*. 

1. Open the *.env* file and take a moment to look through the keys that are included:

    ```
    ENTRAID_CLIENT_ID=
    TEAM_ID=
    CHANNEL_ID=
    OPENAI_API_KEY=
    OPENAI_ENDPOINT=
    OPENAI_MODEL=gpt-4o
    OPENAI_API_VERSION=2024-05-01-preview
    POSTGRES_USER=
    POSTGRES_PASSWORD=
    ACS_CONNECTION_STRING=
    ACS_PHONE_NUMBER=
    ACS_EMAIL_ADDRESS=
    CUSTOMER_EMAIL_ADDRESS=
    CUSTOMER_PHONE_NUMBER=
    API_PORT=3000
    AZURE_AI_SEARCH_ENDPOINT=
    AZURE_AI_SEARCH_KEY=
    AZURE_AI_SEARCH_INDEX=
    ```

1. Update the following values in *.env*. These values will be used by the API server to connect to the local PostgreSQL database.

    ```
    POSTGRES_USER=web
    POSTGRES_PASSWORD=web-password
    ```

1. Now that you have the project in place, let's try out some of the application features and learn how they're built. Select the **Next** button below to continue or jump to a specific exercise using the table of contents.

### Next Step

> [!div class="nextstepaction"]
> [AI: Create an Azure OpenAI Resource and Deploy a Model](./02-openai-create-resource.md)
