# Publishing Messages in Amazon SNS<a name="sns-examples-publishing-messages"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to publish messages to an Amazon SNS topic\.

## The Scenario<a name="sns-examples-publishing-messages-scenario"></a>

In this example, you use a series of Node\.js modules to publish messages from Amazon SNS to topic endpoints, emails, or phone numbers\. The Node\.js modules use the SDK for JavaScript to send messages using this method of the `AWS.SNS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#publish-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#publish-property)

## Prerequisite Tasks<a name="sns-examples-publishing-messages-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](http://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Publishing a Message to an SNS Topic<a name="sns-examples-publishing-text-messages"></a>

In this example, use a Node\.js module to publish a message to an Amazon SNS topic\. Create a Node\.js module with the file name `sns_publishtotopic.js`\. Configure the SDK as previously shown\.

Create an object containing the parameters for publishing a message, including the message text and the ARN of the SNS topic\. For details on available SMS attributes, see [SetSMSAttributes](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#setSMSAttributes-property)\.

Pass the parameters to the `publish` method of the `AWS.SNS` client class\. Create a promise for invoking an Amazon SNS service object, passing the parameters object\. Then handle the response in the promise callback\. 

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set region
AWS.config.update({region: 'REGION'});

// Create publish parameters
var params = {
  Message: 'MESSAGE_TEXT', /* required */
  TopicArn: 'TOPIC_ARN'
};

// Create promise and SNS service object
var publishTextPromise = new AWS.SNS({apiVersion: '2010-03-31'}).publish(params).promise();

// Handle promise's fulfilled/rejected states
publishTextPromise.then(
  function(data) {
    console.log(`Message ${params.Message} send sent to the topic ${params.TopicArn}`);
    console.log("MessageID is " + data.MessageId);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\.

```
node sns_publishtotopic.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sns/sns_publishtotopic.js)\.