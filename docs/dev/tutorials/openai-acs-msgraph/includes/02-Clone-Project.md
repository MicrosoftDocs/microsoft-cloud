<!-- markdownlint-disable MD041 -->

The sample project referenced in this tutorial is available at <a href="https://github.com/microsoft/MicrosoftCloud" target="_blank" rel="noopener">https://github.com/microsoft/MicrosoftCloud</a>. This repository includes both client-side and server-side code required to run the sample, enabling you to explore the integrated features related to artificial intelligence (AI), communication, and organizational data. Additionally, the project serves as resource to guide you in incorporating similar features into your own applications.

In this exercise you will:

- Clone a GitHub repository.
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
    AAD_CLIENT_ID=
    TEAM_ID=
    CHANNEL_ID=
    OPENAI_API_KEY=
    OPENAI_ENDPOINT=
    OPENAI_API_VERSION=2023-03-15-preview
    OPENAI_MODEL=gpt-35-turbo
    POSTGRES_USER=
    POSTGRES_PASSWORD=
    ACS_CONNECTION_STRING=
    ACS_PHONE_NUMBER=
    ACS_EMAIL_ADDRESS=
    CUSTOMER_EMAIL_ADDRESS=
    CUSTOMER_PHONE_NUMBER=
    API_BASE_URL=http://localhost:3000/api/
    ```

1. Assign the following values to `POSTGRES_USER` and `POSTGRES_PASSWORD` in *.env*. These values will be used by the API server to connect to the local PostgreSQL database.

    ```
    POSTGRES_USER=web
    POSTGRES_PASSWORD=web-password
    ```

1. Now that you have the project in place, let's try out some of the application features and learn how they're built. Select the **Next** button below to continue or jump to a specific exercise using the table of contents.
