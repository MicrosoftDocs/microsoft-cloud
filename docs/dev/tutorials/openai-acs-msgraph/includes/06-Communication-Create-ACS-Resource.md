<!-- markdownlint-disable MD041 -->

Effective communication is essential for successful custom business applications. By using Azure Communication Services (ACS), you can add features such as phone calls, live chat, audio/video calls, and email and SMS messaging to your applications. Earlier, you learned how Azure OpenAI can generate completions for email and SMS messages. Now, you'll learn how to send the messages. Together, ACS and OpenAI can enhance your applications by simplifying communication, improving interactions, and boosting business productivity.

In this exercise, you will:

 - Create an Azure Communication Services (ACS) resource.
 - Add a toll-free phone number with calling and SMS capabilities.
 - Connect an email domain.
 - Update the project's *.env* file with values from your ACS resource.

:::image type="content" source="../media/scenario-overview-no-title.png" alt-text="Microsoft Cloud scenario overview" border="false":::

### Create an Azure Communication Services Resource

1. Visit the [Azure Portal](https://portal.azure.com) in your browser and sign in if you haven't already.

1. Type *communication services* in the **search bar** at the top of the page and select **Communication Services** from the options that appear.

    :::image type="content" source="../media/search-acs-portal.png" alt-text="ACS in the Azure Portal":::

1. Select **Create** in the toolbar.

1. Perform the following tasks:
    - Select your Azure subscription.
    - Select the resource group to use (create a new one if one doesn't exist).
    - Enter an ACS resource name. It must be a unique value.
    - Select a data location.

1. Select **Review + Create** followed by **Create**.

1. You've successfully created a new Azure Communication Services resource! Next, you'll  enable phone calling and SMS capabilities. You'll also connect an email domain to the resource.

### Enable Phone Calling and SMS Capabilities

1. Add a phone number and ensure that the phone number has calling capabilities enabled. You'll use this phone number to call out to a phone from the app. 

    - Select `Phone numbers` from the Resource menu.
    - Select `+ Get` in the toolbar (or select the **Get a number** button) and enter the following information:
        - **Country or region**: `United States`
        - **Use case**: Select `An application will be making calls or sending SMS mesages`
        - **Number type**: Toll-free

        > [!NOTE]
        > A credit card is required on your Azure subscription to create the toll-free number. If you don't have a card on file, feel free to skip adding a phone number and jump to the next section of the exercise that connects an email domain. You can still use the app, but won't be able to call out to a phone number.

        - **Calling**: `Make calls`
        - **SMS**: `Send and receive SMS`

1. Select **Next: Numbers**.

1. Select a **Prefix** (for example `877`) and leave the **Quantity** at 1. Select **Search**.

1. Once a toll-free number is displayed, select **Next: Summary**.

1. Review the details and select **Place order** to add the phone number to your ACS resource.

1. Once the phone number is created, select it to get to the **Features** panel.  Ensure that the following values are set:

    - In the **Calling** section, select `Make calls`.
    - In the **SMS** section, select `Send and receive SMS`.
    - Select **Save**.

1. Copy the phone number value into a file for later use.

### Connect an Email Domain

1. Perform the following tasks to create a connected email domain for your ACS resource so that you can send email. This will be used to send email from the app.

    - Select **Domains** from the Resource menu.
    - Select **Connect domain** from the toolbar.
    - Select your **Subscription** and **Resource group**. 
    - Under the **Email Service** dropdown, select `Add an email service`.
    - Give the email service a name such as `acs-demo-email-service`.
    - Select `Review + create` followed by `Create`.
    - Once the deployment completes, select `Go to resource`, and select `1-click add` to add a free Azure subdomain.
    - After the subdomain is added (it'll take a few moments to be deployed), select it.
    - Once you're on the **AzureManagedDomain** screen, select **MailFrom addresses** from the Resource menu. 
    - Copy the **MailFrom** value to a file. You'll use it later as you update the *.env* file.
    - Go back to your Azure Communication Services resource and select `Domains` from the Resource menu.
    - Select `Add domain` and enter the `MailFrom` value from the previous step (ensure you select the correct subscription, resource group, and email service). Select the `Connect` button.

### Update the `.env` File

1. Now that your ACS phone number (with calling and SMS enabled) and email domain are ready, update the following keys/values in the *.env* file in your project:

    ```
    ACS_CONNECTION_STRING=<ACS_CONNECTION_STRING>
    ACS_PHONE_NUMBER=<ACS_PHONE_NUMBER>
    ACS_EMAIL_ADDRESS=<ACS_EMAIL_ADDRESS>
    CUSTOMER_EMAIL_ADDRESS=<EMAIL_ADDRESS_TO_SEND_EMAIL_TO>
    CUSTOMER_PHONE_NUMBER=<UNITED_STATES_BASED_NUMBER_TO_SEND_SMS_TO>
    ```

    - `ACS_CONNECTION_STRING`: The `connection string` value from the **Keys** section of your ACS resource.

    - `ACS_PHONE_NUMBER`: Assign your toll-free number to the `ACS_PHONE_NUMBER` value.

    - `ACS_EMAIL_ADDRESS`: Assign your email "MailTo" address value.

    - `CUSTOMER_EMAIL_ADDRESS`: Assign an email address you'd like email to be sent to from the app (since the customer data in the app's database is only sample data). You can use a personal email address.

    - `CUSTOMER_PHONE_NUMBER`: You'll need to provide a United States based phone number (as of today) due to additional verification that is required in other countries for sending SMS messages. If you don't have a US-based number, you can leave it empty. 

[!INCLUDE [Start-Restart-Services](./Start-Restart-Services.md)]
    
