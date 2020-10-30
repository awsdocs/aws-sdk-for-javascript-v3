--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

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
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchEvents.html#putRule-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchEvents.html#putRule-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchEvents.html#putTargets-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchEvents.html#putTargets-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchEvents.html#putEvents-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchEvents.html#putEvents-property)

For more information about CloudWatch Events, see [Adding events with PutEvents](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/AddEventsPutEvents.html) in the *Amazon CloudWatch Events User Guide*\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

## Prerequisite tasks<a name="cloudwatch-examples-sending-events-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/cloudwatch/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so with minor adjustments these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create an AWS Lambda function using the **hello\-world** blueprint to serve as the target for events\. To learn how, see [ Step 1: Create an AWS Lambda function](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/LogEC2InstanceState.html) in the *Amazon CloudWatch Events User Guide*\.
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

## Creating a scheduled rule<a name="cloudwatch-examples-sending-events-rules"></a>

Create a Node\.js module with the file name `cwe_putrule.ts`\. Be sure to configure the SDK as previously shown, including downloading the CloudWatch client\. To access CloudWatch Events, create an `CloudWatchEvents` client service object\. Create a JSON object containing the parameters needed to specify the new scheduled rule, which include the following: 
+ A name for the rule
+ The ARN of the IAM role you created previously
+ An expression to schedule triggering of the rule every five minutes

Call the `PutRuleCommand` method to create the rule\. The callback returns the ARN of the new or updated rule\.

**Note**  
Replace *REGION* with your AWS Region\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  CloudWatchEventsClient,
  PutRuleCommand,
} = require("@aws-sdk/client-cloudwatch-events");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  Name: "DEMO_EVENT",
  RoleArn: "IAM_ROLE_ARN", //IAM_ROLE_ARN
  ScheduleExpression: "rate(5 minutes)",
  State: "ENABLED",
};

// Create CloudWatch service object
const cwevents = new CloudWatchEventsClient(REGION);

const run = async () => {
  try {
    const data = await cwevents.send(new PutRuleCommand(params));
    console.log("Success, scheduled rule created; Rule ARN:", data.RuleArn);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node cwe_putrule.ts // If you prefer JavaScript, enter 'node cwe_putrule.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/cwe_putrule.ts)\.

## Adding a Lambda function target<a name="cloudwatch-examples-sending-events-targets"></a>

Create a Node\.js module with the file name `cwe_puttargets.ts`\. Be sure to configure the SDK as previously shown, including downloading the CloudWatch client\. To access CloudWatch Events, create an `CloudWatchEvents` service object\. Create a JSON object containing the parameters needed to specify the rule to which to attach the target, including the ARN of the Lambda function you created\. Call the `PutTargetsCommand` method of the `CloudWatchEvents` service object\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *LAMBDA\_FUNCTION\_ARN* with the ARN of the Lambda function\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  CloudWatchEventsClient,
  PutTargetsCommand,
} = require("@aws-sdk/client-cloudwatch-events");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  Rule: "DEMO_EVENT",
  Targets: [
    {
      Arn: "LAMBDA_FUNCTION_ARN", //LAMBDA_FUNCTION_ARN
      Id: "myCloudWatchEventsTarget",
    },
  ],
};

// Create CloudWatch service object
const cwevents = new CloudWatchEventsClient(REGION);

const run = async () => {
  try {
    const data = await cwevents.send(new PutTargetsCommand(params));
    console.log("Success, target added; requestID: ", data.$metadata.requestId);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node cwe_puttargets.ts // If you prefer JavaScript, enter 'node cwe_puttargets.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/cwe_puttargets.ts)\.

## Sending events<a name="cloudwatch-examples-sending-events-putevents"></a>

Create a Node\.js module with the file name `cwe_putevents.ts`\. Be sure to configure the SDK as previously shown, including downloading the CloudWatch client\. To access CloudWatch Events, create an `CloudWatchEvents` client service object\. Create a JSON object containing the parameters needed to send events\. For each event, include the source of the event, the ARNs of any resources affected by the event, and details for the event\. Call the `PutEventsCommands` method of the `CloudWatchEvents` client service object\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *RESOURCE\_ARN* with the resources affected by the event\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  CloudWatchEventsClient,
  PutEventsCommand,
} = require("@aws-sdk/client-cloudwatch-events");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
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

// Create CloudWatch service object
const cwevents = new CloudWatchEventsClient(REGION);

const run = async () => {
  try {
    const data = await cwevents.send(new PutEventsCommand(params));
    console.log("Success, event sent; requestID:", data.$metadata.requestId);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node cwe_putevents.ts // If you prefer JavaScript, enter 'node cwe_putevents.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/cwe_putevents.ts)\.