--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

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
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#sendEmail-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#sendEmail-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#sendTemplatedEmail-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#sendTemplatedEmail-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#sendBulkTemplatedEmail-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#sendBulkTemplatedEmail-property)

## Prerequisite tasks<a name="ses-examples-sending-emails-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/ses/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Email message sending requirements<a name="ses-examples-sending-msail-reqs"></a>

Amazon SES composes an email message and immediately queues it for sending\. To send email using the `SendEmailCommand` method, your message must meet the following requirements:
+ You must send the message from a verified email address or domain\. If you attempt to send email using a non\-verified address or domain, the operation results in an `"Email address not verified"` error\.
+ If your account is still in the Amazon SES sandbox, you can only send to verified addresses or domains, or to email addresses associated with the Amazon SES Mailbox Simulator\. For more information, see [Verifying email addresses and domains](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-addresses-and-domains.html) in the Amazon Simple Email Service Developer Guide\.
+ The total size of the message, including attachments, must be smaller than 10 MB\.
+ The message must include at least one recipient email address\. The recipient address can be a To: address, a CC: address, or a BCC: address\. If a recipient email address is not valid \(that is, it is not in the format `UserName@[SubDomain.]Domain.TopLevelDomain`\), the entire message is rejected, even if the message contains other recipients that are valid\.
+ The message cannot include more than 50 recipients across the To:, CC: and BCC: fields\. If you need to send an email message to a larger audience, you can divide your recipient list into groups of 50 or fewer, and then call the `sendEmail` method several times to send the message to each group\.

## Sending an email<a name="ses-examples-sendmail"></a>

In this example, use a Node\.js module to send email with Amazon SES\. Create a Node\.js module with the file name `ses_sendemail.ts`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the parameter values that define the email to be sent, including sender and receiver addresses, subject, and email body in plain text and HTML formats, to the `SendEmailCommand` method of the `SES` client class\. To call the `SendEmailCommand` method, invoke an Amazon SES service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *RECEIVER\_ADDRESS* with the address to send the email to, and *SENDER\_ADDRESS* with the email address to the send the email from\.

```
*/
// Create the promise and SES service object

// Import required AWS SDK clients and commands for Node.js
const { SESClient, SendEmailCommand } = require("@aws-sdk/client-ses");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  Destination: {
    /* required */
    CcAddresses: [
      /* more items */
    ],
    ToAddresses: [
      "RECEIVER_ADDRESS", //RECEIVER_ADDRESS
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
  Source: "SENDER_ADDRESS", // SENDER_ADDRESS
  ReplyToAddresses: [
    /* more items */
  ],
};

// Create SES service object
const ses = new SESClient(REGION);

const run = async () => {
  try {
    const data = await ses.send(new SendEmailCommand(params));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\. The email is queued for sending by Amazon SES\.

```
ts-node ses_sendemail.ts // If you prefer JavaScript, enter 'node ses_sendemail.js'
```

This example code can be found [found here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_sendemail.ts)\. 

## Sending an email using a template<a name="ses-examples-sendtemplatedemail"></a>

In this example, use a Node\.js module to send email with Amazon SES\. Create a Node\.js module with the file name `ses_sendtemplatedemail.ts`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the parameter values that define the email to be sent, including sender and receiver addresses, subject, email body in plain text and HTML formats, to the `SendTemplatedEmailCommand` method of the `SES` client class\. To call the `SendTemplatedEmailCommand` method, invoke an Amazon SES client service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *RECEIVER\_ADDRESS* with the address to send the email to, *SENDER\_ADDRESS* with the email address to the send the email from, and *TEMPLATE\_NAME* with the name of the template\.

```
// Import required AWS SDK clients and commands for Node.js
const { SESClient, SendTemplatedEmailCommand } = require("@aws-sdk/client-ses");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  Destination: {
    /* required */
    CcAddresses: [
      /* more CC email addresses */
    ],
    ToAddresses: [
      "RECEIVER_ADDRESS", // RECEIVER_ADDRESS
      /* more To-email addresses */
    ],
  },
  Source: "SENDER_ADDRESS", //SENDER_ADDRESS
  Template: "TEMPLATE_NAME", // TEMPLATE_NAME
  TemplateData: '{ "REPLACEMENT_TAG_NAME":"REPLACEMENT_VALUE" }' /* required */,
  ReplyToAddresses: [],
};

// Create SES service object
const ses = new SESClient(REGION);

const run = async () => {
  try {
    const data = await ses.send(new SendTemplatedEmailCommand(params));
    console.log("Success, templated email sent; messageId:", data.MessageId);
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\. The email is queued for sending by Amazon SES\.

```
ts-node ses_sendtemplatedemail.ts // If you prefer JavaScript, enter 'node ses_sendtemplatedemail.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_sendtemplatedemail.ts)\.

## Sending bulk email using a template<a name="ses-examples-sendbulktemplatedemail"></a>

In this example, use a Node\.js module to send email with Amazon SES\. Create a Node\.js module with the file name `ses_sendbulktemplatedemail.ts`\. Configure the SDK as previously shown, including installing the required clients and packages\. 

Create an object to pass the parameter values that define the email to be sent, including sender and receiver addresses, subject, and email body in plain text and HTML formats, to the `SendBulkTemplatedEmailCommand` method of the `SES` client class\. To call the `SendBulkTemplatedEmailCommand` method, invoke an Amazon SES service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *RECEIVER\_ADDRESSES* with the address to send the email to, and *SENDER\_ADDRESS* with the email address to the send the email from\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  SESClient,
  SendBulkTemplatedEmailCommand
} = require("@aws-sdk/client-ses");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
var params = {
  Destinations: [
    /* required */
    {
      Destination: {
        /* required */
        CcAddresses: [
          "RECEIVER_ADDRESSES", //RECEIVER_ADDRESS
          /* more items */
        ],
        ToAddresses: [
          /* more items */
        ],
      },
      ReplacementTemplateData: '{ "REPLACEMENT_TAG_NAME":"REPLACEMENT_VALUE" }',
    },
  ],
  Source: "SENDER_ADDRESS", // SENDER_ADDRESS
  Template: "TEMPLATE", //TEMPLATE
  DefaultTemplateData: '{ "REPLACEMENT_TAG_NAME":"REPLACEMENT_VALUE" }',
  ReplyToAddresses: [],
};

// Create SES service object
const ses = new SESClient(REGION);

const run = async () => {
  try {
    const data = await ses.send(new SendBulkTemplatedEmailCommand(params));
    console.log(data);
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\. The email is queued for sending by Amazon SES\.

```
ts-node ses_sendbulktemplatedemail.ts // If you prefer JavaScript, enter 'node ses_sendbulktemplatedemail.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_sendbulktemplatedemail.ts)\. 