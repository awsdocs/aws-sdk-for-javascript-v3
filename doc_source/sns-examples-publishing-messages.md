--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Publishing Messages in Amazon SNS<a name="sns-examples-publishing-messages"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to publish messages to an Amazon SNS topic\.

## The Scenario<a name="sns-examples-publishing-messages-scenario"></a>

In this example, you use a series of Node\.js modules to publish messages from Amazon SNS to topic endpoints, emails, or phone numbers\. The Node\.js modules use the SDK for JavaScript to send messages using this method of the `SNS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/publishcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/publishcommand.html)

## Prerequisite Tasks<a name="sns-examples-publishing-messages-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/sns/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Publishing a Message to an SNS Topic<a name="sns-examples-publishing-text-messages"></a>

In this example, use a Node\.js module to publish a message to an Amazon SNS topic\.

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

 Create a Node\.js module with the file name `sns_publishtotopic.js`\. Configure the SDK as previously shown\.

Create an object containing the parameters for publishing a message, including the message text and the Amazon Resource Name \(ARN\) of the Amazon SNStopic\. For details on available SMS attributes, see [SetSMSAttributes](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#setSMSAttributes-property)\.

Pass the parameters to the `PublishCommand` method of the `SNS` client class\. create an asynchronous function invoking an Amazon SNS client service object, passing the parameters object\. 

**Note**  
Replace *MESSAGE\_TEXT* with the message text, and *TOPIC\_ARN* with the ARN of the SNS topic\.

```
// Import required AWS SDK clients and commands for Node.js
import {PublishCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = {
  Message: "MESSAGE_TEXT", // MESSAGE_TEXT
  TopicArn: "TOPIC_ARN", //TOPIC_ARN
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
node sns_publishtotopic.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/sns_publishtotopic.js)\.
