--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Publishing Messages in Amazon SNS<a name="sns-examples-publishing-messages"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to publish messages to an Amazon SNS topic\.

## The Scenario<a name="sns-examples-publishing-messages-scenario"></a>

In this example, you use a series of Node\.js modules to publish messages from Amazon SNS to topic endpoints, emails, or phone numbers\. The Node\.js modules use the SDK for JavaScript to send messages using this method of the `SNS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#publish-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#publish-property)

## Prerequisite Tasks<a name="sns-examples-publishing-messages-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up a project environment to run Node TypeScript examples\. For more information, see[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/sns/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Publishing a Message to an SNS Topic<a name="sns-examples-publishing-text-messages"></a>

In this example, use a Node\.js module to publish a message to an Amazon SNS topic\. Create a Node\.js module with the file name `sns_publishtotopic.ts`\. Configure the SDK as previously shown\.

Create an object containing the parameters for publishing a message, including the message text and the Amazon Resource Name \(ARN\) of the Amazon SNStopic\. For details on available SMS attributes, see [SetSMSAttributes](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#setSMSAttributes-property)\.

Pass the parameters to the `PublishCommand` method of the `SNS` client class\. create an asynchronous function invoking an Amazon SNS client service object, passing the parameters object\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *MESSAGE\_TEXT* with the message text, and *TOPIC\_ARN* with the ARN of the SNS topic\.

```
// Import required AWS SDK clients and commands for Node.js
const { SNSClient, PublishCommand } = require("@aws-sdk/client-sns");

// Set the AWS Region
const REGION = "region"; //e.g. "us-east-1"

// Set the parameters
var params = {
  Message: "MESSAGE_TEXT", // MESSAGE_TEXT
  TopicArn: "TOPIC_ARN", //TOPIC_ARN
};

// Create SNS service object
const sns = new SNSClient(REGION);

const run = async () => {
  try {
    const data = await sns.send(new PublishCommand(params));
    console.log("Message sent to the topic");
    console.log("MessageID is " + data.MessageId);
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node sns_publishtotopic.ts // If you prefer JavaScript, enter 'node sns_publishtotopic.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/sns_publishtotopic.ts)\.