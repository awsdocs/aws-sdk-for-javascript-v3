# Sending and Receiving Messages in Amazon SQS<a name="sqs-examples-send-receive-messages"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to send messages in a queue\.
+ How to receive messages in a queue\.
+ How to delete messages in a queue\.

## The Scenario<a name="sqs-examples-send-receive-messages-scenario"></a>

In this example, a series of Node\.js modules are used to send and receive messages\. The Node\.js modules use the SDK for JavaScript to send and receive messages by using these methods of the `AWS.SQS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#sendMessage-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#sendMessage-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#receiveMessage-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#receiveMessage-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#deleteMessage-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#deleteMessage-property)

For more information about Amazon SQS messages, see [Sending a Message to an Amazon SQS Queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-send-message.html) and [Receiving and Deleting a Message from an Amazon SQS Queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-receive-delete-message.html) in the *Amazon Simple Queue Service Developer Guide*\.

## Prerequisite Tasks<a name="sqs-examples-send-receive-messages-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.
+ Create an Amazon SQS queue\. For an example of creating a queue, see [Using Queues in Amazon SQS](sqs-examples-using-queues.md)\.

## Sending a Message to a Queue<a name="sqs-examples-send-receive-messages-sending"></a>

Create a Node\.js module with the file name `sqs_sendmessage.js`\. Be sure to configure the SDK as previously shown\. To access Amazon SQS, create an `AWS.SQS` service object\. Create a JSON object containing the parameters needed for your message, including the URL of the queue to which you want to send this message\. In this example, the message provides details about a book on a list of fiction best sellers including the title, author, and number of weeks on the list\.

Call the `sendMessage` method\. The callback returns the unique ID of the message\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create an SQS service object
var sqs = new AWS.SQS({apiVersion: '2012-11-05'});

var params = {
  DelaySeconds: 10,
  MessageAttributes: {
    "Title": {
      DataType: "String",
      StringValue: "The Whistler"
    },
    "Author": {
      DataType: "String",
      StringValue: "John Grisham"
    },
    "WeeksOn": {
      DataType: "Number",
      StringValue: "6"
    }
  },
  MessageBody: "Information about current NY Times fiction bestseller for week of 12/11/2016.",
  // MessageDeduplicationId: "TheWhistler",  // Required for FIFO queues
  // MessageId: "Group1",  // Required for FIFO queues
  QueueUrl: "SQS_QUEUE_URL"
};

sqs.sendMessage(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.MessageId);
  }
});
```

To run the example, type the following at the command line\.

```
node sqs_sendmessage.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sqs/sqs_sendmessage.js)\.

## Receiving and Deleting Messages from a Queue<a name="sqs-examples-send-receive-messages-receiving"></a>

Create a Node\.js module with the file name `sqs_receivemessage.js`\. Be sure to configure the SDK as previously shown\. To access Amazon SQS, create an `AWS.SQS` service object\. Create a JSON object containing the parameters needed for your message, including the URL of the queue from which you want to receive messages\. In this example, the parameters specify receipt of all message attributes, as well as receipt of no more than 10 messages\.

Call the `receiveMessage` method\. The callback returns an array of `Message` objects from which you can retrieve `ReceiptHandle` for each message that you use to later delete that message\. Create another JSON object containing the parameters needed to delete the message, which are the URL of the queue and the `ReceiptHandle` value\. Call the `deleteMessage` method to delete the message you received\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region
AWS.config.update({region: 'REGION'});

// Create an SQS service object
var sqs = new AWS.SQS({apiVersion: '2012-11-05'});

var queueURL = "SQS_QUEUE_URL";

var params = {
 AttributeNames: [
    "SentTimestamp"
 ],
 MaxNumberOfMessages: 10,
 MessageAttributeNames: [
    "All"
 ],
 QueueUrl: queueURL,
 VisibilityTimeout: 20,
 WaitTimeSeconds: 0
};

sqs.receiveMessage(params, function(err, data) {
  if (err) {
    console.log("Receive Error", err);
  } else if (data.Messages) {
    var deleteParams = {
      QueueUrl: queueURL,
      ReceiptHandle: data.Messages[0].ReceiptHandle
    };
    sqs.deleteMessage(deleteParams, function(err, data) {
      if (err) {
        console.log("Delete Error", err);
      } else {
        console.log("Message Deleted", data);
      }
    });
  }
});
```

To run the example, type the following at the command line\.

```
node sqs_receivemessage.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sqs/sqs_receivemessage.js)\.