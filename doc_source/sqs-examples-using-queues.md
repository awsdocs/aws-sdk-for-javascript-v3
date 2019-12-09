# Using Queues in Amazon SQS<a name="sqs-examples-using-queues"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to get a list of all of your message queues
+ How to obtain the URL for a particular queue
+ How to create and delete queues

## About the Example<a name="sqs-examples-using-queues-scenario"></a>

In this example, a series of Node\.js modules are used to work with queues\. The Node\.js modules use the SDK for JavaScript to enable queues to call the following methods of the `AWS.SQS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#listQueues-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#listQueues-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#createQueue-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#createQueue-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#getQueueUrl-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#getQueueUrl-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#deleteQueue-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#deleteQueue-property)

For more information about Amazon SQS messages, see [How Queues Work](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-how-it-works.html) in the *Amazon Simple Queue Service Developer Guide*\.

## Prerequisite Tasks<a name="sqs-examples-using-queues-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Listing Your Queues<a name="sqs-examples-using-queues-listing-queues"></a>

Create a Node\.js module with the file name `sqs_listqueues.js`\. Be sure to configure the SDK as previously shown\. To access Amazon SQS, create an `AWS.SQS` service object\. Create a JSON object containing the parameters needed to list your queues, which by default is an empty object\. Call the `listQueues` method to retrieve the list of queues\. The callback returns the URLs of all queues\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create an SQS service object
var sqs = new AWS.SQS({apiVersion: '2012-11-05'});

var params = {};

sqs.listQueues(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.QueueUrls);
  }
});
```

To run the example, type the following at the command line\.

```
node sqs_listqueues.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sqs/sqs_listqueues.js)\.

## Creating a Queue<a name="sqs-examples-using-queues-create-queue"></a>

Create a Node\.js module with the file name `sqs_createqueue.js`\. Be sure to configure the SDK as previously shown\. To access Amazon SQS, create an `AWS.SQS` service object\. Create a JSON object containing the parameters needed to list your queues, which must include the name for the queue created\. The parameters can also contain attributes for the queue, such as the number of seconds for which message delivery is delayed or the number of seconds to retain a received message\. Call the `createQueue` method\. The callback returns the URL of the created queue\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create an SQS service object
var sqs = new AWS.SQS({apiVersion: '2012-11-05'});

var params = {
  QueueName: 'SQS_QUEUE_NAME',
  Attributes: {
    'DelaySeconds': '60',
    'MessageRetentionPeriod': '86400'
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
node sqs_createqueue.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sqs/sqs_createqueue.js)\.

## Getting the URL for a Queue<a name="sqs-examples-using-queues-get-queue-url"></a>

Create a Node\.js module with the file name `sqs_getqueueurl.js`\. Be sure to configure the SDK as previously shown\. To access Amazon SQS, create an `AWS.SQS` service object\. Create a JSON object containing the parameters needed to list your queues, which must include the name of the queue whose URL you want\. Call the `getQueueUrl` method\. The callback returns the URL of the specified queue\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create an SQS service object
var sqs = new AWS.SQS({apiVersion: '2012-11-05'});

var params = {
  QueueName: 'SQS_QUEUE_NAME'
};

sqs.getQueueUrl(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.QueueUrl);
  }
});
```

To run the example, type the following at the command line\.

```
node sqs_getqueueurl.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sqs/sqs_getqueueurl.js)\.

## Deleting a Queue<a name="sqs-examples-using-queues-delete-queue"></a>

Create a Node\.js module with the file name `sqs_deletequeue.js`\. Be sure to configure the SDK as previously shown\. To access Amazon SQS, create an `AWS.SQS` service object\. Create a JSON object containing the parameters needed to delete a queue, which consists of the URL of the queue you want to delete\. Call the `deleteQueue` method\. 

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create an SQS service object
var sqs = new AWS.SQS({apiVersion: '2012-11-05'});

var params = {
  QueueUrl: 'SQS_QUEUE_URL'
 };

sqs.deleteQueue(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node sqs_deletequeue.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sqs/sqs_deletequeue.js)\.