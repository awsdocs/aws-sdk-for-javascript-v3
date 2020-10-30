--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

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
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#createTopic-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#createTopic-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#listTopics-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#listTopics-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#deleteTopic-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#deleteTopic-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#getTopicAttributes-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#getTopicAttributes-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#setTopicAttributes-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#setTopicAttributes-property)

## Prerequisite Tasks<a name="sns-examples-managing-topics-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up a project environment to run Node TypeScript examples\. For more information, see[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/sns/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Creating a Topic<a name="sns-examples-managing-topics-createtopic"></a>

In this example, use a Node\.js module to create an Amazon SNS topic\. Create a Node\.js module with the file name `sns_createtopic.ts`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the `Name` for the new topic to the `CreateTopicCommand` method of the `SNS` client class\. To call the `CreateTopicCommand` method, create an asynchronous function invoking an Amazon SNS service object, passing the parameters object\. The `data` returned contains the ARN of the topic\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *TOPIC\_NAME* with the name of the topic\.

```
// Import required AWS SDK clients and commands for Node.js
const { SNSClient, CreateTopicCommand } = require("@aws-sdk/client-sns");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { Name: "TOPIC_NAME" }; //TOPIC_NAME

// Create SNS service object
const sns = new SNSClient(REGION);

const run = async () => {
  try {
    const data = await sns.send(new CreateTopicCommand(params));
    console.log("Topic ARN is " + data.TopicArn);
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node sns_createtopic.ts // If you prefer JavaScript, enter 'node sns_createtopic.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/sns_createtopic.ts)\.

## Listing Your Topics<a name="sns-examples-managing-topics-listtopics"></a>

In this example, use a Node\.js module to list all Amazon SNS topics\. Create a Node\.js module with the file name `sns_listtopics.ts`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an empty object to pass to the `ListTopicsCommand` method of the `SNS` client class\. To call the `ListTopicsCommand` method, create an asynchronous function invoking an Amazon SNS service object, passing the parameters object\. The `data` returned contains an array of your topic Amazon Resource Names \(ARNs\)\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region\.

```
// Import required AWS SDK clients and commands for Node.js
const { SNSClient, ListTopicsCommand } = require("@aws-sdk/client-sns");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Create SNS service object
const sns = new SNSClient(REGION);

const run = async () => {
  try {
    const data = await sns.send(new ListTopicsCommand({}));
    console.log(data.Topics);
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node sns_listtopics.ts // If you prefer JavaScript, enter 'node sns_listtopics.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/sns_listtopics.ts)\.

## Deleting a Topic<a name="sns-examples-managing-topics-deletetopic"></a>

In this example, use a Node\.js module to delete an Amazon SNS topic\. Create a Node\.js module with the file name `sns_deletetopic.ts`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object containing the `TopicArn` of the topic to delete to pass to the `DeleteTopicCommand` method of the `SNS` client class\. To call the `DeleteTopicCommand` method, create an asynchronous function invoking an Amazon SNS client service object, passing the parameters object\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *TOPIC\_ARN* with the Amazon Resource Name \(ARN\) of the topic you are deleting\.

```
// Load the AWS SDK for Node.js

// Import required AWS SDK clients and commands for Node.js
const { SNSClient, DeleteTopicCommand } = require("@aws-sdk/client-sns");

// Create SNS service object
const sns = new SNSClient(REGION);

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { TopicArn: "TOPIC_ARN" }; //TOPIC_ARN

const run = async () => {
  try {
    const data = await sns.send(new DeleteTopicCommand(params));
    console.log("Topic Deleted");
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node sns_deletetopic.ts // If you prefer JavaScript, enter 'node sns_deletetopic.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/sns_deletetopic.ts)\.

## Getting Topic Attributes<a name="sns-examples-managing-topicsgetttopicattributes"></a>

In this example, use a Node\.js module to retrieve attributes of an Amazon SNS topic\. Create a Node\.js module with the file name `sns_gettopicattributes.ts`\. Configure the SDK as previously shown\.

Create an object containing the `TopicArn` of a topic to delete to pass to the `GetTopicAttributesCommand` method of the `SNS` client class\. To call the `GetTopicAttributesCommand` method, invoking an Amazon SNS client service object, passing the parameters object\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *TOPIC\_ARN* with the ARN of the topic\.

```
// Import required AWS SDK clients and commands for Node.js
const { SNSClient, GetTopicAttributesCommand } = require("@aws-sdk/client-sns");

// Set the AWS Region
const REGION = "region"; //e.g. "us-east-1"

// Set the parameters
const params = { TopicArn: "TOPIC_ARN" }; // TOPIC_ARN

// Create SNS service object
const sns = new SNSClient(REGION);

const run = async () => {
  try {
    const data = await sns.send(new GetTopicAttributesCommand(params));
    console.log("Success. Attributes:", data.Attributes);
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node sns_gettopicattributes.ts // If you prefer JavaScript, enter 'node sns_gettopicattributes.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/sns_gettopicattributes.ts)\.

## Setting Topic Attributes<a name="sns-examples-managing-topicsstttopicattributes"></a>

In this example, use a Node\.js module to set the mutable attributes of an Amazon SNS topic\. Create a Node\.js module with the file name `sns_settopicattributes.ts`\. Configure the SDK as previously shown\.

Create an object containing the parameters for the attribute update, including the `TopicArn` of the topic whose attributes you want to set, the name of the attribute to set, and the new value for that attribute\. You can set only the `Policy`, `DisplayName`, and `DeliveryPolicy` attributes\. Pass the parameters to the `SetTopicAttributesCommand` method of the `SNS` client class\. To call the `SetTopicAttributesCommand` method, create an asynchronous function invoking an Amazon SNS cleint service object, passing the parameters object\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *ATTRIBUTE\_NAME* with the name of the attribute you are setting, *TOPIC\_ARN* with the Amazon Resource Name \(ARN\) of the topic whose attributes you want to set, and *NEW\_ATTRIBUTE\_VALUE* with the new value for that attribute\.

```
// Import required AWS SDK clients and commands for Node.js
const { SNSClient, SetTopicAttributesCommand } = require("@aws-sdk/client-sns");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  AttributeName: "ATTRIBUTE_NAME", // ATTRIBUTE_NAME
  TopicArn: "TOPIC_ARN", // TOPIC_ARN
  AttributeValue: "NEW_ATTRIBUTE_VALUE", //NEW_ATTRIBUTE_VALUE
};

// Create SNS service object
const sns = new SNSClient(REGION);

const run = async () => {
  try {
    const data = await sns.send(new SetTopicAttributesCommand(params));
    console.log("Success, attributed updated", data);
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node sns_settopicattributes.ts // If you prefer JavaScript, enter 'node sns_settopicattributes.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/sns_settopicattributes.ts)\.