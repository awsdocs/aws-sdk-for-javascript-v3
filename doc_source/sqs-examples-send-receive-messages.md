--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Sending and receiving messages in Amazon SQS<a name="sqs-examples-send-receive-messages"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to send messages in a queue\.
+ How to receive messages in a queue\.
+ How to delete messages in a queue\.

## The scenario<a name="sqs-examples-send-receive-messages-scenario"></a>

In this example, a series of Node\.js modules are used to send and receive messages\. The Node\.js modules use the SDK for JavaScript to send and receive messages by using these methods of the `SQS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/sendmessagecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/sendmessagecommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/receivemessagecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/receivemessagecommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/deletemessagecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/deletemessagecommand.html)

For more information about Amazon SQS messages, see [Sending a message to an Amazon SQS queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-send-message.html) and [Receiving and deleting a message from an Amazon SQS queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-receive-delete-message.html) in the *Amazon Simple Queue Service Developer Guide*\.

## Prerequisite tasks<a name="sqs-examples-send-receive-messages-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/sqs/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create an Amazon SQS queue\. For an example of creating a queue, see [Using queues in Amazon SQS](sqs-examples-using-queues.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Sending a message to a queue<a name="sqs-examples-send-receive-messages-sending"></a>

Create a `libs` directory, and create a Node\.js module with the file name `sqsClient.js`\. Copy and paste the code below into it, which creates the Amazon SQS client object\. Replace *REGION* with your AWS Region\.

```
import  { SQSClient } from "@aws-sdk/client-sqs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SQS service object.
const sqsClient = new SQSClient({ region: REGION });
export  { sqsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/libs/sqsClient.js)\.

Create a Node\.js module with the file name `sqs_sendmessage.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. Create a JSON object containing the parameters needed for your message, including the URL of the queue to which you want to send this message\. In this example, the message provides details about a book on a list of fiction best sellers including the title, author, and number of weeks on the list\.

Call the `SendMessageCommand` method\. The callback returns the unique ID of the message\.

**Note**  
Replace *SQS\_QUEUE\_URL* with the URL of the SQS queue\.

```
// Import required AWS SDK clients and commands for Node.js
import { SendMessageCommand } from  "@aws-sdk/client-sqs";
import { sqsClient } from  "./libs/sqsClient.js";

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
  QueueUrl: "SQS_QUEUE_URL" //SQS_QUEUE_URL; e.g., 'https://sqs.REGION.amazonaws.com/ACCOUNT-ID/QUEUE-NAME'
};

const run = async () => {
  try {
    const data = await sqsClient.send(new SendMessageCommand(params));
    console.log("Success, message sent. MessageID:", data.MessageId);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node sqs_sendmessage.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/sqs_sendmessage.js)\.

## Receiving and deleting messages from a queue<a name="sqs-examples-send-receive-messages-receiving"></a>

Create a `libs` directory, and create a Node\.js module with the file name `sqsClient.js`\. Copy and paste the code below into it, which creates the Amazon SQS client object\. Replace *REGION* with your AWS Region\.

```
import  { SQSClient } from "@aws-sdk/client-sqs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SQS service object.
const sqsClient = new SQSClient({ region: REGION });
export  { sqsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/libs/sqsClient.js)\.

Create a Node\.js module with the file name `sqs_deletemessage.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. Create a JSON object containing the parameters needed for your message, including the URL of the queue from which you want to receive messages\. In this example, the parameters specify receipt of all message attributes, as well as receipt of no more than 10 messages\.

Call the `ReceiveMessageCommand` method\. The callback returns an array of `Message` objects from which you can retrieve `ReceiptHandle` for each message that you use to later delete that message\. Create another JSON object containing the parameters needed to delete the message, which are the URL of the queue and the `ReceiptHandle` value\. Call the `DeleteMessageCommand` method to delete the message you received\.

**Note**  
Replace and *SQS\_QUEUE\_URL* with the URL of the SQS queue\.

```
// Import required AWS SDK clients and commands for Node.js
import {
  ReceiveMessageCommand,
  DeleteMessageCommand,
} from  "@aws-sdk/client-sqs";
import { sqsClient } from  "./libs/sqsClient.js";

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

const run = async () => {
  try {
    const data = await sqsClient.send(new ReceiveMessageCommand(params));
    if (data.Messages) {
      var deleteParams = {
        QueueUrl: queueURL,
        ReceiptHandle: data.Messages[0].ReceiptHandle,
      };
      try {
        const data = await sqsClient.send(new DeleteMessageCommand(deleteParams));
        console.log("Message deleted", data);
      } catch (err) {
        console.log("Error", err);
      }
    } else {
      console.log("No messages to delete");
    }
    return data; // For unit tests.
  } catch (err) {
    console.log("Receive Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node sqs_deletemessage.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/sqs_deletemessage.js)\.