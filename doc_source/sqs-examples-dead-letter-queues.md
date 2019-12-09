# Using Dead Letter Queues in Amazon SQS<a name="sqs-examples-dead-letter-queues"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to use a queue to receive and hold messages from other queues that the queues can't process\.

## The Scenario<a name="sqs-examples-dead-letter-queues-scenario"></a>

A dead letter queue is one that other \(source\) queues can target for messages that can't be processed successfully\. You can set aside and isolate these messages in the dead letter queue to determine why their processing did not succeed\. You must individually configure each source queue that sends messages to a dead letter queue\. Multiple queues can target a single dead letter queue\.

In this example, a Node\.js module is used to route messages to a dead letter queue\. The Node\.js module uses the SDK for JavaScript to use dead letter queues using this method of the `AWS.SQS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#setQueueAttributes-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#setQueueAttributes-property)

For more information about Amazon SQS dead letter queues, see [Using Amazon SQS Dead Letter Queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html) in the *Amazon Simple Queue Service Developer Guide*\.

## Prerequisite Tasks<a name="sqs-examples-dead-letter-queues-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.
+ Create an Amazon SQS queue to serve as a dead letter queue\. For an example of creating a queue, see [Using Queues in Amazon SQS](sqs-examples-using-queues.md)\.

## Configuring Source Queues<a name="sqs-examples-dead-letter-queues-configuring-source-queues"></a>

After you create a queue to act as a dead letter queue, you must configure the other queues that route unprocessed messages to the dead letter queue\. To do this, specify a redrive policy that identifies the queue to use as a dead letter queue and the maximum number of receives by individual messages before they are routed to the dead letter queue\.

Create a Node\.js module with the file name `sqs_deadletterqueue.js`\. Be sure to configure the SDK as previously shown\. To access Amazon SQS, create an `AWS.SQS` service object\. Create a JSON object containing the parameters needed to update queue attributes, including the `RedrivePolicy` parameter that specifies both the ARN of the dead letter queue, as well as the value of `maxReceiveCount`\. Also specify the URL source queue you want to configure\. Call the `setQueueAttributes` method\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the SQS service object
var sqs = new AWS.SQS({apiVersion: '2012-11-05'});

var params = {
 Attributes: {
  "RedrivePolicy": "{\"deadLetterTargetArn\":\"DEAD_LETTER_QUEUE_ARN\",\"maxReceiveCount\":\"10\"}",
 },
 QueueUrl: "SOURCE_QUEUE_URL"
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
node sqs_deadletterqueue.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sqs/sqs_deadletterqueue.js)\.