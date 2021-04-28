--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Managing visibility timeout in Amazon SQS<a name="sqs-examples-managing-visibility-timeout"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to specify the time interval during which messages received by a queue are not visible\.

## The scenario<a name="sqs-examples-managing-visibility-timeout-scenario"></a>

In this example, a Node\.js module is used to manage visibility timeout\. The Node\.js module uses the SDK for JavaScript to manage visibility timeout by using this method of the `SQS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/changemessagevisibilitycommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/changemessagevisibilitycommand.html)

For more information about Amazon SQS visibility timeout, see [Visibility timeout](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-visibility-timeout.html) in the *Amazon Simple Queue Service Developer Guide*\.

## Prerequisite tasks<a name="sqs-examples-managing-visibility-timeout-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/sqs/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypeScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these examples can also be run in JavaScript\. For more information, see [this article](https://aws.amazon.com/blogs/developer/first-class-typescript-support-in-modular-aws-sdk-for-javascript/) in the AWS Developer Blog\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create an Amazon SQS queue\. For an example of creating a queue, see [Using queues in Amazon SQS](sqs-examples-using-queues.md)\.
+ Send a message to the queue\. For an example of sending a message to a queue, see [Sending and receiving messages in Amazon SQS](sqs-examples-send-receive-messages.md)\.

## Changing the visibility timeout<a name="sqs-examples-managing-visibility-timeout-setting"></a>

Create a Node\.js module with the file name `sqs_changingvisibility.ts`\. Be sure to configure the SDK as previously shown\. To access Amazon SQS, create an `SQS` client service object\. Receive the message from the queue\.

Upon receipt of the message from the queue, create a JSON object containing the parameters needed for setting the timeout, including the URL of the queue containing the message, the `ReceiptHandle` returned when the message was received, and the new timeout in seconds\. Call the `ChangeMessageVisibilityCommand` method\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *ACCOUNT\_ID* with the ID of the account, and *QUEUE\_NAME* with the name of the queue\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  SQSClient,
  ReceiveMessageCommand,
  ChangeMessageVisibilityCommand,
} = require("@aws-sdk/client-sqs");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const queueURL = "https://sqs.REGION.amazonaws.com/ACCOUNT-ID/QUEUE-NAME"; // REGION, ACCOUNT_ID, QUEUE_NAME
const params = {
  AttributeNames: ["SentTimestamp"],
  MaxNumberOfMessages: 1,
  MessageAttributeNames: ["All"],
  QueueUrl: queueURL,
};

// Create SQS service object
const sqs = new SQSClient({ region: REGION });

const run = async () => {
  try {
    const data = await sqs.send(new ReceiveMessageCommand(params));
    if (data.Messages != null) {
      try {
        var visibilityParams = {
          QueueUrl: queueURL,
          ReceiptHandle: data.Messages[0].ReceiptHandle,
          VisibilityTimeout: 20, // 20 second timeout
        };
        const results = await sqs.send(
          new ChangeMessageVisibilityCommand(params)
        );
        console.log("Timeout Changed", results);
      } catch (err) {
        console.log("Delete Error", err);
      }
    } else {
      console.log("No messages to change");
    }
  } catch (err) {
    console.log("Receive Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node sqs_changingvisibility.ts // If you prefer JavaScript, enter 'sqs_changingvisibility.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/sqs_changingvisibility.ts)\.