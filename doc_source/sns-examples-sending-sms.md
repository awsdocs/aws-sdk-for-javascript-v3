# Sending SMS Messages with Amazon SNS<a name="sns-examples-sending-sms"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to get and set SMS messaging preferences for Amazon SNS\.
+ How to check a phone number to see if it has opted out of receiving SMS messages\.
+ How to get a list of phone numbers that have opted out of receiving SMS messages\.
+ How to send an SMS message\.

## The Scenario<a name="sns-examples-sending-sms-scenario"></a>

You can use Amazon SNS to send text messages, or SMS messages, to SMS\-enabled devices\. You can send a message directly to a phone number, or you can send a message to multiple phone numbers at once by subscribing those phone numbers to a topic and sending your message to the topic\.

In this example, you use a series of Node\.js modules to publish SMS text messages from Amazon SNS to SMS\-enabled devices\. The Node\.js modules use the SDK for JavaScript to publish SMS messages using these methods of the `AWS.SNS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#getSMSAttributes-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#getSMSAttributes-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#setSMSAttributes-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#setSMSAttributes-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#checkIfPhoneNumberIsOptedOut-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#checkIfPhoneNumberIsOptedOut-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#listPhoneNumbersOptedOut-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#listPhoneNumbersOptedOut-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#publish-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html#publish-property)

## Prerequisite Tasks<a name="sns-examples-sending-sms-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](http://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Getting SMS Attributes<a name="sending-sms-getattributes"></a>

Use Amazon SNS to specify preferences for SMS messaging, such as how your deliveries are optimized \(for cost or for reliable delivery\), your monthly spending limit, how message deliveries are logged, and whether to subscribe to daily SMS usage reports\. These preferences are retrieved and set as SMS attributes for Amazon SNS\.

In this example, use a Node\.js module to get the current SMS attributes in Amazon SNS\. Create a Node\.js module with the file name `sns_getsmstype.js`\. Configure the SDK as previously shown\. Create an object containing the parameters for getting SMS attributes, including the names of the individual attributes to get\. For details on available SMS attributes, see [SetSMSAttributes](https://docs.aws.amazon.com/sns/latest/api/API_SetSMSAttributes.html) in the Amazon Simple Notification Service API Reference\.

This example gets the `DefaultSMSType` attribute, which controls whether SMS messages are sent as `Promotional`, which optimizes message delivery to incur the lowest cost, or as `Transactional`, which optimizes message delivery to achieve the highest reliability\. Pass the parameters to the `setTopicAttributes` method of the `AWS.SNS` client class\. To call the `getSMSAttributes` method, create a promise for invoking an Amazon SNS service object, passing the parameters object\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set region
AWS.config.update({region: 'REGION'});

// Create SMS Attribute parameter you want to get
var params = {
  attributes: [
    'DefaultSMSType',
    'ATTRIBUTE_NAME'
    /* more items */
  ]
};

// Create promise and SNS service object
var getSMSTypePromise = new AWS.SNS({apiVersion: '2010-03-31'}).getSMSAttributes(params).promise();

// Handle promise's fulfilled/rejected states
getSMSTypePromise.then(
  function(data) {
    console.log(data);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\.

```
node sns_getsmstype.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sns/sns_getsmstype.js)\.

## Setting SMS Attributes<a name="sending-sms-setattributes"></a>

In this example, use a Node\.js module to get the current SMS attributes in Amazon SNS\. Create a Node\.js module with the file name `sns_setsmstype.js`\. Configure the SDK as previously shown\. Create an object containing the parameters for setting SMS attributes, including the names of the individual attributes to set and the values to set for each\. For details on available SMS attributes, see [ SetSMSAttributes](https://docs.aws.amazon.com/sns/latest/api/API_SetSMSAttributes.html) in the Amazon Simple Notification Service API Reference\.

This example sets the `DefaultSMSType` attribute to `Transactional`, which optimizes message delivery to achieve the highest reliability\. Pass the parameters to the `setTopicAttributes` method of the `AWS.SNS` client class\. To call the `getSMSAttributes` method, create a promise for invoking an Amazon SNS service object, passing the parameters object\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set region
AWS.config.update({region: 'REGION'});

// Create SMS Attribute parameters
var params = {
  attributes: { /* required */
    'DefaultSMSType': 'Transactional', /* highest reliability */
    //'DefaultSMSType': 'Promotional' /* lowest cost */
  }
};

// Create promise and SNS service object
var setSMSTypePromise = new AWS.SNS({apiVersion: '2010-03-31'}).setSMSAttributes(params).promise();

// Handle promise's fulfilled/rejected states
setSMSTypePromise.then(
  function(data) {
    console.log(data);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\.

```
node sns_setsmstype.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sns/sns_setsmstype.js)\.

## Checking If a Phone Number Has Opted Out<a name="sending-sms-checkifphonenumberisoptedout"></a>

In this example, use a Node\.js module to check a phone number to see if it has opted out from receiving SMS messages\. Create a Node\.js module with the file name `sns_checkphoneoptout.js`\. Configure the SDK as previously shown\. Create an object containing the phone number to check as a parameter\.

This example sets the `PhoneNumber` parameter to specify the phone number to check\. Pass the object to the `checkIfPhoneNumberIsOptedOut` method of the `AWS.SNS` client class\. To call the `checkIfPhoneNumberIsOptedOut` method, create a promise for invoking an Amazon SNS service object, passing the parameters object\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set region
AWS.config.update({region: 'REGION'});

// Create promise and SNS service object
var phonenumPromise = new AWS.SNS({apiVersion: '2010-03-31'}).checkIfPhoneNumberIsOptedOut({phoneNumber: 'PHONE_NUMBER'}).promise();

// Handle promise's fulfilled/rejected states
phonenumPromise.then(
  function(data) {
    console.log("Phone Opt Out is " + data.isOptedOut);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\.

```
node sns_checkphoneoptout.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sns/sns_checkphoneoptout.js)\.

## Listing Opted\-Out Phone Numbers<a name="sending-sms-listphonenumbersoptedout"></a>

In this example, use a Node\.js module to get a list of phone numbers that have opted out from receiving SMS messages\. Create a Node\.js module with the file name `sns_listnumbersoptedout.js`\. Configure the SDK as previously shown\. Create an empty object as a parameter\.

Pass the object to the `listPhoneNumbersOptedOut` method of the `AWS.SNS` client class\. To call the `listPhoneNumbersOptedOut` method, create a promise for invoking an Amazon SNS service object, passing the parameters object\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set region
AWS.config.update({region: 'REGION'});

// Create promise and SNS service object
var phonelistPromise = new AWS.SNS({apiVersion: '2010-03-31'}).listPhoneNumbersOptedOut({}).promise();

// Handle promise's fulfilled/rejected states
  phonelistPromise.then(
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
node sns_listnumbersoptedout.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sns/sns_listnumbersoptedout.js)\.

## Publishing an SMS Message<a name="sending-sms-publishsms"></a>

In this example, use a Node\.js module to send an SMS message to a phone number\. Create a Node\.js module with the file name `sns_publishsms.js`\. Configure the SDK as previously shown\. Create an object containing the `Message` and `PhoneNumber` parameters\.

When you send an SMS message, specify the phone number using the E\.164 format\. E\.164 is a standard for the phone number structure used for international telecommunication\. Phone numbers that follow this format can have a maximum of 15 digits, and they are prefixed with the plus character \(\+\) and the country code\. For example, a US phone number in E\.164 format would appear as \+1001XXX5550100\. 

This example sets the `PhoneNumber` parameter to specify the phone number to send the message\. Pass the object to the `publish` method of the `AWS.SNS` client class\. To call the `publish` method, create a promise for invoking an Amazon SNS service object, passing the parameters object\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set region
AWS.config.update({region: 'REGION'});

// Create publish parameters
var params = {
  Message: 'TEXT_MESSAGE', /* required */
  PhoneNumber: 'E.164_PHONE_NUMBER',
};

// Create promise and SNS service object
var publishTextPromise = new AWS.SNS({apiVersion: '2010-03-31'}).publish(params).promise();

// Handle promise's fulfilled/rejected states
publishTextPromise.then(
  function(data) {
    console.log("MessageID is " + data.MessageId);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\.

```
node sns_publishsms.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/sns/sns_publishsms.js)\.