--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Amazon SQS examples using SDK for JavaScript V3<a name="javascript_sqs_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for JavaScript V3 with Amazon SQS\.

*Actions* are code excerpts that show you how to call individual Amazon SQS functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple Amazon SQS functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w362aac23b9c31c13)

## Actions<a name="w362aac23b9c31c13"></a>

### Change how long a queue waits for a message<a name="sqs_SetQueueAttributes_javascript_topic"></a>

The following code example shows how to change how long an Amazon SQS queue waits for a message to arrive\.

**SDK for JavaScript V3**  
Create the client\.  

```
import  { SQSClient } from "@aws-sdk/client-sqs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SQS service object.
const sqsClient = new SQSClient({ region: REGION });
export  { sqsClient };
```
Change how long an Amazon SQS queue waits for a message to arrive\.  

```
// Import required AWS SDK clients and commands for Node.js
import { SetQueueAttributesCommand } from  "@aws-sdk/client-sqs";
import { sqsClient } from  "./libs/sqsClient.js";

// Set the parameters
const params = {
  Attributes: {
    ReceiveMessageWaitTimeSeconds: "20",
  },
  QueueUrl: "SQS_QUEUE_URL", //SQS_QUEUE_URL; e.g., 'https://sqs.REGION.amazonaws.com/ACCOUNT-ID/QUEUE-NAME'
};

const run = async () => {
  try {
    const data = await sqsClient.send(new SetQueueAttributesCommand(params));
    console.log("Success", data);
    return data; // For unit tests.
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/sqs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/sqs-examples-enable-long-polling.html#sqs-examples-enable-long-polling-existing-queue)\. 
+  For API details, see [SetQueueAttributes](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/setqueueattributescommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
Change how long an Amazon SQS queue waits for a message to arrive\.  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/sqs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/sqs-examples-enable-long-polling.html#sqs-examples-enable-long-polling-existing-queue)\. 
+  For API details, see [SetQueueAttributes](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sqs-2012-11-05/SetQueueAttributes) in *AWS SDK for JavaScript API Reference*\. 

### Change message timeout visibility<a name="sqs_ChangeMessageVisibility_javascript_topic"></a>

The following code example shows how to change an Amazon SQS message timeout visibility\.

**SDK for JavaScript V3**  
Create the client\.  

```
import  { SQSClient } from "@aws-sdk/client-sqs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SQS service object.
const sqsClient = new SQSClient({ region: REGION });
export  { sqsClient };
```
Receive an Amazon SQS message and change its timeout visibility\.  

```
// Import required AWS SDK clients and commands for Node.js
import {
  ReceiveMessageCommand,
  ChangeMessageVisibilityCommand,
} from  "@aws-sdk/client-sqs";
import { sqsClient } from  "./libs/sqsClient.js";

// Set the parameters
const queueURL = "https://sqs.REGION.amazonaws.com/ACCOUNT-ID/QUEUE-NAME"; // REGION, ACCOUNT_ID, QUEUE_NAME
const params = {
  AttributeNames: ["SentTimestamp"],
  MaxNumberOfMessages: 1,
  MessageAttributeNames: ["All"],
  QueueUrl: queueURL,
};


const run = async () => {
  try {
    const data = await sqsClient.send(new ReceiveMessageCommand(params));
    if (data.Messages != null) {
      try {
        var visibilityParams = {
          QueueUrl: queueURL,
          ReceiptHandle: data.Messages[0].ReceiptHandle,
          VisibilityTimeout: 20, // 20 second timeout
        };
        const results = await sqsClient.send(
          new ChangeMessageVisibilityCommand(params)
        );
        console.log("Timeout Changed", results);
      } catch (err) {
        console.log("Delete Error", err);
      }
    } else {
      console.log("No messages to change");
    }
    return data; // For unit tests.
  } catch (err) {
    console.log("Receive Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/sqs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/sqs-examples-managing-visibility-timeout.html#sqs-examples-managing-visibility-timeout-setting)\. 
+  For API details, see [ChangeMessageVisibility](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/changemessagevisibilitycommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
Receive an Amazon SQS message and change its timeout visibility\.  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/sqs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/sqs-examples-managing-visibility-timeout.html#sqs-examples-managing-visibility-timeout-setting)\. 
+  For API details, see [ChangeMessageVisibility](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sqs-2012-11-05/ChangeMessageVisibility) in *AWS SDK for JavaScript API Reference*\. 

### Create a queue<a name="sqs_CreateQueue_javascript_topic"></a>

The following code example shows how to create an Amazon SQS queue\.

**SDK for JavaScript V3**  
Create the client\.  

```
import  { SQSClient } from "@aws-sdk/client-sqs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SQS service object.
const sqsClient = new SQSClient({ region: REGION });
export  { sqsClient };
```
Create an Amazon SQS standard queue\.  

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
Create an Amazon SQS queue that waits for a message to arrive\.  

```
// Import required AWS SDK clients and commands for Node.js
import { CreateQueueCommand } from  "@aws-sdk/client-sqs";
import { sqsClient } from  "./libs/sqsClient.js";

// Set the parameters
const params = {
  QueueName: "SQS_QUEUE_NAME", //SQS_QUEUE_URL; e.g., 'https://sqs.REGION.amazonaws.com/ACCOUNT-ID/QUEUE-NAME'
  Attributes: {
    ReceiveMessageWaitTimeSeconds: "20",
  },
};

const run = async () => {
  try {
    const data = await sqsClient.send(new CreateQueueCommand(params));
    console.log("Success", data);
    return data; // For unit tests.
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/sqs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/sqs-examples-using-queues.html#sqs-examples-using-queues-create-queue)\. 
+  For API details, see [CreateQueue](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/createqueuecommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
Create an Amazon SQS standard queue\.  

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
Create an Amazon SQS queue that waits for a message to arrive\.  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/sqs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/sqs-examples-using-queues.html#sqs-examples-using-queues-create-queue)\. 
+  For API details, see [CreateQueue](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sqs-2012-11-05/CreateQueue) in *AWS SDK for JavaScript API Reference*\. 

### Delete a message from a queue<a name="sqs_DeleteMessage_javascript_topic"></a>

The following code example shows how to delete a message from an Amazon SQS queue\.

**SDK for JavaScript V3**  
Create the client\.  

```
import  { SQSClient } from "@aws-sdk/client-sqs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SQS service object.
const sqsClient = new SQSClient({ region: REGION });
export  { sqsClient };
```
Receive and delete Amazon SQS messages\.  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/sqs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/sqs-examples-send-receive-messages.html#sqs-examples-send-receive-messages-receiving)\. 
+  For API details, see [DeleteMessage](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/deletemessagecommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
Receive and delete Amazon SQS messages\.  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/sqs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/sqs-examples-send-receive-messages.html#sqs-examples-send-receive-messages-receiving)\. 
+  For API details, see [DeleteMessage](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sqs-2012-11-05/DeleteMessage) in *AWS SDK for JavaScript API Reference*\. 

### Delete a queue<a name="sqs_DeleteQueue_javascript_topic"></a>

The following code example shows how to delete an Amazon SQS queue\.

**SDK for JavaScript V3**  
Create the client\.  

```
import  { SQSClient } from "@aws-sdk/client-sqs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SQS service object.
const sqsClient = new SQSClient({ region: REGION });
export  { sqsClient };
```
Delete an Amazon SQS queue\.  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/sqs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/sqs-examples-using-queues.html#sqs-examples-using-queues-delete-queue)\. 
+  For API details, see [DeleteQueue](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/deletequeuecommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
Delete an Amazon SQS queue\.  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/sqs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/sqs-examples-using-queues.html#sqs-examples-using-queues-delete-queue)\. 
+  For API details, see [DeleteQueue](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sqs-2012-11-05/DeleteQueue) in *AWS SDK for JavaScript API Reference*\. 

### Get the URL of a queue<a name="sqs_GetQueueUrl_javascript_topic"></a>

The following code example shows how to get the URL of an Amazon SQS queue\.

**SDK for JavaScript V3**  
Create the client\.  

```
import  { SQSClient } from "@aws-sdk/client-sqs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SQS service object.
const sqsClient = new SQSClient({ region: REGION });
export  { sqsClient };
```
Get the URL for an Amazon SQS queue\.  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/sqs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/sqs-examples-using-queues.html#sqs-examples-using-queues-get-queue-url)\. 
+  For API details, see [GetQueueUrl](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/getqueueurlcommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
Get the URL for an Amazon SQS queue\.  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/sqs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/sqs-examples-using-queues.html#sqs-examples-using-queues-get-queue-url)\. 
+  For API details, see [GetQueueUrl](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sqs-2012-11-05/GetQueueUrl) in *AWS SDK for JavaScript API Reference*\. 

### List queues<a name="sqs_ListQueues_javascript_topic"></a>

The following code example shows how to list Amazon SQS queues\.

**SDK for JavaScript V3**  
Create the client\.  

```
import  { SQSClient } from "@aws-sdk/client-sqs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SQS service object.
const sqsClient = new SQSClient({ region: REGION });
export  { sqsClient };
```
List your Amazon SQS queues\.  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/sqs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/sqs-examples-using-queues.html#sqs-examples-using-queues-listing-queues)\. 
+  For API details, see [ListQueues](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/listqueuescommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
List your Amazon SQS queues\.  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/sqs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/sqs-examples-using-queues.html#sqs-examples-using-queues-listing-queues)\. 
+  For API details, see [ListQueues](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sqs-2012-11-05/ListQueues) in *AWS SDK for JavaScript API Reference*\. 

### Receive messages from a queue<a name="sqs_ReceiveMessage_javascript_topic"></a>

The following code example shows how to receive messages from an Amazon SQS queue\.

**SDK for JavaScript V3**  
Create the client\.  

```
import  { SQSClient } from "@aws-sdk/client-sqs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SQS service object.
const sqsClient = new SQSClient({ region: REGION });
export  { sqsClient };
```
Receive a message from an Amazon SQS queue using long\-poll support\.  

```
// Import required AWS SDK clients and commands for Node.js
import { ReceiveMessageCommand } from  "@aws-sdk/client-sqs";
import { sqsClient } from  "./libs/sqsClient.js";

// Set the parameters
const queueURL = "SQS_QUEUE_URL"; // SQS_QUEUE_URL
const params = {
  AttributeNames: ["SentTimestamp"],
  MaxNumberOfMessages: 1,
  MessageAttributeNames: ["All"],
  QueueUrl: queueURL,
  WaitTimeSeconds: 20,
};

const run = async () => {
  try {
    const data = await sqsClient.send(new ReceiveMessageCommand(params));
    console.log("Success, ", data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/sqs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/sqs-examples-enable-long-polling.html#sqs-examples-enable-long-polling-on-receive-message)\. 
+  For API details, see [ReceiveMessage](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/receivemessagecommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
Receive a message from an Amazon SQS queue using long\-poll support\.  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/sqs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/sqs-examples-enable-long-polling.html#sqs-examples-enable-long-polling-on-receive-message)\. 
+  For API details, see [ReceiveMessage](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sqs-2012-11-05/ReceiveMessage) in *AWS SDK for JavaScript API Reference*\. 

### Send a message to a queue<a name="sqs_SendMessage_javascript_topic"></a>

The following code example shows how to send a message to an Amazon SQS queue\.

**SDK for JavaScript V3**  
Create the client\.  

```
import  { SQSClient } from "@aws-sdk/client-sqs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SQS service object.
const sqsClient = new SQSClient({ region: REGION });
export  { sqsClient };
```
Send a message to an Amazon SQS queue\.  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/sqs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/sqs-examples-send-receive-messages.html#sqs-examples-send-receive-messages-sending)\. 
+  For API details, see [SendMessage](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sqs/classes/sendmessagecommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
Send a message to an Amazon SQS queue\.  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create an SQS service object
var sqs = new AWS.SQS({apiVersion: '2012-11-05'});

var params = {
   // Remove DelaySeconds parameter and value for FIFO queues
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
  // MessageGroupId: "Group1",  // Required for FIFO queues
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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/sqs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/sqs-examples-send-receive-messages.html#sqs-examples-send-receive-messages-sending)\. 
+  For API details, see [SendMessage](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sqs-2012-11-05/SendMessage) in *AWS SDK for JavaScript API Reference*\. 