--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Sending and receiving messages in Amazon SQS<a name="sqs-examples-send-receive-messages"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to send messages in a queue\.
+ How to receive messages in a queue\.
+ How to delete messages in a queue\.

## The scenario<a name="sqs-examples-send-receive-messages-scenario"></a>

In this example, a series of Node\.js modules are used to send and receive messages\. The Node\.js modules use the SDK for JavaScript to send and receive messages by using these methods of the `SQS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#sendMessage-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#sendMessage-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#receiveMessage-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#receiveMessage-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#deleteMessage-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#deleteMessage-property)

For more information about Amazon SQS messages, see [Sending a message to an Amazon SQS queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-send-message.html) and [Receiving and deleting a message from an Amazon SQS queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-receive-delete-message.html) in the *Amazon Simple Queue Service Developer Guide*\.

## Prerequisite tasks<a name="sqs-examples-send-receive-messages-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/sqs/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create an Amazon SQS queue\. For an example of creating a queue, see [Using queues in Amazon SQS](sqs-examples-using-queues.md)\.

## Sending a message to a queue<a name="sqs-examples-send-receive-messages-sending"></a>

Create a Node\.js module with the file name `sqs_sendmessage.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access Amazon SQS, create an `SQS` client service object\. Create a JSON object containing the parameters needed for your message, including the URL of the queue to which you want to send this message\. In this example, the message provides details about a book on a list of fiction best sellers including the title, author, and number of weeks on the list\.

Call the `SendMessageCommand` method\. The callback returns the unique ID of the message\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *SQS\_QUEUE\_URL* with the URL of the SQS queue\.

```
// Import required AWS SDK clients and commands for Node.js
const { SQSClient, SendMessageCommand } = require("@aws-sdk/client-sqs");

// Set the AWS Region
const REGION = "region"; //e.g. "us-east-1"

// Set the parameters
const params = {
  DelaySeconds: 10,
  MessageAttributes: {
    Title: {
      DataType: "String",
      StringValue: "The Whistler",
    },
    Author: {
      DataType: "String",
      StringValue: "John Grisham",
    },
    WeeksOn: {
      DataType: "Number",
      StringValue: "6",
    },
  },
  MessageBody:
    "Information about current NY Times fiction bestseller for week of 12/11/2016.",
  // MessageDeduplicationId: "TheWhistler",  // Required for FIFO queues
  // MessageGroupId: "Group1",  // Required for FIFO queues
  QueueUrl: "SQS_QUEUE_URL", //SQS_QUEUE_URL; e.g., 'https://sqs.REGION.amazonaws.com/ACCOUNT-ID/QUEUE-NAME'
};

// Create SQS service object
const sqs = new SQSClient(REGION);

const run = async () => {
  try {
    const data = await sqs.send(new SendMessageCommand(params));
    console.log("Success, message sent. MessageID:", data.MessageId);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node sqs_sendmessage.ts // If you prefer JavaScript, enter 'sqs_sendmessage.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/sqs_sendmessage.ts)\.

## Receiving and deleting messages from a queue<a name="sqs-examples-send-receive-messages-receiving"></a>

Create a Node\.js module with the file name `sqs_receivemessage.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access Amazon SQS, create an `SQS` service object\. Create a JSON object containing the parameters needed for your message, including the URL of the queue from which you want to receive messages\. In this example, the parameters specify receipt of all message attributes, as well as receipt of no more than 10 messages\.

Call the `ReceiveMessageCommand` method\. The callback returns an array of `Message` objects from which you can retrieve `ReceiptHandle` for each message that you use to later delete that message\. Create another JSON object containing the parameters needed to delete the message, which are the URL of the queue and the `ReceiptHandle` value\. Call the `DeleteMessageCommand` method to delete the message you received\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *SQS\_QUEUE\_URL* with the URL of the SQS queue\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  SQSClient,
  ReceiveMessageCommand,
  DeleteMessageCommand,
} = require("@aws-sdk/client-sqs");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const queueURL = "SQS_QUEUE_URL"; //SQS_QUEUE_URL; e.g., 'https://sqs.REGION.amazonaws.com/ACCOUNT-ID/QUEUE-NAME'
const params = {
  AttributeNames: ["SentTimestamp"],
  MaxNumberOfMessages: 10,
  MessageAttributeNames: ["All"],
  QueueUrl: queueURL,
  VisibilityTimeout: 20,
  WaitTimeSeconds: 0,
};

// Create SQS service object
const sqs = new SQSClient(REGION);

const run = async () => {
  try {
    const data = await sqs.send(new ReceiveMessageCommand(params));
    if (data.Messages) {
      var deleteParams = {
        QueueUrl: queueURL,
        ReceiptHandle: data.Messages[0].ReceiptHandle,
      };
      try {
        const data = await sqs.send(new DeleteMessageCommand(deleteParams));
        console.log("Message Deleted", data);
      } catch (err) {
        console.log("Delete Error", err);
      }
    } else {
      console.log("No messages to delete");
    }
  } catch (err) {
    console.log("Receive Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node sqs_receivemessage.ts // If you prefer JavaScript, enter 'sqs_receivemessage.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/sqs_receivemessage.ts)\.
