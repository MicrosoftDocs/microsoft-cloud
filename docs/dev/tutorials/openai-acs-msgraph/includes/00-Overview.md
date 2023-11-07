<!-- markdownlint-disable MD041 -->

**Level**: Intermediate

This tutorial demonstrates how Azure OpenAI, Azure Communication Services, and Microsoft Graph/Microsoft Graph Toolkit can be integrated into Line of Business (LOB) applications to enhance user productivity, elevate the user experience, and take LOB apps to the next level. 

- **AI**: Enable users to ask questions in natural language and convert their answers to SQL that can be used to query a database, allow users to define rules that can be used to automatically generate email and SMS messages, and learn how natural language can be used to retrieve data from your own custom data sources. Azure OpenAI is used for these features.
- **Communication**: Enable in-app phone calling to customers and Email/SMS functionality using Azure Communication Services.
- **Organizational Data**: Pull in related organizational data that users may need (documents, chats, emails, calendar events) as they work with customers to avoid context switching. Providing access to this type of organizational data reduces the need for the user to switch to Outlook, Teams, OneDrive, other custom apps, their phone, etc. since the specific data and functionality they need is provided directly in the app. Microsoft Graph and Microsoft Graph Toolkit are used for this feature.

The application is a simple customer management app that allows users to manage their customers and related data. It consists of a front-end built using TypeScript that calls back-end APIs to retrieve data, interact with AI functionality, send email/SMS messages, and pull in organizational data. Here's an overview of the application solution that you'll walk through in this tutorial:

:::image type="content" source="../media/scenario-overview-no-title.png" alt-text="Microsoft Cloud scenario overview" border="false":::

The tutorial will walk you through the process of setting up the required Azure and Microsoft 365 resources. It'll also walk you through the code that is used to implement the AI, communication, and organizational data features. While you won't be required to copy and paste code, some of the exercises will have you modify code to try out different scenarios.

### Choose Your Own Adventure

You can complete the entire tutorial from start to finish or complete specific topics of interest you. The tutorial is broken down into the following topic areas:

- [Clone the Project Exercise](/microsoft-cloud/dev/tutorials/openai-acs-msgraph/?tutorial-step=1) (required exercise).
- **AI Exercises**: Create an [Azure OpenAI resource](/microsoft-cloud/dev/tutorials/openai-acs-msgraph/?tutorial-step=2) and use it to convert natural language to SQL, generate email/SMS messages, and work with your own data and documents.
- **Communication Exercises**: Create an [Azure Communication Services resource](/microsoft-cloud/dev/tutorials/openai-acs-msgraph/?tutorial-step=6) and use it to make phone calls from the app and send email/SMS messages.
- **Organizational Data Exercises**: [Create a Microsoft Entra ID app registration](/microsoft-cloud/dev/tutorials/openai-acs-msgraph/?tutorial-step=9) so that Microsoft Graph and Microsoft Graph Toolkit can be used to authenticate and pull organizational data into the application.

:::image type="content" source="../media/choose-your-own-adventure.png" alt-text="Choose your own adventure. Complete the entire tutorial or select specific topic areas." border="false":::

<a id="prerequisites"></a>
### Prerequisites

- [Node](https://nodejs.org) - Node 16+ and npm 7+ will be used for this project
- [git](/devops/develop/git/install-and-set-up-git?WT.mc_id=m365-94501-dwahlin)
- [Visual Studio Code](https://code.visualstudio.com?WT.mc_id=m365-94501-dwahlin) (although Visual Studio Code is recommended, any editor can be used)
- [Azure subscription](https://azure.microsoft.com/free/search?WT.mc_id=m365-94501-dwahlin)
- [Microsoft 365 developer tenant](https://developer.microsoft.com/microsoft-365/dev-program?WT.mc_id=m365-94501-dwahlin)
- [Docker Desktop](https://www.docker.com/get-started/) or another OCI (Open Container Initiative) compliant container runtime such as [Podman](https://podman-desktop.io/downloads), or [nerdctl](https://github.com/containerd/nerdctl) capable of running a container.

### Microsoft Cloud Technologies used in this Tutorial

- Microsoft Entra ID
- Azure Communication Services
- Azure OpenAI Service
- Microsoft Graph
- Microsoft Graph Toolkit
