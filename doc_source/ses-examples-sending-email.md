# Sending Email Using Amazon SES<a name="ses-examples-sending-email"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ Send a text or HTML email\.
+ Send emails based on an email template\.
+ Send bulk emails based on an email template\.

The Amazon SES API provides two different ways for you to send an email, depending on how much control you want over the composition of the email message: formatted and raw\. For details, see [Sending Formatted Email Using the Amazon SES API](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/send-email-formatted.html) and [Sending Raw Email Using the Amazon SES API](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/send-email-raw.html)\.

## The Scenario<a name="ses-examples-sending-email-scenario"></a>

In this example, you use a series of Node\.js modules to send email in a variety of ways\. The Node\.js modules use the SDK for JavaScript to create and use email templates using these methods of the `AWS.SES` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#sendEmail-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#sendEmail-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#sendTemplatedEmail-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#sendTemplatedEmail-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#sendBulkTemplatedEmail-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#sendBulkTemplatedEmail-property)

## Prerequisite Tasks<a name="ses-examples-sending-emails-prerequisites"></a>
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Email Message Sending Requirements<a name="ses-examples-sending-msail-reqs"></a>

Amazon SES composes an email message and immediately queues it for sending\. To send email using the `SES.sendEmail` method, your message must meet the following requirements:
+ You must send the message from a verified email address or domain\. If you attempt to send email using a non\-verified address or domain, the operation results in an `"Email address not verified"` error\.
+ If your account is still in the Amazon SES sandbox, you can only send to verified addresses or domains, or to email addresses associated with the Amazon SES Mailbox Simulator\. For more information, see [Verifying Email Addresses and Domains](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-addresses-and-domains.html) in the Amazon Simple Email Service Developer Guide\.
+ The total size of the message, including attachments, must be smaller than 10 MB\.
+ The message must include at least one recipient email address\. The recipient address can be a To: address, a CC: address, or a BCC: address\. If a recipient email address is invalid \(that is, it is not in the format `UserName@[SubDomain.]Domain.TopLevelDomain`\), the entire message is rejected, even if the message contains other recipients that are valid\.
+ The message cannot include more than 50 recipients, across the To:, CC: and BCC: fields\. If you need to send an email message to a larger audience, you can divide your recipient list into groups of 50 or fewer, and then call the `sendEmail` method several times to send the message to each group\.

## Sending an Email<a name="ses-examples-sendmail"></a>

In this example, use a Node\.js module to send email with Amazon SES\. Create a Node\.js module with the file name `ses_sendemail.js`\. Configure the SDK as previously shown\.

Create an object to pass the parameter values that define the email to be sent, including sender and receiver addresses, subject, email body in plain text and HTML formats, to the `sendEmail` method of the `AWS.SES` client class\. To call the `sendEmail` method, create a promise for invoking an Amazon SES service object, passing the parameters\. Then handle the `response` in the promise callback\. 

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create sendEmail params 
var params = {
  Destination: { /* required */
    CcAddresses: [
      'EMAIL_ADDRESS',
      /* more items */
    ],
    ToAddresses: [
      'EMAIL_ADDRESS',
      /* more items */
    ]
  },
  Message: { /* required */
    Body: { /* required */
      Html: {
       Charset: "UTF-8",
       Data: "HTML_FORMAT_BODY"
      },
      Text: {
       Charset: "UTF-8",
       Data: "TEXT_FORMAT_BODY"
      }
     },
     Subject: {
      Charset: 'UTF-8',
      Data: 'Test email'
     }
    },
  Source: 'SENDER_EMAIL_ADDRESS', /* required */
  ReplyToAddresses: [
     'EMAIL_ADDRESS',
    /* more items */
  ],
};

// Create the promise and SES service object
var sendPromise = new AWS.SES({apiVersion: '2010-12-01'}).sendEmail(params).promise();

// Handle promise's fulfilled/rejected states
sendPromise.then(
  function(data) {
    console.log(data.MessageId);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\. The email is queued for sending by Amazon SES\.

```
node ses_sendemail.js
```

This example code can be [found here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ses/ses_sendemail.js)\. 

## Sending an Email Using a Template<a name="ses-examples-sendtemplatedemail"></a>

In this example, use a Node\.js module to send email with Amazon SES\. Create a Node\.js module with the file name `ses_sendtemplatedemail.js`\. Configure the SDK as previously shown\.

Create an object to pass the parameter values that define the email to be sent, including sender and receiver addresses, subject, email body in plain text and HTML formats, to the `sendTemplatedEmail` method of the `AWS.SES` client class\. To call the `sendTemplatedEmail` method, create a promise for invoking an Amazon SES service object, passing the parameters\. Then handle the `response` in the promise callback\. 

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create sendTemplatedEmail params 
var params = {
  Destination: { /* required */
    CcAddresses: [
      'EMAIL_ADDRESS',
      /* more CC email addresses */
    ],
    ToAddresses: [
      'EMAIL_ADDRESS',
      /* more To email addresses */
    ]
  },
  Source: 'EMAIL_ADDRESS', /* required */
  Template: 'TEMPLATE_NAME', /* required */
  TemplateData: '{ \"REPLACEMENT_TAG_NAME\":\"REPLACEMENT_VALUE\" }', /* required */
  ReplyToAddresses: [
    'EMAIL_ADDRESS'
  ],
};


// Create the promise and SES service object
var sendPromise = new AWS.SES({apiVersion: '2010-12-01'}).sendTemplatedEmail(params).promise();

// Handle promise's fulfilled/rejected states
sendPromise.then(
  function(data) {
    console.log(data);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\. The email is queued for sending by Amazon SES\.

```
node ses_sendtemplatedemail.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ses/ses_sendtemplatedemail.js)\.

## Sending Bulk Email Using a Template<a name="ses-examples-sendbulktemplatedemail"></a>

In this example, use a Node\.js module to send email with Amazon SES\. Create a Node\.js module with the file name `ses_sendbulktemplatedemail.js`\. Configure the SDK as previously shown\. 

Create an object to pass the parameter values that define the email to be sent, including sender and receiver addresses, subject, email body in plain text and HTML formats, to the `sendBulkTemplatedEmail` method of the `AWS.SES` client class\. To call the `sendBulkTemplatedEmail` method, create a promise for invoking an Amazon SES service object, passing the parameters\. Then handle the `response` in the promise callback\. 

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create sendBulkTemplatedEmail params 
var params = {
  Destinations: [ /* required */
    {
      Destination: { /* required */
        CcAddresses: [
          'EMAIL_ADDRESS',
          /* more items */
        ],
        ToAddresses: [
          'EMAIL_ADDRESS',
          'EMAIL_ADDRESS'
          /* more items */
        ]
      },
      ReplacementTemplateData: '{ \"REPLACEMENT_TAG_NAME\":\"REPLACEMENT_VALUE\" }'
  },
  ],
  Source: 'EMAIL_ADDRESS', /* required */
  Template: 'TEMPLATE_NAME', /* required */
  DefaultTemplateData: '{ \"REPLACEMENT_TAG_NAME\":\"REPLACEMENT_VALUE\" }',
  ReplyToAddresses: [
    'EMAIL_ADDRESS'
  ]
};


// Create the promise and SES service object
var sendPromise = new AWS.SES({apiVersion: '2010-12-01'}).sendBulkTemplatedEmail(params).promise();

// Handle promise's fulfilled/rejected states
sendPromise.then(
  function(data) {
    console.log(data);
  }).catch(
    function(err) {
    console.log(err, err.stack);
  });
```

To run the example, type the following at the command line\. The email is queued for sending by Amazon SES\.

```
node ses_sendbulktemplatedemail.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ses/ses_sendbulktemplatedemail.js)\. 