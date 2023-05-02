<!-- markdownlint-disable MD041 -->

In addition to phone calls, Azure Communication Services can also be used to send email and SMS messages. This can be useful when you want to send a message to a customer or other user directly from the application.

In this exercise, you will:
- Explore how email and SMS messages can be sent from the application.
- Walk through the code to learn how the email and SMS functionality is implemented.

### Using the Email and SMS Features

1. In a [previous exercise](/microsoft-cloud/dev/tutorials/openai-msgraph-acs/?tutorial-step=5) you created an Azure Communication Services (ACS) resource and started the database, web server, and API server. You also updated the following values in the *.env* file.

    ```
    ACS_CONNECTION_STRING=<ACS_CONNECTION_STRING>
    ACS_PHONE_NUMBER=<ACS_PHONE_NUMBER>
    ACS_EMAIL_ADDRESS=<ACS_EMAIL_ADDRESS>
    CUSTOMER_EMAIL_ADDRESS=<EMAIL_ADDRESS_TO_SEND_EMAIL_TO>
    CUSTOMER_PHONE_NUMBER=<UNITED_STATES_BASED_NUMBER_TO_SEND_SMS_TO>
    ```

    Ensure that you've completed the [previous exercise](/microsoft-cloud/dev/tutorials/openai-msgraph-acs/?tutorial-step=5) before continuing.

1. Go back to the browser (*http://localhost:4200*), locate the datagrid, and select **Contact Customer** followed by **Email/SMS Customer** in the first row.

    :::image type="content" source="../media/acs-email-sms-customer.png" alt-text="Send an email or SMS message using ACS.":::

1. In the Email/SMS tab, perform the following tasks:

    - Enter an Email **Subject** and **Body** and select the **Send Email** button.
    - Enter an SMS message and select the **Send SMS** button.

1. Check the email address and phone number you defined in the *.env* file to view the messages. 

    > [!NOTE]
    > If you don't see the email message in your inbox for the address you defined in the *.env* file, check your spam folder. If you don't have a United States based phone number to use for SMS messages you can skip that step.

### Exploring the Email Code

[!INCLUDE [Note-Open-Files-VS-Code](./tip-open-files-vs-code.md)]

1. Open the *customers-list.component.ts* file. The full path to the file is *openai-msgraph-acs/client/src/app/customers-list/customers-list.component.ts*.

### Exploring the SMS Code

1. Open the *customers-list.component.ts* file. The full path to the file is *openai-msgraph-acs/client/src/app/customers-list/customers-list.component.ts*.

