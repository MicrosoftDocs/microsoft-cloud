<!-- markdownlint-disable MD041 -->

**Level**: Intermediate

This tutorial demonstrates how Azure OpenAI, Azure Communication Services, and Microsoft Graph/Microsoft Graph Toolkit can be integrated into Line of Business (LOB) applications to enhance user productivity, elevate the user experience, and take LOB apps to the next level.

- **AI**: Enable users to ask questions in natural language and convert their answers to SQL that can be used to query a database. Allow users to define rules that can be used to automatically generate email and SMS messages. Azure OpenAI is used for these features.
- **Communication**: Enable in-app phone calling to customers and Email/SMS functionality using Azure Communication Services.
- **Organizational Data**: Pull in related organizational data that users may need (documents, chats, emails, calendar events) as they work with customers to avoid context switching. Providing access to this type of organizational data reduces the need for the user to switch to Outlook, Teams, OneDrive, other custom apps, their phone, etc. since the specific data and functionality they need is provided directly in the app. Microsoft Graph and Microsoft Graph Toolkit are used for this feature.

Here's an overview of the application solution that you'll be looking at in this tutorial:

:::image type="content" source="../media/scenario-overview-no-title.png" alt-text="Microsoft Cloud scenario overview" border="false":::

### Pre-requisites

- [Node](https://nodejs.org) - Node 16+ and npm 7+ will be used for this project
- [git](https://learn.microsoft.com/devops/develop/git/install-and-set-up-git)
- [Visual Studio Code](https://code.visualstudio.com) (while we'll reference Visual Studio Code in this tutorial, any editor can be used)
- [Azure subscription](https://azure.microsoft.com/free/search)
- [Microsoft 365 developer tenant](https://developer.microsoft.com/microsoft-365/dev-program)
- [Docker Desktop](https://www.docker.com/get-started/) - or an equivalent container runtime environment capable of running `docker-compose` commands

### Microsoft Cloud technologies used in this tutorial include

- Azure Communication Services
- Azure OpenAI Service
- Microsoft Graph
- Microsoft Graph Toolkit