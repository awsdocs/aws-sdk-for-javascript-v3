--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Sending SMS Messages with Amazon SNS<a name="sns-examples-sending-sms"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to get and set SMS messaging preferences for Amazon SNS\.
+ How to check a phone number to see if it has opted out of receiving SMS messages\.
+ How to get a list of phone numbers that have opted out of receiving SMS messages\.
+ How to send an SMS message\.

## The Scenario<a name="sns-examples-sending-sms-scenario"></a>

You can use Amazon SNS to send text messages, or SMS messages, to SMS\-enabled devices\. You can send a message directly to a phone number, or you can send a message to multiple phone numbers at once by subscribing those phone numbers to a topic and sending your message to the topic\.

In this example, you use a series of Node\.js modules to publish SMS text messages from Amazon SNS to SMS\-enabled devices\. The Node\.js modules use the SDK for JavaScript to publish SMS messages using these methods of the `SNS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/publishcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/publishcommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/getsmsattributescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/getsmsattributescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/setsmsattributescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/setsmsattributescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/checkifphonenumberisoptedoutcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/checkifphonenumberisoptedoutcommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/listphonenumbersoptedoutcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/listphonenumbersoptedoutcommand.html)

## Prerequisite Tasks<a name="sns-examples-sending-sms-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/sns/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Publishing an SMS Message<a name="sending-sms-publishsms"></a>

In this example, use a Node\.js module to send an SMS message to a phone number\.

Create a `libs` directory, and create a Node\.js module with the file name `snsClient.js`\. Copy and paste the code below into it, which creates the Amazon SNS client object\. Replace *REGION* with your AWS Region\.

```
import  { SNSClient } from "@aws-sdk/client-sns";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const snsClient = new SNSClient({ region: REGION });
export  { snsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/libs/snsClient.js)\.

Create a Node\.js module with the file name `sns_publishsms.js`\. Configure the SDK as previously shown, including installing the required clients and packages\. Create an object containing the `Message` and `PhoneNumber` parameters\.

When you send an SMS message, specify the phone number using the E\.164 format\. E\.164 is a standard for the phone number structure used for international telecommunication\. Phone numbers that follow this format can have a maximum of 15 digits, and they are prefixed with the plus character \(\+\) and the country code\. For example, a US phone number in E\.164 format would appear as \+1001XXX5550100\. 

This example sets the `PhoneNumber` parameter to specify the phone number to send the message\. Pass the object to the `PublishCommand` method of the `SNS` client class\. To call the `PublishCommand` method, create an asynchronous function invoking an Amazon SNS service object, passing the parameters object\. 

**Note**  
Replace *TEXT\_MESSAGE* with the text message, and *PHONE\_NUMBER* with the phone number\.

```
// Import required AWS SDK clients and commands for Node.js
import {PublishCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = {
  Message: "MESSAGE_TEXT" /* required */,
  PhoneNumber: "PHONE_NUMBER", //PHONE_NUMBER, in the E.164 phone number structure
};

const run = async () => {
  try {
    const data = await snsClient.send(new PublishCommand(params));
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node sns_publishsms.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/sns_publishsms.js)\.

## Getting SMS Attributes<a name="sending-sms-getattributes"></a>

Use Amazon SNS to specify preferences for SMS messaging, such as how your deliveries are optimized \(for cost or for reliable delivery\), your monthly spending limit, how message deliveries are logged, and whether to subscribe to daily SMS usage reports\. These preferences are retrieved and set as SMS attributes for Amazon SNS\.

In this example, use a Node\.js module to get the current SMS attributes in Amazon SNS\.

Create a `libs` directory, and create a Node\.js module with the file name `snsClient.js`\. Copy and paste the code below into it, which creates the Amazon SNS client object\. Replace *REGION* with your AWS Region\.

```
import  { SNSClient } from "@aws-sdk/client-sns";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const snsClient = new SNSClient({ region: REGION });
export  { snsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/libs/snsClient.js)\.

 Create a Node\.js module with the file name `sns_getsmstype.js`\.

Configure the SDK as previously shown, including downloading the required clients and packages\. Create an object containing the parameters for getting SMS attributes, including the names of the individual attributes to get\. For details on available SMS attributes, see [SetSMSAttributes](https://docs.aws.amazon.com/sns/latest/api/API_SetSMSAttributes.html) in the Amazon Simple Notification Service API Reference\.

This example gets the `DefaultSMSType` attribute, which controls whether SMS messages are sent as `Promotional`, which optimizes message delivery to incur the lowest cost, or as `Transactional`, which optimizes message delivery to achieve the highest reliability\. Pass the parameters to the `SetTopicAttributesCommand` method of the `SNS` client class\. To call the `SetSMSAttributesCommand` method, create an asynchronous function invoking an Amazon SNS client service object, passing the parameters object\. 

**Note**  
Replace *ATTRIBUTE\_NAME* with the name of the attribute\.

```
// Import required AWS SDK clients and commands for Node.js
import {GetSMSAttributesCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = {
  attributes: [
    "DefaultSMSType",
    "ATTRIBUTE_NAME",
    /* more items */
  ],
};

const run = async () => {
  try {
    const data = await snsClient.send(new GetSMSAttributesCommand(params));
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node sns_getsmstype.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/sns_getsmstype.js)\.

## Setting SMS Attributes<a name="sending-sms-setattributes"></a>

In this example, use a Node\.js module to get the current SMS attributes in Amazon SNS\.

Create a `libs` directory, and create a Node\.js module with the file name `snsClient.js`\. Copy and paste the code below into it, which creates the Amazon SNS client object\. Replace *REGION* with your AWS Region\.

```
import  { SNSClient } from "@aws-sdk/client-sns";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const snsClient = new SNSClient({ region: REGION });
export  { snsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/libs/snsClient.js)\.

 Create a Node\.js module with the file name `sns_setsmstype.js`\. Configure the SDK as previously shown, including installing the required clients and packages\. Create an object containing the parameters for setting SMS attributes, including the names of the individual attributes to set and the values to set for each\. For details on available SMS attributes, see [ SetSMSAttributes](https://docs.aws.amazon.com/sns/latest/api/API_SetSMSAttributes.html) in the Amazon Simple Notification Service API Reference\.

This example sets the `DefaultSMSType` attribute to `Transactional`, which optimizes message delivery to achieve the highest reliability\. Pass the parameters to the `SetTopicAttributesCommand` method of the `SNS` client class\. To call the `SetSMSAttributesCommand` method, create an asynchronous function invoking an Amazon SNS client service object, passing the parameters object\. 

```
// Import required AWS SDK clients and commands for Node.js
import {SetSMSAttributesCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = {
  attributes: {
    /* required */
    DefaultSMSType: "Transactional" /* highest reliability */,
    //'DefaultSMSType': 'Promotional' /* lowest cost */
  },
};

const run = async () => {
  try {
    const data = await snsClient.send(new SetSMSAttributesCommand(params));
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node sns_setsmstype.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/sns_setsmstype.js)\.

## Checking If a Phone Number Has Opted Out<a name="sending-sms-checkifphonenumberisoptedout"></a>

In this example, use a Node\.js module to check a phone number to see if it has opted out from receiving SMS messages\. 

Create a `libs` directory, and create a Node\.js module with the file name `snsClient.js`\. Copy and paste the code below into it, which creates the Amazon SNS client object\. Replace *REGION* with your AWS Region\.

```
import  { SNSClient } from "@aws-sdk/client-sns";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const snsClient = new SNSClient({ region: REGION });
export  { snsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/libs/snsClient.js)\.

Create a Node\.js module with the file name `sns_checkphoneoptout.js`\. Configure the SDK as previously shown\. Create an object containing the phone number to check as a parameter\.

This example sets the `PhoneNumber` parameter to specify the phone number to check\. Pass the object to the `CheckIfPhoneNumberIsOptedOutCommand` method of the `SNS` client class\. To call the `CheckIfPhoneNumberIsOptedOutCommand` method, create an asynchronous function invoking an Amazon SNS client service object, passing the parameters object\. 

**Note**  

Replace *PHONE\_NUMBER* with the phone number\.

```
// Import required AWS SDK clients and commands for Node.js
import {CheckIfPhoneNumberIsOptedOutCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = { phoneNumber: "353861230764" }; //PHONE_NUMBER, in the E.164 phone number structure

const run = async () => {
  try {
    const data = await snsClient.send(
      new CheckIfPhoneNumberIsOptedOutCommand(params)
    );
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node sns_checkphoneoptout.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/sns_checkphoneoptout.js)\.

## Listing Opted\-Out Phone Numbers<a name="sending-sms-listphonenumbersoptedout"></a>

In this example, use a Node\.js module to get a list of phone numbers that have opted out from receiving SMS messages\.

Create a `libs` directory, and create a Node\.js module with the file name `snsClient.js`\. Copy and paste the code below into it, which creates the Amazon SNS client object\. Replace *REGION* with your AWS Region\.

```
import  { SNSClient } from "@aws-sdk/client-sns";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const snsClient = new SNSClient({ region: REGION });
export  { snsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/libs/snsClient.js)\.

Create a Node\.js module with the file name `sns_listnumbersoptedout.js`\. Configure the SDK as previously shown\. Create an empty object as a parameter\.

Pass the object to the `ListPhoneNumbersOptedOutCommand` method of the `SNS` client class\. To call the `ListPhoneNumbersOptedOutCommand` method, create an asynchronous function invoking an Amazon SNS client service object, passing the parameters object\. 

```
// Import required AWS SDK clients and commands for Node.js
import {ListPhoneNumbersOptedOutCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

const run = async () => {
  try {
    const data = await snsClient.send(new ListPhoneNumbersOptedOutCommand({}));
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node sns_listnumbersoptedout.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/sns_listnumbersoptedout.js)\.
