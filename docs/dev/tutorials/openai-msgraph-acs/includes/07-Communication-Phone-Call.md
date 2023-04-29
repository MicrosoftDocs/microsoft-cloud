<!-- markdownlint-disable MD041 -->

Integrating Azure Communication Services' phone calling capabilities into a custom Line of Business (LOB) application offers several key benefits to businesses and their users:

- Enables seamless and real-time communication between employees, customers, and partners, directly from within the LOB application, eliminating the need to switch between multiple platforms or devices. 
- Enhances the user experience and improves overall operational efficiency. 
- Facilitates rapid problem resolution, as users can quickly connect with relevant support teams or subject matter experts quickly and easily.

In this exercise, you will:
- Learn how phone calling can be integrated into a custom LOB application.

### Exploring the Phone Calling Feature

1. In the [previous exercise](/microsoft-cloud/dev/tutorials/openai-msgraph-acs/?tutorial-step=5) you created an Azure Communication Services (ACS) resource and started the database, web server, and API server. This included adding an ACS phone number into the `.env` file.

1. Go back to the browser (*http://localhost:4200*), locate the datagrid, and select **Contact Customer** followed by **Call Customer** in one of the rows.

1. A phone calling feature will be added into the header. Enter 



### Exploring the Phone Calling Code

1. Open the `openai-msgraph-acs/client/src/app/app.component.ts` file in your editor and locate the `callCustomer()` method.