<!-- markdownlint-disable MD041 -->

In this exercise, you'll automate the process of creating a Microsoft Teams meeting link and passing to the ACS by using Azure Functions and Microsoft Graph.

:::image type="content" source="../media/3-create-teams-meeting-link.png" alt-text="Create Teams Meeting":::

1. You'll need to create an Azure Active Directory (AAD) app for Daemon app authentication. In this step, authentication will be handled in the background with `app credentials`, and AAD app will use Application Permissions to make Microsoft Graph API calls. Microsoft Graph will be used to dynamically create a Microsoft Teams meeting and return the Teams meeting URL.

1. Perform the following steps to create an AAD app:
    1. Go to [Azure Portal](https://portal.azure.com) and select `Azure Active Directory`.
    1. Select the `App registration` tab followed by `+ New registration`.
    1. Fill in the new app registration form details as shown below and select `Register`:
        - Name: *ACS Teams Interop App*
        - Supported account types: *Accounts in any organizational directory (Any Azure AD directory - Multitenant) and personal Microsoft accounts (e.g. Skype, Xbox)*
        - Redirect URI: leave this blank
    1. After the app is registered, go to `API permissions` and select `+ Add a permission`.
    1. Select `Microsoft Graph` followed by `Application permissions`.
    1. Select the **Calendars.ReadWrite** permission and then select `Add`.
    1. After adding the permissions, select `Grant admin consent for <your organization name>`.
    1. Go to the `Certificates & secrets` tab, select `+ New client secret`, and then select `Add`. 
    1. Copy the value of the secret into a local file. You'll use the value later in this exercise.
    1. Go to the `Overview` tab and copy the `Application (client) ID` and `Directory (tenant) ID` values into the same local file that you used in the previous step.

<!-- ::: zone pivot="programming-language-csharp"
[!INCLUDE [C#](./04-Create-Teams-Meeting-CS.md)]
::: zone-end -->

<!-- ::: zone pivot="programming-language-typescript"
[!INCLUDE [TypeScript](./04-Create-Teams-Meeting-TS.md)]
::: zone-end -->

[!INCLUDE [TypeScript](./04-Create-Teams-Meeting-TS.md)]



