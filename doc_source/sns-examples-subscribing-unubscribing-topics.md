# Managing Subscriptions in Amazon SNS<a name="sns-examples-subscribing-unubscribing-topics"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to list all subscriptions to an Amazon SNS topic\.
+ How to subscribe an email address, an application endpoint, or an AWS Lambda function to an Amazon SNS topic\.
+ How to unsubscribe from Amazon SNS topics\.

## The Scenario<a name="sns-examples-subscribing-unubscribing-yopics-scenario"></a>

In this example, you use a series of Node\.js modules to publish notification messages to Amazon SNS topics\. The Node\.js modules use the SDK for JavaScript to manage topics using these methods of the `AWS.SNS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#subscribe-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#subscribe-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#confirmSubscription-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#confirmSubscription-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#listSubscriptionsByTopic-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#listSubscriptionsByTopic-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#unsubscribe-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#unsubscribe-property)

## Prerequisite Tasks<a name="sns-examples-subscribing-unubscribing-topics-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](http://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Listing Subscriptions to a Topic<a name="sns-examples-list-subscriptions-email"></a>

In this example, use a Node\.js module to list all subscriptions to an Amazon SNS topic\. Create a Node\.js module with the file name `sns_listsubscriptions.js`\. Configure the SDK as previously shown\.

Create an object containing the `TopicArn` parameter for the topic whose subscriptions you want to list\. Pass the parameters to the `listSubscriptionsByTopic` method of the `AWS.SNS` client class\. To call the `listSubscriptionsByTopic` method, create a promise for invoking an Amazon SNS service object, passing the parameters object\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set region
AWS.config.update({region: 'REGION'});

const params = {
  TopicArn : 'TOPIC_ARN'
}

// Create promise and SNS service object
var subslistPromise = new AWS.SNS({apiVersion: '2010-03-31'}).listSubscriptionsByTopic(params).promise();

// Handle promise's fulfilled/rejected states
  subslistPromise.then(
    function(data) {
      console.log(data);
    }).catch(
    function(err) {
      console.error(err, err.stack);
    }
  );
```

To run the example, type the following at the command line\.

```
node sns_listsubscriptions.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sns/sns_listsubscriptions.js)\.

## Subscribing an Email Address to a Topic<a name="sns-examples-subscribing-email"></a>

In this example, use a Node\.js module to subscribe an email address so that it receives SMTP email messages from an Amazon SNS topic\. Create a Node\.js module with the file name `sns_subscribeemail.js`\. Configure the SDK as previously shown\.

Create an object containing the `Protocol` parameter to specify the `email` protocol, the `TopicArn` for the topic to subscribe to, and an email address as the message `Endpoint`\. Pass the parameters to the `subscribe` method of the `AWS.SNS` client class\. You can use the `subscribe` method to subscribe several different endpoints to an Amazon SNS topic, depending on the values used for parameters passed, as other examples in this topic will show\.

To call the `subscribe` method, create a promise for invoking an Amazon SNS service object, passing the parameters object\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set region
AWS.config.update({region: 'REGION'});

// Create subscribe/email parameters
var params = {
  Protocol: 'EMAIL', /* required */
  TopicArn: 'TOPIC_ARN', /* required */
  Endpoint: 'EMAIL_ADDRESS'
};

// Create promise and SNS service object
var subscribePromise = new AWS.SNS({apiVersion: '2010-03-31'}).subscribe(params).promise();

// Handle promise's fulfilled/rejected states
subscribePromise.then(
  function(data) {
    console.log("Subscription ARN is " + data.SubscriptionArn);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\.

```
node sns_subscribeemail.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sns/sns_subscribeemail.js)\.

## Subscribing an Application Endpoint to a Topic<a name="sns-examples-subscribing-apps"></a>

In this example, use a Node\.js module to subscribe a mobile application endpoint so it receives notifications from an Amazon SNS topic\. Create a Node\.js module with the file name `sns_subscribeapp.js`\. Configure the SDK as previously shown\.

Create an object containing the `Protocol` parameter to specify the `application` protocol, the `TopicArn` for the topic to subscribe to, and the ARN of a mobile application endpoint for the `Endpoint` parameter\. Pass the parameters to the `subscribe` method of the `AWS.SNS` client class\.

To call the `subscribe` method, create a promise for invoking an Amazon SNS service object, passing the parameters object\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set region
AWS.config.update({region: 'REGION'});

// Create subscribe/email parameters
var params = {
  Protocol: 'application', /* required */
  TopicArn: 'TOPIC_ARN', /* required */
  Endpoint: 'MOBILE_ENDPOINT_ARN'
};

// Create promise and SNS service object
var subscribePromise = new AWS.SNS({apiVersion: '2010-03-31'}).subscribe(params).promise();

// Handle promise's fulfilled/rejected states
subscribePromise.then(
  function(data) {
    console.log("Subscription ARN is " + data.SubscriptionArn);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\.

```
node sns_subscribeapp.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sns/sns_subscribeapp.js)\.

## Subscribing a Lambda Function to a Topic<a name="sns-examples-subscribing-lambda"></a>

In this example, use a Node\.js module to subscribe an AWS Lambda function so it receives notifications from an Amazon SNS topic\. Create a Node\.js module with the file name `sns_subscribelambda.js`\. Configure the SDK as previously shown\.

Create an object containing the `Protocol` parameter, specifying the `lambda` protocol, the `TopicArn` for the topic to subscribe to, and the ARN of an AWS Lambda function as the `Endpoint` parameter\. Pass the parameters to the `subscribe` method of the `AWS.SNS` client class\.

To call the `subscribe` method, create a promise for invoking an Amazon SNS service object, passing the parameters object\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set region
AWS.config.update({region: 'REGION'});

// Create subscribe/email parameters
var params = {
  Protocol: 'lambda', /* required */
  TopicArn: 'TOPIC_ARN', /* required */
  Endpoint: 'LAMBDA_FUNCTION_ARN'
};

// Create promise and SNS service object
var subscribePromise = new AWS.SNS({apiVersion: '2010-03-31'}).subscribe(params).promise();

// Handle promise's fulfilled/rejected states
subscribePromise.then(
  function(data) {
    console.log("Subscription ARN is " + data.SubscriptionArn);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\.

```
node sns_subscribelambda.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sns/sns_subscribelambda.js)\.

## Unsubscribing from a Topic<a name="sns-examples-unsubscribing"></a>

In this example, use a Node\.js module to unsubscribe an Amazon SNS topic subscription\. Create a Node\.js module with the file name `sns_unsubscribe.js`\. Configure the SDK as previously shown\.

Create an object containing the `SubscriptionArn` parameter, specifying the ARN of the subscription to unsubscribe\. Pass the parameters to the `unsubscribe` method of the `AWS.SNS` client class\.

To call the `unsubscribe` method, create a promise for invoking an Amazon SNS service object, passing the parameters object\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set region
AWS.config.update({region: 'REGION'});                

// Create promise and SNS service object
var subscribePromise = new AWS.SNS({apiVersion: '2010-03-31'}).unsubscribe({SubscriptionArn : TOPIC_SUBSCRIPTION_ARN}).promise();

// Handle promise's fulfilled/rejected states
subscribePromise.then(
  function(data) {
    console.log(data);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\.

```
node sns_unsubscribe.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sns/sns_unsubscribe.js)\.