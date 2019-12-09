# Enabling Long Polling in Amazon SQS<a name="sqs-examples-enable-long-polling"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to enable long polling for a newly created queue
+ How to enable long polling for an existing queue
+ How to enable long polling upon receipt of a message

## The Scenario<a name="sqs-examples-enable-long-polling-scenario"></a>

Long polling reduces the number of empty responses by allowing Amazon SQS to wait a specified time for a message to become available in the queue before sending a response\. Also, long polling eliminates false empty responses by querying all of the servers instead of a sampling of servers\. To enable long polling, you must specify a non\-zero wait time for received messages\. You can do this by setting the `ReceiveMessageWaitTimeSeconds` parameter of a queue or by setting the `WaitTimeSeconds` parameter on a message when it is received\.

In this example, a series of Node\.js modules are used to enable long polling\. The Node\.js modules use the SDK for JavaScript to enable long polling using these methods of the `AWS.SQS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#setQueueAttributes-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#setQueueAttributes-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#receiveMessage-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#receiveMessage-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#createQueue-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#createQueue-property)

For more information about Amazon SQS long polling, see [Long Polling](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-long-polling.html) in the *Amazon Simple Queue Service Developer Guide*\.

## Prerequisite Tasks<a name="sqs-examples-enable-long-polling-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Enabling Long Polling When Creating a Queue<a name="sqs-examples-enable-long-polling-on-queue-creation"></a>

Create a Node\.js module with the file name `sqs_longpolling_createqueue.js`\. Be sure to configure the SDK as previously shown\. To access Amazon SQS, create an `AWS.SQS` service object\. Create a JSON object containing the parameters needed to create a queue, including a non\-zero value for the `ReceiveMessageWaitTimeSeconds` parameter\. Call the `createQueue` method\. Long polling is then enabled for the queue\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the SQS service object
var sqs = new AWS.SQS({apiVersion: '2012-11-05'});

var params = {
  QueueName: 'SQS_QUEUE_NAME',
  Attributes: {
    'ReceiveMessageWaitTimeSeconds': '20',
  }
};

sqs.createQueue(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.QueueUrl);
  }
});
```

To run the example, type the following at the command line\.

```
node sqs_longpolling_createqueue.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sqs/sqs_longpolling_createqueue.js)\.

## Enabling Long Polling on an Existing Queue<a name="sqs-examples-enable-long-polling-existing-queue"></a>

Create a Node\.js module with the file name `sqs_longpolling_existingqueue.js`\. Be sure to configure the SDK as previously shown\. To access Amazon Simple Queue Service, create an `AWS.SQS` service object\. Create a JSON object containing the parameters needed to set the attributes of queue, including a non\-zero value for the `ReceiveMessageWaitTimeSeconds` parameter and the URL of the queue\. Call the `setQueueAttributes` method\. Long polling is then enabled for the queue\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the SQS service object
var sqs = new AWS.SQS({apiVersion: '2012-11-05'});

var params = {
 Attributes: {
  "ReceiveMessageWaitTimeSeconds": "20",
 },
 QueueUrl: "SQS_QUEUE_URL"
};

sqs.setQueueAttributes(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node sqs_longpolling_existingqueue.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sqs/sqs_longpolling_existingqueue.js)\.

## Enabling Long Polling on Message Receipt<a name="sqs-examples-enable-long-polling-on-receive-message"></a>

Create a Node\.js module with the file name `sqs_longpolling_receivemessage.js`\. Be sure to configure the SDK as previously shown\. To access Amazon Simple Queue Service, create an `AWS.SQS` service object\. Create a JSON object containing the parameters needed to receive messages, including a non\-zero value for the `WaitTimeSeconds` parameter and the URL of the queue\. Call the `receiveMessage` method\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the SQS service object
var sqs = new AWS.SQS({apiVersion: '2012-11-05'});

var queueURL = "SQS_QUEUE_URL";

var params = {
 AttributeNames: [
    "SentTimestamp"
 ],
 MaxNumberOfMessages: 1,
 MessageAttributeNames: [
    "All"
 ],
 QueueUrl: queueURL,
 WaitTimeSeconds: 20
};

sqs.receiveMessage(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node sqs_longpolling_receivemessage.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sqs/sqs_longpolling_receivemessage.js)\.