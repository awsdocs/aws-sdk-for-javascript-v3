--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Managing Topics in Amazon SNS<a name="sns-examples-managing-topics"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create topics in Amazon SNS to which you can publish notifications\.
+ How to delete topics created in Amazon SNS\.
+ How to get a list of available topics\.
+ How to get and set topic attributes\.

## The Scenario<a name="sns-examples-managing-topics-scenario"></a>

In this example, you use a series of Node\.js modules to create, list, and delete Amazon SNS topics, and to handle topic attributes\. The Node\.js modules use the SDK for JavaScript to manage topics using these methods of the `SNS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/createtopiccommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/createtopiccommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/listtopicscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/listtopicscommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/deletetopiccommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/deletetopiccommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/gettopicattributescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/gettopicattributescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/settopicattributescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/settopicattributescommand.html)

## Prerequisite Tasks<a name="sns-examples-managing-topics-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/sns/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Creating a Topic<a name="sns-examples-managing-topics-createtopic"></a>

In this example, use a Node\.js module to create an Amazon SNS topic\. 

Create a `libs` directory, and create a Node\.js module with the file name `snsClient.js`\. Copy and paste the code below into it, which creates the Amazon SNS client object\. Replace *REGION* with your AWS Region\.

```
import  { SNSClient } from "@aws-sdk/client-sns";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const snsClient = new SNSClient({ region: REGION });
export  { snsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/sns/src/libs/snsClient.js)\.

Create a Node\.js module with the file name `sns_createtopic.js`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the `Name` for the new topic to the `CreateTopicCommand` method of the `SNS` client class\. To call the `CreateTopicCommand` method, create an asynchronous function invoking an Amazon SNS service object, passing the parameters object\. The `data` returned contains the ARN of the topic\.

**Note**  
Replace *TOPIC\_NAME* with the name of the topic\.

```
// Import required AWS SDK clients and commands for Node.js
import {CreateTopicCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = { Name: "TOPIC_NAME" }; //TOPIC_NAME

const run = async () => {
  try {
    const data = await snsClient.send(new CreateTopicCommand(params));
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node sns_createtopic.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/sns/src/sns_createtopic.js)\.

## Listing Your Topics<a name="sns-examples-managing-topics-listtopics"></a>

In this example, use a Node\.js module to list all Amazon SNS topics\. 

Create a `libs` directory, and create a Node\.js module with the file name `snsClient.js`\. Copy and paste the code below into it, which creates the Amazon SNS client object\. Replace *REGION* with your AWS Region\.

```
import  { SNSClient } from "@aws-sdk/client-sns";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const snsClient = new SNSClient({ region: REGION });
export  { snsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/sns/src/libs/snsClient.js)\.

Create a Node\.js module with the file name `sns_listtopics.js`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an empty object to pass to the `ListTopicsCommand` method of the `SNS` client class\. To call the `ListTopicsCommand` method, create an asynchronous function invoking an Amazon SNS service object, passing the parameters object\. The `data` returned contains an array of your topic Amazon Resource Names \(ARNs\)\.

```
// Import required AWS SDK clients and commands for Node.js
import {ListTopicsCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

const run = async () => {
  try {
    const data = await snsClient.send(new ListTopicsCommand({}));
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node sns_listtopics.js 
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/sns/src/sns_listtopics.js)\.

## Deleting a Topic<a name="sns-examples-managing-topics-deletetopic"></a>

In this example, use a Node\.js module to delete an Amazon SNS topic\. 

Create a `libs` directory, and create a Node\.js module with the file name `snsClient.js`\. Copy and paste the code below into it, which creates the Amazon SNS client object\. Replace *REGION* with your AWS Region\.

```
import  { SNSClient } from "@aws-sdk/client-sns";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const snsClient = new SNSClient({ region: REGION });
export  { snsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/sns/src/libs/snsClient.js)\.

Create a Node\.js module with the file name `sns_deletetopic.js`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object containing the `TopicArn` of the topic to delete to pass to the `DeleteTopicCommand` method of the `SNS` client class\. To call the `DeleteTopicCommand` method, create an asynchronous function invoking an Amazon SNS client service object, passing the parameters object\. 

**Note**  
Replace *TOPIC\_ARN* with the Amazon Resource Name \(ARN\) of the topic you are deleting\.

```
// Load the AWS SDK for Node.js

// Import required AWS SDK clients and commands for Node.js
import {DeleteTopicCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = { TopicArn: "TOPIC_ARN" }; //TOPIC_ARN

const run = async () => {
  try {
    const data = await snsClient.send(new DeleteTopicCommand(params));
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node sns_deletetopic.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/sns/src/sns_deletetopic.js)\.

## Getting Topic Attributes<a name="sns-examples-managing-topicsgetttopicattributes"></a>

In this example, use a Node\.js module to retrieve attributes of an Amazon SNS topic\.

Create a `libs` directory, and create a Node\.js module with the file name `snsClient.js`\. Copy and paste the code below into it, which creates the Amazon SNS client object\. Replace *REGION* with your AWS Region\.

```
import  { SNSClient } from "@aws-sdk/client-sns";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const snsClient = new SNSClient({ region: REGION });
export  { snsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/sns/src/libs/snsClient.js)\.

Create a Node\.js module with the file name `sns_gettopicattributes.js`\. Configure the SDK as previously shown\.

Create an object containing the `TopicArn` of a topic to delete to pass to the `GetTopicAttributesCommand` method of the `SNS` client class\. To call the `GetTopicAttributesCommand` method, invoking an Amazon SNS client service object, passing the parameters object\. 

**Note**  
Replace *TOPIC\_ARN* with the ARN of the topic\.

```
// Import required AWS SDK clients and commands for Node.js
import {GetTopicAttributesCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = { TopicArn: "TOPIC_ARN" }; // TOPIC_ARN

const run = async () => {
  try {
    const data = await snsClient.send(new GetTopicAttributesCommand(params));
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node sns_gettopicattributes.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/sns/src/sns_gettopicattributes.js)\.

## Setting Topic Attributes<a name="sns-examples-managing-topicsstttopicattributes"></a>

In this example, use a Node\.js module to set the mutable attributes of an Amazon SNS topic\. 

Create a `libs` directory, and create a Node\.js module with the file name `snsClient.js`\. Copy and paste the code below into it, which creates the Amazon SNS client object\. Replace *REGION* with your AWS Region\.

```
import  { SNSClient } from "@aws-sdk/client-sns";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const snsClient = new SNSClient({ region: REGION });
export  { snsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/sns/src/libs/snsClient.js)\.

Create a Node\.js module with the file name `sns_settopicattributes.js`\. Configure the SDK as previously shown\.

Create an object containing the parameters for the attribute update, including the `TopicArn` of the topic whose attributes you want to set, the name of the attribute to set, and the new value for that attribute\. You can set only the `Policy`, `DisplayName`, and `DeliveryPolicy` attributes\. Pass the parameters to the `SetTopicAttributesCommand` method of the `SNS` client class\. To call the `SetTopicAttributesCommand` method, create an asynchronous function invoking an Amazon SNS client service object, passing the parameters object\. 

**Note**  
Replace *ATTRIBUTE\_NAME* with the name of the attribute you are setting, *TOPIC\_ARN* with the Amazon Resource Name \(ARN\) of the topic whose attributes you want to set, and *NEW\_ATTRIBUTE\_VALUE* with the new value for that attribute\.

```
// Import required AWS SDK clients and commands for Node.js
import {SetTopicAttributesCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = {
  AttributeName: "ATTRIBUTE_NAME", // ATTRIBUTE_NAME
  TopicArn: "TOPIC_ARN", // TOPIC_ARN
  AttributeValue: "NEW_ATTRIBUTE_VALUE", //NEW_ATTRIBUTE_VALUE
};

const run = async () => {
  try {
    const data = await snsClient.send(new SetTopicAttributesCommand(params));
    console.log("Success.",  data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node sns_settopicattributes.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/sns/src/sns_settopicattributes.js)\.