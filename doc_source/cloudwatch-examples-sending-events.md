--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Sending events to Amazon CloudWatch Events<a name="cloudwatch-examples-sending-events"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create and update a rule used to trigger an event\.
+ How to define one or more targets to respond to an event\.
+ How to send events that are matched to targets for handling\.

## The scenario<a name="cloudwatch-examples-sending-events-scenario"></a>

CloudWatch Events delivers a near real\-time stream of system events that describe changes in Amazon Web Services resources to any of various targets\. Using simple rules, you can match events and route them to one or more target functions or streams\.

In this example, a series of Node\.js modules are used to send events to CloudWatch Events\. The Node\.js modules use the SDK for JavaScript to manage instances using these methods of the `CloudWatchEvents` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch-events/classes/putrulecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch-events/classes/putrulecommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch-events/classes/puttargetscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch-events/classes/puttargetscommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch-events/classes/puteventscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch-events/classes/puteventscommand.html)

For more information about CloudWatch Events, see [Adding events with PutEvents](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/AddEventsPutEvents.html) in the *Amazon CloudWatch Events User Guide*\.

## Prerequisite tasks<a name="cloudwatch-examples-sending-events-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/cloudwatch/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create an AWS Lambda function using the **hello\-world** blueprint to serve as the target for events\. To learn how, see [ Step 1: Create an Lambda function](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/LogEC2InstanceState.html) in the *Amazon CloudWatch Events User Guide*\.
+ Create an IAM role whose policy grants permission to CloudWatch Events and that includes `events.amazonaws.com` as a trusted entity\. For more information about creating an IAM role, see [ Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

Use the following role policy when creating the IAM role\.

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Sid": "CloudWatchEventsFullAccess",
         "Effect": "Allow",
         "Action": "events:*",
         "Resource": "*"
      },
      {
         "Sid": "IAMPassRoleForCloudWatchEvents",
         "Effect": "Allow",
         "Action": "iam:PassRole",
         "Resource": "arn:aws:iam::*:role/AWS_Events_Invoke_Targets"
      }      
   ]
}
```

Use the following trust relationship when creating the IAM role\.

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Allow",
         "Principal": {
            "Service": "events.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
      }      
   ]
}
```

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Creating a scheduled rule<a name="cloudwatch-examples-sending-events-rules"></a>

Create a `libs` directory, and create a Node\.js module with the file name `cloudWatchEventsClient.js`\. Copy and paste the code below into it, which creates the CloudWatch Events client object\. Replace *REGION* with your AWS region\.

```
import { CloudWatchEventsClient } from "@aws-sdk/client-cloudwatch-events";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon CloudWatch service client object.
export const cweClient = new CloudWatchEventsClient({ region: REGION });
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/libs/cloudWatchEventsClient.js)\.

Create a Node\.js module with the file name `putRule.js`\. Be sure to configure the SDK as previously shown, including downloading the CloudWatch client\. To access CloudWatch Events, create an `CloudWatchEvents` client service object\. Create a JSON object containing the parameters needed to specify the new scheduled rule, which include the following: 
+ A name for the rule
+ The ARN of the IAM role you created previously
+ An expression to schedule triggering of the rule every five minutes

Call the `PutRuleCommand` method to create the rule\. The callback returns the ARN of the new or updated rule\.

```
// Import required AWS SDK clients and commands for Node.js
import { PutRuleCommand } from "@aws-sdk/client-cloudwatch-events";
import { cweClient } from "./libs/cloudWatchEventsClient.js";

// Set the parameters
export const params = {
  Name: "DEMO_EVENT",
  RoleArn: "IAM_ROLE_ARN", //IAM_ROLE_ARN
  ScheduleExpression: "rate(5 minutes)",
  State: "ENABLED",
};

export const run = async () => {
  try {
    const data = await cweClient.send(new PutRuleCommand(params));
    console.log("Success, scheduled rule created; Rule ARN:", data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
// Uncomment this line to run execution within this file.
// run();
```

To run the example, enter the following at the command prompt\.

```
node putRule.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch-events/src/putRule.js)\.

## Adding a Lambda function target<a name="cloudwatch-examples-sending-events-targets"></a>

Create a `libs` directory, and create a Node\.js module with the file name `cloudWatchEventsClient.js`\. Copy and paste the code below into it, which creates the CloudWatch Events client object\. Replace *REGION* with your AWS region\.

```
import { CloudWatchEventsClient } from "@aws-sdk/client-cloudwatch-events";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon CloudWatch service client object.
export const cweClient = new CloudWatchEventsClient({ region: REGION });
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/libs/cloudWatchEventsClient.js)\.

Create a Node\.js module with the file name `putTargets.js`\. Be sure to configure the SDK as previously shown, including downloading the CloudWatch client\. To access CloudWatch Events, create an `CloudWatchEvents` service object\. Create a JSON object containing the parameters needed to specify the rule to which to attach the target, including the ARN of the Lambda function you created\. Call the `PutTargetsCommand` method of the `CloudWatchEvents` service object\.

**Note**  
Replace *LAMBDA\_FUNCTION\_ARN* with the ARN of the Lambda function\.

```
// Import required AWS SDK clients and commands for Node.js
import { PutTargetsCommand } from "@aws-sdk/client-cloudwatch-events";
import { cweClient } from "./libs/cloudWatchEventsClient.js";

// Set the parameters
export const params = {
  Rule: "DEMO_EVENT",
  Targets: [
    {
      Arn: "LAMBDA_FUNCTION_ARN", //LAMBDA_FUNCTION_ARN
      Id: "myCloudWatchEventsTarget",
    },
  ],
};

export const run = async () => {
  try {
    const data = await cweClient.send(new PutTargetsCommand(params));
    console.log("Success, target added; requestID: ", data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
// Uncomment this line to run execution within this file.
// run();
```

To run the example, enter the following at the command prompt\.

```
node putTargets.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch-events/src/putTargets.js)\.

## Sending events<a name="cloudwatch-examples-sending-events-putevents"></a>

Create a `libs` directory, and create a Node\.js module with the file name `cloudWatchEventsClient.js`\. Copy and paste the code below into it, which creates the CloudWatch Events client object\. Replace *REGION* with your AWS region\.

```
import { CloudWatchEventsClient } from "@aws-sdk/client-cloudwatch-events";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon CloudWatch service client object.
export const cweClient = new CloudWatchEventsClient({ region: REGION });
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/libs/cloudWatchEventsClient.js)\.

Create a Node\.js module with the file name `putEvents.js`\. Be sure to configure the SDK as previously shown, including downloading the CloudWatch client\. To access CloudWatch Events, create an `CloudWatchEvents` client service object\. Create a JSON object containing the parameters needed to send events\. For each event, include the source of the event, the ARNs of any resources affected by the event, and details for the event\. Call the `PutEventsCommands` method of the `CloudWatchEvents` client service object\.

**Note**  
Replace *RESOURCE\_ARN* with the resources affected by the event\.

```
// Import required AWS SDK clients and commands for Node.js
import { PutEventsCommand } from "@aws-sdk/client-cloudwatch-events";
import { cweClient } from "./libs/cloudWatchEventsClient.js";

// Set the parameters
export const params = {
  Entries: [
    {
      Detail: '{ "key1": "value1", "key2": "value2" }',
      DetailType: "appRequestSubmitted",
      Resources: [
        "RESOURCE_ARN", //RESOURCE_ARN
      ],
      Source: "com.company.app",
    },
  ],
};

export const run = async () => {
  try {
    const data = await cweClient.send(new PutEventsCommand(params));
    console.log("Success, event sent; requestID:", data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
// Uncomment this line to run execution within this file.
// run();
```

To run the example, enter the following at the command prompt\.

```
node putEvents.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch-events/src/putEvents.js)\.