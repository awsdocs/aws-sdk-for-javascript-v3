--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Using queues in Amazon SQS<a name="sqs-examples-using-queues"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to get a list of all of your message queues\.
+ How to obtain the URL for a particular queue\.
+ How to create and delete queues\.

## About the example<a name="sqs-examples-using-queues-scenario"></a>

In this example, a series of Node\.js modules are used to work with queues\. The Node\.js modules use the SDK for JavaScript to enable queues to call the following methods of the `SQS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#listQueues-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#listQueues-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#createQueue-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#createQueue-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#getQueueUrl-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#getQueueUrl-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#deleteQueue-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#deleteQueue-property)

For more information about Amazon SQS messages, see [How queues work](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-how-it-works.html) in the *Amazon Simple Queue Service Developer Guide*\.

## Prerequisite tasks<a name="sqs-examples-using-queues-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/sqs/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Listing your queues<a name="sqs-examples-using-queues-listing-queues"></a>

Create a Node\.js module with the file name `sqs_listqueues.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access Amazon SQS, create an `SQS` client service object\. Create a JSON object containing the parameters needed to list your queues, which by default is an empty object\. Call the `ListQueuesCommand` method to retrieve the list of queues\. The function returns the URLs of all queues\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region\.

```
// Import required AWS SDK clients and commands for Node.js
const { SQSClient, ListQueuesCommand } = require("@aws-sdk/client-sqs");

// Set the AWS Region
const REGION = "region"; //e.g. "us-east-1"

// Create SQS service object
const sqs = new SQSClient(REGION);

const run = async () => {
  try {
    const data = await sqs.send(new ListQueuesCommand({}));
    console.log("Subscription ARN is " + data.SubscriptionArn);
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node sqs_listqueues.ts // If you prefer JavaScript, enter 'node sqs_listqueues.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/sqs_listqueues.ts)\.

## Creating a queue<a name="sqs-examples-using-queues-create-queue"></a>

Create a Node\.js module with the file name `sqs_createqueue.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access Amazon SQS, create an `SQS` service object\. Create a JSON object containing the parameters needed to list your queues, which must include the name for the queue created\. The parameters can also contain attributes for the queue, such as the number of seconds for which message delivery is delayed or the number of seconds to retain a received message\. Call the `CreateQueueCommand` method\. The function returns the URL of the created queue\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *SQS\_QUEUE\_NAME* with the name of the Amazon SNS queue, *DelaySeconds* with the number of seconds for which message delivery is delayed, and *MessageRetentionPeriod* with the number of seconds to retain a received message\.

```
// Import required AWS SDK clients and commands for Node.js
const { SQSClient, CreateQueueCommand } = require("@aws-sdk/client-sqs");

// Set the AWS Region
const REGION = "region"; //e.g. "us-east-1"

// Set the parameters
const params = {
  QueueName: "SQS_QUEUE_NAME", //SQS_QUEUE_URL
  Attributes: {
    DelaySeconds: "60", //number of seconds delay
    MessageRetentionPeriod: "86400", //number of seconds delay
  },
};

// Create SQS service object
const sqs = new SQSClient(REGION);

const run = async () => {
  try {
    const data = await sqs.send(new CreateQueueCommand(params));
    console.log("Success, new queue created. Queue URL: ", data.QueueUrl);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node sqs_createqueue.ts // If you prefer JavaScript, enter 'sqs_createqueue.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/sqs_createqueue.ts)\.

## Getting the URL for a queue<a name="sqs-examples-using-queues-get-queue-url"></a>

Create a Node\.js module with the file name `sqs_getqueueurl.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access Amazon SQS, create an `SQS` service object\. Create a JSON object containing the parameters needed to list your queues, which must include the name of the queue whose URL you want\. Call the `GetQueueUrlCommand` method\. The function returns the URL of the specified queue\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *SQS\_QUEUE\_NAME* with the SQS queue name\.

```
// Import required AWS SDK clients and commands for Node.js
const { SQSClient, GetQueueUrlCommand } = require("@aws-sdk/client-sqs");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { QueueName: "SQS_QUEUE_NAME" }; //SQS_QUEUE_NAME

// Create SQS service object
const sns = new SQSClient(REGION);

const run = async () => {
  try {
    const data = await sns.send(new GetQueueUrlCommand(params));
    console.log("Success, SQS queue URL:", data.QueueUrl);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node sqs_getqueueurl.ts // If you prefer JavaScript, enter 'sqs_getqueueurl.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/sqs_getqueueurl.ts)\.

## Deleting a queue<a name="sqs-examples-using-queues-delete-queue"></a>

Create a Node\.js module with the file name `sqs_deletequeue.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access Amazon SQS, create an `SQS` client service object\. Create a JSON object containing the parameters needed to delete a queue, which consists of the URL of the queue you want to delete\. Call the `DeleteQueueCommand` method\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *SQS\_QUEUE\_URL* with the URL of the Amazon SQS queue\.

```
// Import required AWS SDK clients and commands for Node.js
const { SQSClient, DeleteQueueCommand } = require("@aws-sdk/client-sqs");

// Set the AWS Region
const REGION = "region"; //e.g. "us-east-1"

// Set the parameters
const params = { QueueUrl: "SQS_QUEUE_URL" }; //SQS_QUEUE_URL e.g., 'https://sqs.REGION.amazonaws.com/ACCOUNT-ID/QUEUE-NAME'

// Create SQS service object
const sns = new SQSClient(REGION);

const run = async () => {
  try {
    const data = await sns.send(new DeleteQueueCommand(params));
    console.log("Success, queue deleted. RequestID:", data.$metadata.requestId);
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node sqs_deletequeue.ts // If you prefer JavaScript, enter 'sqs_deletequeue.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/sqs_deletequeue.ts)\.