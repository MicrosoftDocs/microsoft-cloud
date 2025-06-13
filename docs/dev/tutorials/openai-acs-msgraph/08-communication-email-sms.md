---
title: Sending Email and SMS Messages
description: Learn how to implement email and SMS messaging capabilities in your application using Azure Communication Services to send messages to customers directly from your Line of Business app.
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

# Communication: Sending Email and SMS Messages

In addition to phone calls, Azure Communication Services can also send email and SMS messages. This can be useful when you want to send a message to a customer or other user directly from the application.

In this exercise, you will:
- Explore how email and SMS messages can be sent from the application.
- Walk through the code to learn how the email and SMS functionality is implemented.

### Using the Email and SMS Features

1. In a [previous exercise](/microsoft-cloud/dev/tutorials/openai-acs-msgraph/06-communication-create-acs-resource?#start-app-services) you created an Azure Communication Services (ACS) resource and started the database, web server, and API server. You also updated the following values in the *.env* file.

    ```
    ACS_CONNECTION_STRING=<ACS_CONNECTION_STRING>
    ACS_PHONE_NUMBER=<ACS_PHONE_NUMBER>
    ACS_EMAIL_ADDRESS=<ACS_EMAIL_ADDRESS>
    CUSTOMER_EMAIL_ADDRESS=<EMAIL_ADDRESS_TO_SEND_EMAIL_TO>
    CUSTOMER_PHONE_NUMBER=<UNITED_STATES_BASED_NUMBER_TO_SEND_SMS_TO>
    ```

    Ensure you've completed the [previous exercise](/microsoft-cloud/dev/tutorials/openai-acs-msgraph/06-communication-create-acs-resource?#start-app-services) before continuing.

1. Go back to the browser (*http://localhost:4200*) and select **Contact Customer** followed by **Email/SMS Customer** in the first row.

    :::image type="content" source="./media/acs-email-sms-customer.png" alt-text="Send an email or SMS message using ACS.":::

1. Select the **Email/SMS** tab and perform the following tasks:

    - Enter an Email **Subject** and **Body** and select the **Send Email** button.
    - Enter an SMS message and select the **Send SMS** button.

    :::image type="content" source="./media/acs-email-sms-customer-dialog.png" alt-text="Email/SMS Customer dialog box.":::

    > [!NOTE]
    > SMS verification for toll-free numbers is now mandatory in the United States and Canada. To enable SMS messaging, you must submit verification after the phone number purchase. While this tutorial won't go through that process, you can select **Telephony and SMS** --> **Regulatory Documents** from your Azure Communication Services resource in the Azure portal and add the required validation documentation.

1. Check that you received the email and SMS messages. SMS functionality will only work if you submitted the regulatory documents mentioned in the previous note. As a reminder, the email message will be sent to the value defined for `CUSTOMER_EMAIL_ADDRESS` and the SMS message will be sent to the value defined for `CUSTOMER_PHONE_NUMBER` in the *.env* file. If you weren't able to supply a United States based phone number to use for SMS messages you can skip that step.

    > [!NOTE]
    > If you don't see the email message in your inbox for the address you defined for `CUSTOMER_EMAIL_ADDRESS` in the *.env* file, check your spam folder.

### Exploring the Email Code

[!INCLUDE [Note-Open-Files-VS-Code](./includes/tip-open-files-vs-code.md)]

1. Open the *customers-list.component.ts* file. The full path to the file is *client/src/app/customers-list/customers-list.component.ts*.

1. When you selected **Contact Customer** followed by **Email/SMS Customer** in the datagrid, the `customer-list` component displayed a dialog box. This is handled by the `openEmailSmsDialog()` function in the *customer-list.component.ts* file.

    ```typescript
    openEmailSmsDialog(data: any) {
        if (data.phone && data.email) {
            // Create the data for the dialog
            let dialogData: EmailSmsDialogData = {
                prompt: '',
                title: `Contact ${data.company}`,
                company: data.company,
                customerName: data.first_name + ' ' + data.last_name,
                customerEmailAddress: data.email,
                customerPhoneNumber: data.phone
            }

            // Open the dialog
            const dialogRef = this.dialog.open(EmailSmsDialogComponent, {
                data: dialogData
            });

            // Subscribe to the dialog afterClosed observable to get the dialog result
            this.subscription.add(
                dialogRef.afterClosed().subscribe((response: EmailSmsDialogData) => {
                    console.log('SMS dialog result:', response);
                    if (response) {
                        dialogData = response;
                    }
                })
            );
        }
        else {
            alert('No phone number available.');
        }
    }
    ```

    The `openEmailSmsDialog()` function performs the following tasks:

    - Checks to see if the `data` object (which represents the row from the datagrid) contains a `phone` and `email` property. If it does, it creates a `dialogData` object that contains the information to pass to the dialog.
    - Opens the `EmailSmsDialogComponent` dialog box and passes the `dialogData` object to it.
    - Subscribes to the `afterClosed()` event of the dialog box. This event is fired when the dialog box is closed. The `response` object contains the information that was entered into the dialog box.

1. Open the *email-sms-dialog.component.ts* file. The full path to the file is *client/src/app/email-sms-dialog/email-sms-dialog.component.ts*.

1. Locate the `sendEmail()` function:

    ```typescript
    sendEmail() {
        if (this.featureFlags.acsEmailEnabled) {
            // Using CUSTOMER_EMAIL_ADDRESS instead of this.data.email for testing purposes
            this.subscription.add(
                this.acsService.sendEmail(this.emailSubject, this.emailBody, 
                    this.getFirstName(this.data.customerName), CUSTOMER_EMAIL_ADDRESS /* this.data.email */)
                .subscribe(res => {
                    console.log('Email sent:', res);
                    if (res.status) {
                        this.emailSent = true;
                    }
                })
            );
        }
        else {
            this.emailSent = true; // Used when ACS email isn't enabled
        }
    }
    ```

    The `sendEmail()` function performs the following tasks:

    - Checks to see if the `acsEmailEnabled` feature flag is set to `true`. This flag checks to see if the `ACS_EMAIL_ADDRESS` environment variable has an assigned value.
    - If `acsEmailEnabled` is true, the `acsService.sendEmail()` function is called and the email subject, body, customer name, and customer email address are passed. Because the database contains sample data, the `CUSTOMER_EMAIL_ADDRESS` environment variable is used instead of `this.data.email`. In a real-world application the `this.data.email` value would be used.
    - Subscribes to the `sendEmail()` function in the `acsService` service. This function returns an RxJS observable that contains the response from the client-side service.
    - If the email was sent successfully, the `emailSent` property is set to `true`.

1. To provide better code encapsulation and reuse, client-side services such as *acs.service.ts* are used throughout the application. This allows all ACS functionality to be consolidated into a single place.

1. Open *acs.service.ts* and locate the `sendEmail()` function. The full path to the file is *client/src/app/core/acs.service.ts*.

    ```typescript
    sendEmail(subject: string, message: string, customerName: string, customerEmailAddress: string) : Observable<EmailSmsResponse> {
        return this.http.post<EmailSmsResponse>(this.apiUrl + 'sendEmail', { subject, message, customerName, customerEmailAddress })
        .pipe(
            catchError(this.handleError)
        );
    }
    ```

    The `sendEmail()` function in `AcsService` performs the following tasks:

    - Calls the `http.post()` function and passes the email subject, message, customer name, and customer email address to it. The `http.post()` function returns an RxJS observable that contains the response from the API call.
    - Handles any errors returned by the `http.post()` function using the RxJS `catchError` operator.

1. Now let's examine how the application interacts with the ACS email feature. Open the *acs.ts* file and locate the `sendEmail()` function. The full path to the file is *server/typescript/acs.ts*.

1. The `sendEmail()` function performs the following tasks:

    - Creates a new `EmailClient` object and passes the ACS connection string to it (this value is retrieved from the `ACS_CONNECTION_STRING` environment variable).

        ```typescript
        const emailClient = new EmailClient(connectionString);
        ```

    - Creates a new `EmailMessage` object and passes the sender, subject, message, and recipient information.

        ```typescript
        const msgObject: EmailMessage = {
            senderAddress: process.env.ACS_EMAIL_ADDRESS as string,
            content: {
                subject: subject,
                plainText: message,
            },
            recipients: {
                to: [
                    {
                        address: customerEmailAddress,
                        displayName: customerName,
                    },
                ],
            },
        };
        ```

    - Sends the email using the `emailClient.beginSend()` function and returns the response. Although the function is only sending to one recipient in this example, the `beginSend()` function can be used to send to multiple recipients as well.

        ```typescript
        const poller = await emailClient.beginSend(msgObject);
        ```

    - Waits for the `poller` object to signal it's done and sends the response to the caller.


### Exploring the SMS Code

1. Go back to the *email-sms-dialog.component.ts* file that you opened earlier. The full path to the file is *client/src/app/email-sms-dialog/email-sms-dialog.component.ts*.

1. Locate the `sendSms()` function:

    ```typescript
    sendSms() {
        if (this.featureFlags.acsPhoneEnabled) {
            // Using CUSTOMER_PHONE_NUMBER instead of this.data.customerPhoneNumber for testing purposes
            this.subscription.add(
                this.acsService.sendSms(this.smsMessage, CUSTOMER_PHONE_NUMBER /* this.data.customerPhoneNumber */)
                  .subscribe(res => {
                    if (res.status) {
                        this.smsSent = true;
                    }
                })
            );
        }
        else {
            this.smsSent = true;
        }
    }
    ```

    The `sendSMS()` function performs the following tasks:

    - Checks to see if the `acsPhoneEnabled` feature flag is set to `true`. This flag checks to see if the `ACS_PHONE_NUMBER` environment variable has an assigned value.
    - If `acsPhoneEnabled` is true, the `acsService.SendSms()` function is called and the SMS message and customer phone number are passed. Because the database contains sample data, the `CUSTOMER_PHONE_NUMBER` environment variable is used instead of `this.data.customerPhoneNumber`. In a real-world application the `this.data.customerPhoneNumber` value would be used.
    - Subscribes to the `sendSms()` function in the `acsService` service. This function returns an RxJS observable that contains the response from the client-side service.
    - If the SMS message was sent successfully, it sets the `smsSent` property to `true`.

1. Open *acs.service.ts* and locate the `sendSms()` function. The full path to the file is *client/src/app/core/acs.service.ts*.

    ```typescript
    sendSms(message: string, customerPhoneNumber: string) : Observable<EmailSmsResponse> {
        return this.http.post<EmailSmsResponse>(this.apiUrl + 'sendSms', { message, customerPhoneNumber })
        .pipe(
            catchError(this.handleError)
        );
    }  
    ```

    The `sendSms()` function performs the following tasks:

    - Calls the `http.post()` function and passes the message and customer phone number to it. The `http.post()` function returns an RxJS observable that contains the response from the API call.
    - Handles any errors returned by the `http.post()` function using the RxJS `catchError` operator.

1. Finally, let's examine how the application interacts with the ACS SMS feature. Open the *acs.ts* file. The full path to the file is *server/typescript/acs.ts* and locate the `sendSms()` function.

1. The `sendSms()` function performs the following tasks:

    - Creates a new `SmsClient` object and passes the ACS connection string to it (this value is retrieved from the `ACS_CONNECTION_STRING` environment variable).

        ```typescript
        const smsClient = new SmsClient(connectionString);
        ```

    - Calls the `smsClient.send()` function and passes the ACS phone number (`from`), customer phone number (`to`), and SMS message:

        ```typescript
        const sendResults = await smsClient.send({
            from: process.env.ACS_PHONE_NUMBER as string,
            to: [customerPhoneNumber],
            message: message
        });
        return sendResults;
        ```

    - Returns the response to the caller.

1. You can learn more about ACS email and SMS functionality in the following articles:

    - [Email in Azure Communication Services](/azure/communication-services/concepts/email/email-overview?WT.mc_id=m365-94501-dwahlin)
    - [SMS in Azure Communication Services](/azure/communication-services/concepts/sms/concepts?WT.mc_id=m365-94501-dwahlin)

1. Before moving on to the next exercise, let's review the key concepts covered in this exercise:

    - The *acs.service.ts* file encapsulates the ACS email and SMS functionality used by the client-side application. It handles the API calls to the server and returns the response to the caller. 
    - The server-side API uses the ACS `EmailClient` and `SmsClient` objects to send email and SMS messages.

1. Now that you've learned how email and SMS messages can be sent, let's switch our focus to integrating organizational data into the application.

## Next Step

> [!div class="nextstepaction"]
> [Organizational Data: Creating a Microsoft Entra ID App Registration](./09-orgdata-create-entraid-app.md)



