--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Managing Subscriptions in Amazon SNS<a name="sns-examples-subscribing-unsubscribing-topics"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to list all subscriptions to an Amazon SNS topic\.
+ How to subscribe an email address, an application endpoint, or an AWS Lambda function to an Amazon SNS topic\.
+ How to unsubscribe from Amazon SNS topics\.

## The Scenario<a name="sns-examples-subscribing-unsubscribing-yopics-scenario"></a>

In this example, you use a series of Node\.js modules to publish notification messages to Amazon SNS topics\. The Node\.js modules use the SDK for JavaScript to manage topics using these methods of the `SNS` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/listsubscriptionsbytopiccommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/listsubscriptionsbytopiccommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/subscribecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/subscribecommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/confirmsubscriptioncommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/confirmsubscriptioncommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/unsubscribecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/unsubscribecommand.html)

## Prerequisite Tasks<a name="sns-examples-subscribing-unsubscribing-topics-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/sns/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Listing Subscriptions to a Topic<a name="sns-examples-list-subscriptions-email"></a>

In this example, use a Node\.js module to list all subscriptions to an Amazon SNS topic\. 

Create a `libs` directory, and create a Node\.js module with the file name `snsClient.js`\. Copy and paste the code below into it, which creates the Amazon SNS client object\. Replace *REGION* with your AWS Region\.

```
import  { SNSClient } from "@aws-sdk/client-sns";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const snsClient = new SNSClient({ region: REGION });
export  { snsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/libs/snsClient.js)\.

 Create a Node\.js module with the file name `sns_listsubscriptions.js`\. Configure the SDK as previously shown\.

Create an object containing the `TopicArn` parameter for the topic whose subscriptions you want to list\. Pass the parameters to the `ListSubscriptionsByTopicCommand` method of the `SNS` client class\. To call the `ListSubscriptionsByTopicCommand` method, create an asynchronous function invoking an Amazon SNS client service object, and passing the parameters object\. 

**Note**  
Replace *TOPIC\_ARN* with the Amazon Resource Name \(ARN\) for the topic whose subscriptions you want to list \.

```
// Import required AWS SDK clients and commands for Node.js
import {ListSubscriptionsByTopicCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = { TopicArn: "TOPIC_ARN" }; //TOPIC_ARN

const run = async () => {
  try {
    const data = await snsClient.send(new ListSubscriptionsByTopicCommand(params));
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
node sns_listsubscriptions.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/sns_listsubscriptions.js)\.

## Subscribing an Email Address to a Topic<a name="sns-examples-subscribing-email"></a>

In this example, use a Node\.js module to subscribe an email address so that it receives SMTP email messages from an Amazon SNS topic\. 

Create a `libs` directory, and create a Node\.js module with the file name `snsClient.js`\. Copy and paste the code below into it, which creates the Amazon SNS client object\. Replace *REGION* with your AWS Region\.

```
import  { SNSClient } from "@aws-sdk/client-sns";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const snsClient = new SNSClient({ region: REGION });
export  { snsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/libs/snsClient.js)\.

Create a Node\.js module with the file name `sns_subscribeemail.js`\. Configure the SDK as previously shown\.

Create an object containing the `Protocol` parameter to specify the `email` protocol, the `TopicArn` for the topic to subscribe to, and an email address as the message `Endpoint`\. Pass the parameters to the `SubscribeCommand` method of the `SNS` client class\. You can use the `subscribe` method to subscribe several different endpoints to an Amazon SNS topic, depending on the values used for parameters passed, as other examples in this topic will show\.

To call the `SubscribeCommand` method, create an asynchronous function invoking an Amazon SNS client service object, and passing the parameters object\. 

**Note**  
Replace *TOPIC\_ARN* with the Amazon Resource Name \(ARN\) for the topic, and *EMAIL\_ADDRESS* with the email address to subscribe to\.

```
// Import required AWS SDK clients and commands for Node.js
import {SubscribeCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = {
  Protocol: "email" /* required */,
  TopicArn: "TOPIC_ARN", //TOPIC_ARN
  Endpoint: "EMAIL_ADDRESS", //EMAIL_ADDRESS
};

const run = async () => {
  try {
    const data = await snsClient.send(new SubscribeCommand(params));
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
node sns_subscribeemail.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/sns_subscribeemail.js)\.

### Confirming Subscriptions<a name="sns-confirm-subscription-email"></a>

In this example, use a Node\.js module to verify an endpoint owner's intent to receive emails by validating the token sent to the endpoint by a previous subscribe action\.

Create a `libs` directory, and create a Node\.js module with the file name `snsClient.js`\. Copy and paste the code below into it, which creates the Amazon SNS client object\. Replace *REGION* with your AWS Region\.

```
import  { SNSClient } from "@aws-sdk/client-sns";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const snsClient = new SNSClient({ region: REGION });
export  { snsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/libs/snsClient.js)\.

Create a Node\.js module with the file name `sns_confirmsubscription.js`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Define the parameters, including the `TOPIC_ARN` and `TOKEN`, and define a value of `TRUE` or `FALSE` for `AuthenticateOnUnsubscribe`\. If set to `TRUE` the `Confirm Subscription` action requires an AWS signature\. 

The token is a short\-lived token sent to the owner of an endpoint during a previous `SUBSCRIBE` action\. For example, for an email endpoint the `TOKEN` is in the URL of the Confirm Subscription email sent to the email owner\. For example, `abc123` is the token in the following URL\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

To call the `ConfirmSubscriptionCommand` method, create an asynchronous function invoking an Amazon SNS client service object, passing the parameters object\. 

**Note**  
Replace *TOPIC\_ARN* with the Amazon Resource Name \(ARN\) for the topic, *TOKEN* with the token value from the URL sent to the endpoint owner in a previous `Subscribe` action, and define *AuthenticateOnUnsubscribe*\. with a value of `TRUE` or `FALSE`\.

```
// Import required AWS SDK clients and commands for Node.js
import {ConfirmSubscriptionCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = {
  Token: "TOKEN", // Required. Token sent to the endpoint by an earlier Subscribe action. */
  TopicArn: "TOPIC_ARN", // Required
  AuthenticateOnUnsubscribe: "true", // 'true' or 'false'
};

const run = async () => {
  try {
    const data = await snsClient.send(new ConfirmSubscriptionCommand(params));
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
node sns_confirmsubscription.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/sns_confirmsubscription.js)\.

## Subscribing an Application Endpoint to a Topic<a name="sns-examples-subscribing-apps"></a>

In this example, use a Node\.js module to subscribe a mobile application endpoint so it receives notifications from an Amazon SNS topic\. 

Create a `libs` directory, and create a Node\.js module with the file name `snsClient.js`\. Copy and paste the code below into it, which creates the Amazon SNS client object\. Replace *REGION* with your AWS Region\.

```
import  { SNSClient } from "@aws-sdk/client-sns";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const snsClient = new SNSClient({ region: REGION });
export  { snsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/libs/snsClient.js)\.

Create a Node\.js module with the file name `sns_confirmsubscription.js`\. Configure the SDK as previously shown, including installing the required modules and packages\.

Create an object containing the `Protocol` parameter to specify the `application` protocol, the `TopicArn` for the topic to subscribe to, and the Amazon Resource Name \(ARN\) of a mobile application endpoint for the `Endpoint` parameter\. Pass the parameters to the `SubscribeCommand` method of the `SNS` client class\.

To call the `SubscribeCommand` method, create an asynchronous function invoking an Amazon SNS service object, passing the parameters object\. 

**Note**  
Replace *TOPIC\_ARN* with the Amazon Resource Name \(ARN\) for the topic, and *MOBILE\_ENDPOINT\_ARN* with the endpoint you are subscribing to the topic\.

```
// Import required AWS SDK clients and commands for Node.js
import {SubscribeCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = {
  Protocol: "application" /* required */,
  TopicArn: "TOPIC_ARN", //TOPIC_ARN
  Endpoint: "MOBILE_ENDPOINT_ARN", // MOBILE_ENDPOINT_ARN
};

const run = async () => {
  try {
    const data = await snsClient.send(new SubscribeCommand(params));
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
node sns_subscribeapp.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/sns/src/sns_subscribeapp.js)\.

## Subscribing a Lambda Function to a Topic<a name="sns-examples-subscribing-lambda"></a>

In this example, use a Node\.js module to subscribe an AWS Lambda function so it receives notifications from an Amazon SNS topic\. 

Create a `libs` directory, and create a Node\.js module with the file name `snsClient.js`\. Copy and paste the code below into it, which creates the Amazon SNS client object\. Replace *REGION* with your AWS Region\.

```
import  { SNSClient } from "@aws-sdk/client-sns";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const snsClient = new SNSClient({ region: REGION });
export  { snsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/libs/snsClient.js)\.

Create a Node\.js module with the file name `sns_subscribelambda.js`\. Configure the SDK as previously shown\.

Create an object containing the `Protocol` parameter, specifying the `lambda` protocol, the `TopicArn` for the topic to subscribe to, and the Amazon Resource Name \(ARN\) of an AWS Lambda function as the `Endpoint` parameter\. Pass the parameters to the `SubscribeCommand` method of the `SNS` client class\.

To call the `SubscribeCommand` method, create an asynchronous function invoking an Amazon SNS client service object, passing the parameters object\. 

**Note**  
Replace *TOPIC\_ARN* with the Amazon Resource Name \(ARN\) for the topic, and *LAMBDA\_FUNCTION\_ARN* with the Amazon Resource Name \(ARN\) of the Lambda function\.

```
// Import required AWS SDK clients and commands for Node.js
import {SubscribeCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = {
  Protocol: "lambda" /* required */,
  TopicArn: "TOPIC_ARN", //TOPIC_ARN
  Endpoint: "LAMBDA_FUNCTION_ARN", //LAMBDA_FUNCTION_ARN
};

const run = async () => {
  try {
    const data = await snsClient.send(new SubscribeCommand(params));
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
node sns_subscribelambda.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/sns_subscribelambda.js)\.

## Unsubscribing from a Topic<a name="sns-examples-unsubscribing"></a>

In this example, use a Node\.js module to unsubscribe an Amazon SNS topic subscription\.

Create a `libs` directory, and create a Node\.js module with the file name `snsClient.js`\. Copy and paste the code below into it, which creates the Amazon SNS client object\. Replace *REGION* with your AWS Region\.

```
import  { SNSClient } from "@aws-sdk/client-sns";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SNS service object.
const snsClient = new SNSClient({ region: REGION });
export  { snsClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/libs/snsClient.js)\.

Create a Node\.js module with the file name `sns_unsubscribe.js`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object containing the `SubscriptionArn` parameter, specifying the Amazon Resource Name \(ARN\) of the subscription to unsubscribe\. Pass the parameters to the `UnsubscribeCommand` method of the `SNS` client class\.

To call the `UnsubscribeCommand` method, create an asynchronous function invoking an Amazon SNS client service object, passing the parameters object\. 

**Note**  
Replace *TOPIC\_SUBSCRIPTION\_ARN* with the Amazon Resource Name \(ARN\) of the subscription to unsubscribe\.

```
// Import required AWS SDK clients and commands for Node.js
import {UnsubscribeCommand } from "@aws-sdk/client-sns";
import {snsClient } from "./libs/snsClient.js";

// Set the parameters
const params = { SubscriptionArn: "TOPIC_SUBSCRIPTION_ARN" }; //TOPIC_SUBSCRIPTION_ARN

const run = async () => {
  try {
    const data = await snsClient.send(new UnsubscribeCommand(params));
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
node sns_unsubscribe.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/sns/src/sns_unsubscribe.js)\.