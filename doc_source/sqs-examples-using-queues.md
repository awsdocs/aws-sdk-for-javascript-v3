--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Using queues in Amazon SQS<a name="sqs-examples-using-queues"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to get a list of all of your message queues\.
+ How to obtain the URL for a particular queue\.
+ How to create and delete queues\.

## About the example<a name="sqs-examples-using-queues-scenario"></a>

In this example, a series of Node\.js modules are used to work with queues\. The Node\.js modules use the SDK for JavaScript to enable queues to call the following methods of the `SQS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/listqueuescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/listqueuescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/createqueuecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/createqueuecommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/getqueueurlcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/getqueueurlcommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/deletequeuecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/deletequeuecommand.html)

For more information about Amazon SQS messages, see [How queues work](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-how-it-works.html) in the *Amazon Simple Queue Service Developer Guide*\.

## Prerequisite tasks<a name="sqs-examples-using-queues-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/sqs/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Listing your queues<a name="sqs-examples-using-queues-listing-queues"></a>

Create a `libs` directory, and create a Node\.js module with the file name `sqsClient.js`\. Copy and paste the code below into it, which creates the Amazon SQS client object\. Replace *REGION* with your AWS Region\.

```
import  { SQSClient } from "@aws-sdk/client-sqs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const sqsClient = new SQSClient({ region: REGION });
export  { sqsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/libs/sqsClient.js)\.

Create a Node\.js module with the file name `sqs_listqueues.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. Create a JSON object containing the parameters needed to list your queues, which by default is an empty object\. Call the `ListQueuesCommand` method to retrieve the list of queues\. The function returns the URLs of all queues\.

```
// Import required AWS SDK clients and commands for Node.js
import { ListQueuesCommand } from  "@aws-sdk/client-sqs";
import { sqsClient } from  "./libs/sqsClient.js";

const run = async () => {
  try {
    const data = await sqsClient.send(new ListQueuesCommand({}));
    console.log("Success", data);
    return data; // For unit tests.
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node sqs_listqueues.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/sqs_listqueues.js)\.

## Creating a queue<a name="sqs-examples-using-queues-create-queue"></a>

Create a `libs` directory, and create a Node\.js module with the file name `sqsClient.js`\. Copy and paste the code below into it, which creates the Amazon SQS client object\. Replace *REGION* with your AWS Region\.

```
import  { SQSClient } from "@aws-sdk/client-sqs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const sqsClient = new SQSClient({ region: REGION });
export  { sqsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/libs/sqsClient.js)\.

Create a `libs` directory, and create a Node\.js module with the file name `sqsClient.js`\. Copy and paste the code below into it, which creates the Amazon SQS client object\. Replace *REGION* with your AWS Region\.

```
import  { SQSClient } from "@aws-sdk/client-sqs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const sqsClient = new SQSClient({ region: REGION });
export  { sqsClient };
```

```
import  { SQSClient } from "@aws-sdk/client-sqs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const sqsClient = new SQSClient({ region: REGION });
export { sqsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/libs/sqsClient.js)\.

Create a Node\.js module with the file name `sqs_createqueue.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. Create a JSON object containing the parameters needed to list your queues, which must include the name for the queue created\. The parameters can also contain attributes for the queue, such as the number of seconds for which message delivery is delayed or the number of seconds to retain a received message\. Call the `CreateQueueCommand` method\. The function returns the URL of the created queue\.

**Note**  
Replace *SQS\_QUEUE\_NAME* with the name of the Amazon SNS queue, *DelaySeconds* with the number of seconds for which message delivery is delayed, and *MessageRetentionPeriod* with the number of seconds to retain a received message\.

```
// Import required AWS SDK clients and commands for Node.js
import { CreateQueueCommand } from  "@aws-sdk/client-sqs";
import { sqsClient } from  "./libs/sqsClient.js";

// Set the parameters
const params = {
  QueueName: "SQS_QUEUE_NAME", //SQS_QUEUE_URL
  Attributes: {
    DelaySeconds: "60", // Number of seconds delay.
    MessageRetentionPeriod: "86400", // Number of seconds delay.
  },
};

const run = async () => {
  try {
    const data = await sqsClient.send(new CreateQueueCommand(params));
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
node sqs_createqueue.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/sqs_createqueue.js)\.

## Getting the URL for a queue<a name="sqs-examples-using-queues-get-queue-url"></a>

Create a `libs` directory, and create a Node\.js module with the file name `sqsClient.js`\. Copy and paste the code below into it, which creates the Amazon SQS client object\. Replace *REGION* with your AWS Region\.

```
import  { SQSClient } from "@aws-sdk/client-sqs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const sqsClient = new SQSClient({ region: REGION });
export  { sqsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/libs/sqsClient.js)\.

Create a Node\.js module with the file name `sqs_getqueueurl.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. Create a JSON object containing the parameters needed to list your queues, which must include the name of the queue whose URL you want\. Call the `GetQueueUrlCommand` method\. The function returns the URL of the specified queue\.

**Note**  
Replace and *SQS\_QUEUE\_NAME* with the SQS queue name\.

```
// Import required AWS SDK clients and commands for Node.js
import { GetQueueUrlCommand } from  "@aws-sdk/client-sqs";
import { sqsClient } from  "./libs/sqsClient.js";

// Set the parameters
const params = { QueueName: "SQS_QUEUE_NAME" };

const run = async () => {
  try {
    const data = await sqsClient.send(new GetQueueUrlCommand(params));
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
node sqs_getqueueurl.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/sqs_getqueueurl.js)\.

## Deleting a queue<a name="sqs-examples-using-queues-delete-queue"></a>

Create a `libs` directory, and create a Node\.js module with the file name `sqsClient.js`\. Copy and paste the code below into it, which creates the Amazon SQS client object\. Replace *REGION* with your AWS Region\.

```
import  { SQSClient } from "@aws-sdk/client-sqs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const sqsClient = new SQSClient({ region: REGION });
export  { sqsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/libs/sqsClient.js)\.

Create a Node\.js module with the file name `sqs_deletequeue.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. Create a JSON object containing the parameters needed to delete a queue, which consists of the URL of the queue you want to delete\. Call the `DeleteQueueCommand` method\. 

**Note**  
Replace *SQS\_QUEUE\_URL* with the URL of the Amazon SQS queue\.

```
// Import required AWS SDK clients and commands for Node.js
import { DeleteQueueCommand } from  "@aws-sdk/client-sqs";
import { sqsClient } from  "./libs/sqsClient.js";

// Set the parameters
const params = { QueueUrl: "SQS_QUEUE_URL" }; //SQS_QUEUE_URL e.g., 'https://sqs.REGION.amazonaws.com/ACCOUNT-ID/QUEUE-NAME'

const run = async () => {
  try {
    const data = await sqsClient.send(new DeleteQueueCommand(params));
    console.log("Success", data);
    return data; // For unit tests.
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node sqs_deletequeue.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sqs/src/sqs_deletequeue.js)\.