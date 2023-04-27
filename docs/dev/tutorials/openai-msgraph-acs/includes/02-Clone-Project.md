<!-- markdownlint-disable MD041 -->

The sample project referenced in this tutorial is available at https://github.com/microsoft/MicrosoftCloud. This repository includes both client-side and server-side code required to run the sample, enabling you to explore the integrated features related to artificial intelligence (AI), communication, and organizational data. Additionally, the project serves as a valuable resource to inspire and guide you in incorporating similar features into your own applications.

In this exercise you will:

- Clone a GitHub repository.
- Add an *.env* file into the project and update it.

Perform the following tasks:

1. Run the following command to clone the [Microsoft Cloud GitHub Repository](https://github.com/microsoft/MicrosoftCloud) to your machine.

    ```console
    git clone https://github.com/microsoft/MicrosoftCloud
    ```

1. Open the *MicrosoftCloud/tutorials/openai-msgraph-acs* folder in Visual Studio Code.

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
    POSTGRES_USER=
    POSTGRES_PASSWORD=
    ACS_CONNECTION_STRING=
    ACS_PHONE_NUMBER=
    ACS_EMAIL_ADDRESS=
    CUSTOMER_EMAIL_ADDRESS=>
    CUSTOMER_PHONE_NUMBER=
    API_BASE_URL=http://localhost:3000/api/
    ```

1. Assign the following values to `POSTGRES_USER` and `POSTGRES_PASSWORD` in *.env*.

    ```
    POSTGRES_USER=web
    POSTGRES_PASSWORD=web-password
    ```
