--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Using dead\-letter queues in Amazon SQS<a name="sqs-examples-dead-letter-queues"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to use a queue to receive and hold messages from other queues that the queues can't process\.

## The scenario<a name="sqs-examples-dead-letter-queues-scenario"></a>

A dead\-letter queue is one that other \(source\) queues can target for messages that can't be processed successfully\. You can set aside and isolate these messages in the dead\-letter queue to determine why their processing did not succeed\. You must individually configure each source queue that sends messages to a dead\-letter queue\. Multiple queues can target a single dead\-letter queue\.

In this example, a Node\.js module is used to route messages to a dead letter queue\. The Node\.js module uses the SDK for JavaScript to use dead letter queues using this method of the `SQS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/setqueueattributescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/setqueueattributescommand.html)

For more information about Amazon SQS dead\-letter queues, see [Using Amazon SQS dead\-letter queues](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-dead-letter-queues.html) in the *Amazon Simple Queue Service Developer Guide*\.

## Prerequisite tasks<a name="sqs-examples-dead-letter-queues-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create an Amazon SQS queue to serve as a dead\-letter queue\. For an example of creating a queue, see [Using queues in Amazon SQS](sqs-examples-using-queues.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Configuring source queues<a name="sqs-examples-dead-letter-queues-configuring-source-queues"></a>

After you create a queue to act as a dead\-letter queue, you must configure the other queues that route unprocessed messages to the dead\-letter queue\. To do this, specify a redrive policy that identifies the queue to use as a dead\-letter queue and the maximum number of receives by individual messages before they are routed to the dead\-letter queue\.

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

Create a Node\.js module with the file name `sqs_deadletterqueue.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. Create a JSON object containing the parameters needed to update queue attributes, including the `RedrivePolicy` parameter that specifies both the ARN of the dead\-letter queue, as well as the value of `maxReceiveCount`\. Also specify the URL source queue you want to configure\. Call the `SetQueueAttributesCommand` method\.

**Note**  
Replace *SQS\_QUEUE\_URL* with the URL of the SQS queue, and *DEAD\_LETTER\_QUEUE\_ARN* with the ARN of the dead letter queue\.

```
// Import required AWS SDK clients and commands for Node.js
import { SetQueueAttributesCommand } from  "@aws-sdk/client-sqs";
import { sqsClient } from  "./libs/sqsClient.js";

// Set the parameters
var params = {
  Attributes: {
    RedrivePolicy:
      '{"deadLetterTargetArn":"DEAD_LETTER_QUEUE_ARN",' +
      '"maxReceiveCount":"10"}', //DEAD_LETTER_QUEUE_ARN
  },
  QueueUrl: "SQS_QUEUE_URL", //SQS_QUEUE_URL
};

const run = async () => {
  try {
    const data = await sqsClient.send(new SetQueueAttributesCommand(params));
    console.log("Success", data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node sqs_deadletterqueue.js // If you prefer JavaScript, enter 'sqs_deadletterqueue.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs//src/sqs_deadletterqueue.js)\.