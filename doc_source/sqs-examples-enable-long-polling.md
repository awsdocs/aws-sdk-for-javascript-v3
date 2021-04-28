--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Enabling long polling in Amazon SQS<a name="sqs-examples-enable-long-polling"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to enable long polling for a newly created queue\.
+ How to enable long polling for an existing queue\.
+ How to enable long polling upon receipt of a message\.

## The scenario<a name="sqs-examples-enable-long-polling-scenario"></a>

Long polling reduces the number of empty responses by allowing Amazon SQS to wait a specified time for a message to become available in the queue before sending a response\. Also, long polling eliminates false empty responses by querying all of the servers instead of a sampling of servers\. To enable long polling, you must specify a non\-zero wait time for received messages\. You can do this by setting the `ReceiveMessageWaitTimeSeconds` parameter of a queue or by setting the `WaitTimeSeconds` parameter on a message when it is received\.

In this example, a series of Node\.js modules are used to enable long polling\. The Node\.js modules use the SDK for JavaScript to enable long polling using these methods of the `SQS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/setqueueattributescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/setqueueattributescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/receivemessagecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/receivemessagecommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/createqueuecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/createqueuecommand.html)

For more information about Amazon SQS long polling, see [Long polling](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-long-polling.html) in the *Amazon Simple Queue Service Developer Guide*\.

## Prerequisite tasks<a name="sqs-examples-enable-long-polling-prerequisites"></a>

To set up and run this example, you must first complete these tasks:

Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/sqs/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypeScript, so for consistency these examples are presented in TypeScript\. TypeScript is a super\-set of JavaScript so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Enabling long polling when creating a queue<a name="sqs-examples-enable-long-polling-on-queue-creation"></a>

Create a Node\.js module with the file name `sqs_longpolling_createqueue.ts`\. Be sure to configure the SDK as previously shown\. To access Amazon SQS, create an `SQS` client service object\. Create a JSON object containing the parameters needed to create a queue, including a non\-zero value for the `ReceiveMessageWaitTimeSeconds` parameter\. Call the `CreateQueueCommand` method\. Long polling is then enabled for the queue\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *SQS\_QUEUE\_URL* with the URL of the SQS queue\.

```
// Import required AWS SDK clients and commands for Node.js
const { SQSClient, CreateQueueCommand } = require("@aws-sdk/client-sqs");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  QueueName: "SQS_QUEUE_NAME", //SQS_QUEUE_URL; e.g., 'https://sqs.REGION.amazonaws.com/ACCOUNT-ID/QUEUE-NAME'
  Attributes: {
    ReceiveMessageWaitTimeSeconds: "20",
  },
};

// Create SQS service object
const sqs = new SQSClient({ region: REGION });

const run = async () => {
  try {
    const data = await sqs.send(new CreateQueueCommand(params));
    console.log(
      "Success, new SQS queue created. SQS queue URL:",
      data.QueueUrl
    );
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node sqs_longpolling_createqueue.ts // If you prefer JavaScript, enter 'sqs_longpolling_createqueue.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/sqs_longpolling_createqueue.ts)\.

## Enabling long polling on an existing queue<a name="sqs-examples-enable-long-polling-existing-queue"></a>

Create a Node\.js module with the file name `sqs_longpolling_existingqueue.ts`\. Be sure to configure the SDK as previously shown\. To access Amazon SQS, create an `SQS` client service object\. Create a JSON object containing the parameters needed to set the attributes of the queue, including a non\-zero value for the `ReceiveMessageWaitTimeSeconds` parameter and the URL of the queue\. Call the `SetQueueAttributesCommand` method\. Long polling is then enabled for the queue\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *SQS\_QUEUE\_URL* with the URL of the SQS queue, and *ReceiveMessageWaitTimeSeconds* with the number of seconds to wait before the message is received\.

```
// Import required AWS SDK clients and commands for Node.js
const { SQSClient, SetQueueAttributesCommand } = require("@aws-sdk/client-sqs");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  Attributes: {
    ReceiveMessageWaitTimeSeconds: "20",
  },
  QueueUrl: "SQS_QUEUE_URL", //SQS_QUEUE_URL; e.g., 'https://sqs.REGION.amazonaws.com/ACCOUNT-ID/QUEUE-NAME'
};

// Create SQS service object
const sqs = new SQSClient({ region: REGION });

const run = async () => {
  try {
    const data = await sqs.send(new SetQueueAttributesCommand(params));
    console.log(
      "Success, longpolling enabled on queue. RequestID:",
      data.$metadata.requestId
    );
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node sqs_longpolling_existingqueue.ts // If you prefer JavaScript, enter 'sqs_longpolling_existingqueue.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/sqs_longpolling_existingqueue.ts)\.

## Enabling long polling on message receipt<a name="sqs-examples-enable-long-polling-on-receive-message"></a>

Create a Node\.js module with the file name `sqs_longpolling_receivemessage.ts`\. Be sure to configure the SDK as previously shown\. To access Amazon SQS, create an `SQS` client service object\. Create a JSON object containing the parameters needed to receive messages, including a non\-zero value for the `WaitTimeSeconds` parameter and the URL of the queue\. Call the `ReceiveMessageCommand` method\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.
Replace *REGION* with your AWS Region, *SQS\_QUEUE\_URL* with the URL of the SQS queue, *MaxNumberOfMessages* with the maximum number of messages, and *WaitTimeSeconds* with the time to wait \(in seconds\)\.

```
// Import required AWS SDK clients and commands for Node.js
const { SQSClient, ReceiveMessageCommand } = require("@aws-sdk/client-sqs");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const queueURL = "SQS_QUEUE_URL"; // SQS_QUEUE_URL
const params = {
  AttributeNames: ["SentTimestamp"],
  MaxNumberOfMessages: 1,
  MessageAttributeNames: ["All"],
  QueueUrl: queueURL,
  WaitTimeSeconds: 20,
};

// Create SQS service object
const sqs = new SQSClient({ region: REGION });

const run = async () => {
  try {
    const data = await sqs.send(new ReceiveMessageCommand(params));
    console.log("Success, ", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node sqs_longpolling_receivemessae.ts // If you prefer JavaScript, enter 'sqs_longpolling_receivemessage.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/sqs_longpolling_receivemessage.ts)\.