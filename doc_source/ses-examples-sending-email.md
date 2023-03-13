--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Sending email using Amazon SES<a name="ses-examples-sending-email"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ Send a text or HTML email\.
+ Send emails based on an email template\.
+ Send bulk emails based on an email template\.

The Amazon SES API provides two different ways for you to send an email, depending on how much control you want over the composition of the email message: formatted and raw\. For details, see [Sending formatted email using the Amazon SES API](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/send-email-formatted.html) and [Sending raw email using the Amazon SES API](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/send-email-raw.html)\.

## The scenario<a name="ses-examples-sending-email-scenario"></a>

In this example, you use a series of Node\.js modules to send email in a variety of ways\. The Node\.js modules use the SDK for JavaScript to create and use email templates using these methods of the `SES` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/sendemailcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/sendemailcommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/sendtemplatedemailcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/sendtemplatedemailcommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/sendbulktemplatedemailcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/sendbulktemplatedemailcommand.html)

## Prerequisite tasks<a name="ses-examples-sending-emails-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/ses/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Email message sending requirements<a name="ses-examples-sending-msail-reqs"></a>

Amazon SES composes an email message and immediately queues it for sending\. To send email using the `SendEmailCommand` method, your message must meet the following requirements:
+ You must send the message from a verified email address or domain\. If you attempt to send email using a non\-verified address or domain, the operation results in an `"Email address not verified"` error\.
+ If your account is still in the Amazon SES sandbox, you can only send to verified addresses or domains, or to email addresses associated with the Amazon SES Mailbox Simulator\. For more information, see [Verifying email addresses and domains](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-addresses-and-domains.html) in the Amazon Simple Email Service Developer Guide\.
+ The total size of the message, including attachments, must be smaller than 10 MB\.
+ The message must include at least one recipient email address\. The recipient address can be a To: address, a CC: address, or a BCC: address\. If a recipient email address is not valid \(that is, it is not in the format `UserName@[SubDomain.]Domain.TopLevelDomain`\), the entire message is rejected, even if the message contains other recipients that are valid\.
+ The message cannot include more than 50 recipients across the To:, CC: and BCC: fields\. If you need to send an email message to a larger audience, you can divide your recipient list into groups of 50 or fewer, and then call the `sendEmail` method several times to send the message to each group\.

## Sending an email<a name="ses-examples-sendmail"></a>

In this example, use a Node\.js module to send email with Amazon SES\. 

Create a `libs` directory, and create a Node\.js module with the file name `sesClient.js`\. Copy and paste the code below into it, which creates the Amazon SES client object\. Replace *REGION* with your AWS Region\.

```
import { SESClient } from "@aws-sdk/client-ses";
// Set the AWS Region.
const REGION = "us-east-1";
// Create SES service object.
const sesClient = new SESClient({ region: REGION });
export { sesClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/libs/sesClient.js)\.

Create a Node\.js module with the file name `ses_sendemail.js`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the parameter values that define the email to be sent, including sender and receiver addresses, subject, and email body in plain text and HTML formats, to the `SendEmailCommand` method of the `SES` client class\. To call the `SendEmailCommand` method, invoke an Amazon SES service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *RECEIVER\_ADDRESS* with the address to send the email to, and *SENDER\_ADDRESS* with the email address to the send the email from\.

```
import { SendEmailCommand } from "@aws-sdk/client-ses";
import { sesClient } from "./libs/sesClient.js";

const createSendEmailCommand = (toAddress, fromAddress) => {
  return new SendEmailCommand({
    Destination: {
      /* required */
      CcAddresses: [
        /* more items */
      ],
      ToAddresses: [
        toAddress,
        /* more To-email addresses */
      ],
    },
    Message: {
      /* required */
      Body: {
        /* required */
        Html: {
          Charset: "UTF-8",
          Data: "HTML_FORMAT_BODY",
        },
        Text: {
          Charset: "UTF-8",
          Data: "TEXT_FORMAT_BODY",
        },
      },
      Subject: {
        Charset: "UTF-8",
        Data: "EMAIL_SUBJECT",
      },
    },
    Source: fromAddress,
    ReplyToAddresses: [
      /* more items */
    ],
  });
};

const run = async () => {
  const sendEmailCommand = createSendEmailCommand(
    "recipient@example.com",
    "sender@example.com"
  );

  try {
    return await sesClient.send(sendEmailCommand);
  } catch (e) {
    console.error("Failed to send email.");
    return e;
  }
};
```

To run the example, enter the following at the command prompt\. The email is queued for sending by Amazon SES\.

```
node ses_sendemail.js 
```

This example code can be found [found here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_sendemail.js)\. 

## Sending an email using a template<a name="ses-examples-sendtemplatedemail"></a>

In this example, use a Node\.js module to send email with Amazon SES\. Create a Node\.js module with the file name `ses_sendtemplatedemail.js`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the parameter values that define the email to be sent, including sender and receiver addresses, subject, email body in plain text and HTML formats, to the `SendTemplatedEmailCommand` method of the `SES` client class\. To call the `SendTemplatedEmailCommand` method, invoke an Amazon SES client service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *RECEIVER\_ADDRESS* with the address to send the email to, *SENDER\_ADDRESS* with the email address to the send the email from, and *TEMPLATE\_NAME* with the name of the template\.

```
import { SendTemplatedEmailCommand } from "@aws-sdk/client-ses";
import { getUniqueName, postfix } from "../../libs/utils/util-string.js";
import { sesClient } from "./libs/sesClient.js";

/**
 * Replace this with the name of an existing template.
 */
const TEMPLATE_NAME = getUniqueName("ReminderTemplate");

/**
 * Replace these with existing verified emails.
 */
const VERIFIED_EMAIL = postfix(getUniqueName("Bilbo"), "@example.com");

const USER = { firstName: "Bilbo", emailAddress: VERIFIED_EMAIL };

/**
 *
 * @param { { emailAddress: string, firstName: string } } user
 * @param { string } templateName - The name of an existing template in Amazon SES.
 * @returns { SendTemplatedEmailCommand }
 */
const createReminderEmailCommand = (user, templateName) => {
  return new SendTemplatedEmailCommand({
    /**
     * Here's an example of how a template would be replaced with user data:
     * Template: <h1>Hello {{contact.firstName}},</h1><p>Don't forget about the party gifts!</p>
     * Destination: <h1>Hello Bilbo,</h1><p>Don't forget about the party gifts!</p>
     */
    Destination: { ToAddresses: [user.emailAddress] },
    TemplateData: JSON.stringify({ contact: { firstName: user.firstName } }),
    Source: VERIFIED_EMAIL,
    Template: templateName,
  });
};

const run = async () => {
  const sendReminderEmailCommand = createReminderEmailCommand(
    USER,
    TEMPLATE_NAME
  );
  try {
    return await sesClient.send(sendReminderEmailCommand);
  } catch (err) {
    console.log("Failed to send template email", err);
    return err;
  }
};
```

To run the example, enter the following at the command prompt\. The email is queued for sending by Amazon SES\.

```
node ses_sendtemplatedemail.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_sendtemplatedemail.js)\.

## Sending bulk email using a template<a name="ses-examples-sendbulktemplatedemail"></a>

In this example, use a Node\.js module to send email with Amazon SES\. 

Create a `libs` directory, and create a Node\.js module with the file name `sesClient.js`\. Copy and paste the code below into it, which creates the Amazon SES client object\. Replace *REGION* with your AWS Region\.

```
import { SESClient } from "@aws-sdk/client-ses";
// Set the AWS Region.
const REGION = "us-east-1";
// Create SES service object.
const sesClient = new SESClient({ region: REGION });
export { sesClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/libs/sesClient.js)\.

Create a Node\.js module with the file name `ses_sendbulktemplatedemail.js`\. Configure the SDK as previously shown, including installing the required clients and packages\. 

Create an object to pass the parameter values that define the email to be sent, including sender and receiver addresses, subject, and email body in plain text and HTML formats, to the `SendBulkTemplatedEmailCommand` method of the `SES` client class\. To call the `SendBulkTemplatedEmailCommand` method, invoke an Amazon SES service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *RECEIVER\_ADDRESSES* with the address to send the email to, and *SENDER\_ADDRESS* with the email address to the send the email from\.

```
import { SendBulkTemplatedEmailCommand } from "@aws-sdk/client-ses";
import { getUniqueName, postfix } from "../../libs/utils/util-string.js";
import { sesClient } from "./libs/sesClient.js";

/**
 * Replace this with the name of an existing template.
 */
const TEMPLATE_NAME = getUniqueName("ReminderTemplate");

/**
 * Replace these with existing verified emails.
 */
const VERIFIED_EMAIL_1 = postfix(getUniqueName("Bilbo"), "@example.com");
const VERIFIED_EMAIL_2 = postfix(getUniqueName("Frodo"), "@example.com");

const USERS = [
  { firstName: "Bilbo", emailAddress: VERIFIED_EMAIL_1 },
  { firstName: "Frodo", emailAddress: VERIFIED_EMAIL_2 },
];

/**
 *
 * @param { { emailAddress: string, firstName: string }[] } users
 * @param { string } templateName the name of an existing template in SES
 * @returns { SendBulkTemplatedEmailCommand }
 */
const createBulkReminderEmailCommand = (users, templateName) => {
  return new SendBulkTemplatedEmailCommand({
    /**
     * Each 'Destination' uses a corresponding set of replacement data. We can map each user
     * to a 'Destination' and provide user specific replacement data to create personalized emails.
     *
     * Here's an example of how a template would be replaced with user data:
     * Template: <h1>Hello {{name}},</h1><p>Don't forget about the party gifts!</p>
     * Destination 1: <h1>Hello Bilbo,</h1><p>Don't forget about the party gifts!</p>
     * Destination 2: <h1>Hello Frodo,</h1><p>Don't forget about the party gifts!</p>
     */
    Destinations: users.map((user) => ({
      Destination: { ToAddresses: [user.emailAddress] },
      ReplacementTemplateData: JSON.stringify({ name: user.firstName }),
    })),
    DefaultTemplateData: JSON.stringify({ name: "Shireling" }),
    Source: VERIFIED_EMAIL_1,
    Template: templateName,
  });
};

const run = async () => {
  const sendBulkTemplateEmailCommand = createBulkReminderEmailCommand(
    USERS,
    TEMPLATE_NAME
  );
  try {
    return await sesClient.send(sendBulkTemplateEmailCommand);
  } catch (err) {
    console.log("Failed to send bulk template email", err);
    return err;
  }
};
```

To run the example, enter the following at the command prompt\. The email is queued for sending by Amazon SES\.

```
node ses_sendbulktemplatedemail.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_sendbulktemplatedemail.js)\. 