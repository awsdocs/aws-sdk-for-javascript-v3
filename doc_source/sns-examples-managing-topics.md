# Managing Topics in Amazon SNS<a name="sns-examples-managing-topics"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create topics in Amazon SNS to which you can publish notifications\.
+ How to delete topics created in Amazon SNS\.
+ How to get a list of available topics\.
+ How to get and set topic attributes\.

## The Scenario<a name="sns-examples-managing-topics-scenario"></a>

In this example, you use a series of Node\.js modules to create, list, and delete Amazon SNS topics, and to handle topic attributes\. The Node\.js modules use the SDK for JavaScript to manage topics using these methods of the `AWS.SNS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#createTopic-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#createTopic-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#listTopics-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#listTopics-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#deleteTopic-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#deleteTopic-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#getTopicAttributes-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#getTopicAttributes-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#setTopicAttributes-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#setTopicAttributes-property)

## Prerequisite Tasks<a name="sns-examples-managing-topics-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](http://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Creating a Topic<a name="sns-examples-managing-topics-createtopic"></a>

In this example, use a Node\.js module to create an Amazon SNS topic\. Create a Node\.js module with the file name `sns_createtopic.js`\. Configure the SDK as previously shown\.

Create an object to pass the `Name` for the new topic to the `createTopic` method of the `AWS.SNS` client class\. To call the `createTopic` method, create a promise for invoking an Amazon SNS service object, passing the parameters object\. Then handle the `response` in the promise callback\. The `data` returned by the promise contains the ARN of the topic\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set region
AWS.config.update({region: 'REGION'});

// Create promise and SNS service object
var createTopicPromise = new AWS.SNS({apiVersion: '2010-03-31'}).createTopic({Name: "TOPIC_NAME"}).promise();

// Handle promise's fulfilled/rejected states
createTopicPromise.then(
  function(data) {
    console.log("Topic ARN is " + data.TopicArn);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\.

```
node sns_createtopic.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sns/sns_createtopic.js)\.

## Listing Your Topics<a name="sns-examples-managing-topics-listtopics"></a>

In this example, use a Node\.js module to list all Amazon SNS topics\. Create a Node\.js module with the file name `sns_listtopics.js`\. Configure the SDK as previously shown\.

Create an empty object to pass to the `listTopics` method of the `AWS.SNS` client class\. To call the `listTopics` method, create a promise for invoking an Amazon SNS service object, passing the parameters object\. Then handle the `response` in the promise callback\. The `data` returned by the promise contains an array of your topic ARNs\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set region
AWS.config.update({region: 'REGION'});

// Create promise and SNS service object
var listTopicsPromise = new AWS.SNS({apiVersion: '2010-03-31'}).listTopics({}).promise();

// Handle promise's fulfilled/rejected states
listTopicsPromise.then(
  function(data) {
    console.log(data.Topics);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\.

```
node sns_listtopics.js
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sns/sns_listtopics.js)\.

## Deleting a Topic<a name="sns-examples-managing-topics-deletetopic"></a>

In this example, use a Node\.js module to delete an Amazon SNS topic\. Create a Node\.js module with the file name `sns_deletetopic.js`\. Configure the SDK as previously shown\.

Create an object containing the `TopicArn` of the topic to delete to pass to the `deleteTopic` method of the `AWS.SNS` client class\. To call the `deleteTopic` method, create a promise for invoking an Amazon SNS service object, passing the parameters object\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set region
AWS.config.update({region: 'REGION'});

// Create promise and SNS service object
var deleteTopicPromise = new AWS.SNS({apiVersion: '2010-03-31'}).deleteTopic({TopicArn: 'TOPIC_ARN'}).promise();

// Handle promise's fulfilled/rejected states
deleteTopicPromise.then(
  function(data) {
    console.log("Topic Deleted");
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\.

```
node sns_deletetopic.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sns/sns_deletetopic.js)\.

## Getting Topic Attributes<a name="sns-examples-managing-topicsgetttopicattributes"></a>

In this example, use a Node\.js module to retrieve attributes of an Amazon SNS topic\. Create a Node\.js module with the file name `sns_gettopicattributes.js`\. Configure the SDK as previously shown\.

Create an object containing the `TopicArn` of a topic to delete to pass to the `getTopicAttributes` method of the `AWS.SNS` client class\. To call the `getTopicAttributes` method, create a promise for invoking an Amazon SNS service object, passing the parameters object\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set region
AWS.config.update({region: 'REGION'});


// Create promise and SNS service object
var getTopicAttribsPromise = new AWS.SNS({apiVersion: '2010-03-31'}).getTopicAttributes({TopicArn: 'TOPIC_ARN'}).promise();

// Handle promise's fulfilled/rejected states
getTopicAttribsPromise.then(
  function(data) {
    console.log(data);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\.

```
node sns_gettopicattributes.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sns/sns_gettopicattributes.js)\.

## Setting Topic Attributes<a name="sns-examples-managing-topicsstttopicattributes"></a>

In this example, use a Node\.js module to set the mutable attributes of an Amazon SNS topic\. Create a Node\.js module with the file name `sns_settopicattributes.js`\. Configure the SDK as previously shown\.

Create an object containing the parameters for the attribute update, including the `TopicArn` of the topic whose attributes you want to set, the name of the attribute to set, and the new value for that attribute\. You can set only the `Policy`, `DisplayName`, and `DeliveryPolicy` attributes\. Pass the parameters to the `setTopicAttributes` method of the `AWS.SNS` client class\. To call the `setTopicAttributes` method, create a promise for invoking an Amazon SNS service object, passing the parameters object\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set region
AWS.config.update({region: 'REGION'});

// Create setTopicAttributes parameters
var params = {
  AttributeName: 'ATTRIBUTE_NAME', /* required */
  TopicArn: 'TOPIC_ARN', /* required */
  AttributeValue: 'NEW_ATTRIBUTE_VALUE'
};

// Create promise and SNS service object
var setTopicAttribsPromise = new AWS.SNS({apiVersion: '2010-03-31'}).setTopicAttributes(params).promise();

// Handle promise's fulfilled/rejected states
setTopicAttribsPromise.then(
  function(data) {
    console.log(data);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\.

```
node sns_settopicattributes.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sns/sns_settopicattributes.js)\.