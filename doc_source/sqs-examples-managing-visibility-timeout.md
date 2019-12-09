# Managing Visibility Timeout in Amazon SQS<a name="sqs-examples-managing-visibility-timeout"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to specify the time interval during which messages received by a queue are not visible\.

## The Scenario<a name="sqs-examples-managing-visibility-timeout-scenario"></a>

In this example, a Node\.js module is used to manage visibility timeout\. The Node\.js module uses the SDK for JavaScript to manage visibility timeout by using this method of the `AWS.SQS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#changeMessageVisibility-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html#changeMessageVisibility-property)

For more information about Amazon SQS visibility timeout, see [Visibility Timeout](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-visibility-timeout.html) in the *Amazon Simple Queue Service Developer Guide*\.

## Prerequisite Tasks<a name="sqs-examples-managing-visibility-timeout-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.
+ Create an Amazon SQS queue\. For an example of creating a queue, see [Using Queues in Amazon SQS](sqs-examples-using-queues.md)\.
+ Send a message to the queue\. For an example of sending a message to a queue, see [Sending and Receiving Messages in Amazon SQS](sqs-examples-send-receive-messages.md)\.

## Changing the Visibility Timeout<a name="sqs-examples-managing-visibility-timeout-setting"></a>

Create a Node\.js module with the file name `sqs_changingvisibility.js`\. Be sure to configure the SDK as previously shown\. To access Amazon Simple Queue Service, create an `AWS.SQS` service object\. Receive the message from the queue\.

Upon receipt of the message from the queue, create a JSON object containing the parameters needed for setting the timeout, including the URL of the queue containing the message, the `ReceiptHandle` returned when the message was received, and the new timeout in seconds\. Call the `changeMessageVisibility` method\. 

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk')
// Set the region to us-west-2
AWS.config.update({ region: 'us-west-2' })

// Create the SQS service object
var sqs = new AWS.SQS({ apiVersion: '2012-11-05' })

var queueURL = 'https://sqs.REGION.amazonaws.com/ACCOUNT-ID/QUEUE-NAME'

var params = {
  AttributeNames: ['SentTimestamp'],
  MaxNumberOfMessages: 1,
  MessageAttributeNames: ['All'],
  QueueUrl: queueURL
}

sqs.receiveMessage(params, function (err, data) {
  if (err) {
    console.log('Receive Error', err)
  } else {
    // Make sure we have a message
    if (data.Messages != null) {
      var visibilityParams = {
        QueueUrl: queueURL,
        ReceiptHandle: data.Messages[0].ReceiptHandle,
        VisibilityTimeout: 20 // 20 second timeout
      }
      sqs.changeMessageVisibility(visibilityParams, function (err, data) {
        if (err) {
          console.log('Delete Error', err)
        } else {
          console.log('Timeout Changed', data)
        }
      })
    } else {
      console.log('No messages to change')
    }
  }
})
```

To run the example, type the following at the command line\.

```
node sqs_changingvisibility.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sqs/sqs_changingvisibility.js)\.