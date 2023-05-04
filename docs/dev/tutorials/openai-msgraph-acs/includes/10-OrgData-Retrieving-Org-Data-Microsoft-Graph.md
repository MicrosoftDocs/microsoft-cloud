<!-- markdownlint-disable MD041 -->

In today's digital environment, users manage a wide array of organizational data, including emails, chats, files, calendar events, and more, often across multiple platforms. This can lead to frequent context shifts—switching between tasks or applications—which can disrupt focus and reduce productivity.

To enhance efficiency, there is growing interest in integrating organizational data directly into the applications users use daily. By unifying data from various sources into one interface, users can access and manage information more seamlessly, minimizing context shifts and improving productivity. Additionally, this integration provides valuable insights and context, enabling users to make informed decisions and work more effectively.

In this exercise, you will:

- Learn how to Microsoft Graph can be used to retrieve organizational data.
- Walk through code examples of retrieving organizational data using Microsoft Graph.

### Using the Organizational Data Feature

1. In a [previous exercise](/microsoft-cloud/dev/tutorials/openai-msgraph-acs/?tutorial-step=7) you created an app registration in Azure AD and started the application server and API server. You also updated the following values in the `.env` file.

    ```
    AAD_CLIENT_ID=<APPLICATION_CLIENT_ID_VALUE>
    TEAM_ID=<TEAMS_TEAM_ID>
    CHANNEL_ID=<TEAMS_CHANNEL_ID>
    ```

1. Go back to the browser (*http://localhost:4200*). If you haven't already signed in, select **Sign In** in the header, and sign in using a user that is a member of your Microsoft 365 Developer tenant. 

    > [!NOTE]
    > You can view the users in your Microsoft 365 tenant by going to the [Microsoft 365 admin center](https://admin.microsoft.com/Adminportal/Home#/users).

1. 
