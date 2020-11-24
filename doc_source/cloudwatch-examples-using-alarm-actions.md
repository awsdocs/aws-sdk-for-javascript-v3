--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Using alarm actions in Amazon CloudWatch<a name="cloudwatch-examples-using-alarm-actions"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to change the state of your Amazon EC2 instances automatically based on a CloudWatch alarm\.

## The scenario<a name="cloudwatch-examples-using-alarm-actions-scenario"></a>

Using alarm actions, you can create alarms that automatically stop, terminate, reboot, or recover your Amazon EC2 instances\. You can use the stop or terminate actions when you no longer need an instance to be running\. You can use the reboot and recover actions to automatically reboot those instances\. 

In this example, a series of Node\.js modules are used to define an alarm action in CloudWatch that triggers the reboot of an Amazon EC2 instance\. The Node\.js modules use the SDK for JavaScript to manage Amazon EC2 instances using these methods of the `CloudWatch` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#enableAlarmActions-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#enableAlarmActions-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#disableAlarmActions-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#disableAlarmActions-property)

For more information about CloudWatch alarm actions, see [Create alarms to stop, terminate, reboot, or recover an instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/UsingAlarmActions.html) in the *Amazon CloudWatch User Guide*\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

## Prerequisite tasks<a name="cloudwatch-examples-using-alarm-actions-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/cloudwatch/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so with minor adjustments these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create an IAM role whose policy grants permission to describe, reboot, stop, or terminate an Amazon EC2 instance\. For more information about creating an IAM role, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

Use the following role policy when creating the IAM role\.

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Allow",
         "Action": [
            "cloudwatch:Describe*",
            "ec2:Describe*",
            "ec2:RebootInstances",
            "ec2:StopInstances*",
            "ec2:TerminateInstances"
         ],
         "Resource": [
            "*"
         ]
      }
   ]
}
```

## <a name="cloudwatch-examples-using-alarm-actions-configure-sdk"></a>

Configure the SDK for JavaScript by importing the `CloudWatch` client module, and the commands that this example requires\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  CloudWatch,
  PutMetricAlarmCommand,
  EnableAlarmActionsCommand,
} = require("@aws-sdk/client-cloudwatch");
```

## Creating and enabling actions on an alarm<a name="cloudwatch-examples-using-alarm-actions-enabling"></a>

Create a Node\.js module with the file name `cw_enablealarmactions.ts`\. Be sure to configure the SDK as previously shown, including downloading the CloudWatch client\. To access CloudWatch, create a `CloudWatch` client service object\.

Create a JSON object to hold the parameters for creating an alarm, specifying `ActionsEnabled` as `true` and an array of Amazon Resource Names \(ARNs\) for the actions the alarm will trigger\. Call the `PutMetricAlarmCommand` method of the `CloudWatch` client service object, which creates the alarm if it does not exist or updates it if the alarm does exist\.

In the callback function for the `PutMetricAlarmCommand`, on successful completion create a JSON object containing the name of the CloudWatch alarm\. Call the `EnableAlarmActionsCommand` method to enable the alarm action\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *ALARM\_NAME* with the name of the alarm, and *INSTANCE\_ID* with the ID of the Amazon EC2 instance\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  CloudWatchClient,
  PutMetricAlarmCommand,
  EnableAlarmActionsCommand
} = require("@aws-sdk/client-cloudwatch");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  AlarmName: "ALARM_NAME", //ALARM_NAME
  ComparisonOperator: "GreaterThanThreshold",
  EvaluationPeriods: 1,
  MetricName: "CPUUtilization",
  Namespace: "AWS/EC2",
  Period: 60,
  Statistic: "Average",
  Threshold: 70.0,
  ActionsEnabled: true,
  AlarmActions: ["ACTION_ARN"], //e.g., "arn:aws:automate:us-east-1:ec2:stop"
  AlarmDescription: "Alarm when server CPU exceeds 70%",
  Dimensions: [
    {
      Name: "InstanceId",
      Value: "INSTANCE_ID",
    },
  ],
  Unit: "Percent",
};

// Create CloudWatch service object
const cw = new CloudWatchClient(REGION);

const run = async () => {
  try {
    const data = await cw.send(new PutMetricAlarmCommand(params));
    console.log("Alarm action added; RequestID:", data.$metadata.requestId);
    const paramsEnableAlarmAction = {
      AlarmNames: [params.AlarmName],
    };
    try {
      const data = await cw.send(
        new EnableAlarmActionsCommand(paramsEnableAlarmAction)
      );
      console.log("Alarm action enabled; RequestID:", data.$metadata.requestId);
    } catch (err) {
      console.log("Error", err);
    }
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node cw_enablealarmactions.ts // If you prefer JavaScript, enter 'node cw_enablealarmactions.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/cw_enablealarmactions.ts)\.

## Disabling actions on an alarm<a name="cloudwatch-examples-using-alarm-actions-disabling"></a>

Create a Node\.js module with the file name `cw_disablealarmactions.ts`\. Be sure to configure the SDK as previously shown, including downloading the CloudWatch client\. 

To access CloudWatch, create a `CloudWatch` client service object\. Create a JSON object containing the name of the CloudWatch alarm\. Call the `DisableAlarmActionsCommand` method to disable the actions for this alarm\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *ALARM\_NAME* with the alarm names\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  CloudWatchClient,
  DisableAlarmActionsCommand
} = require("@aws-sdk/client-cloudwatch");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { AlarmNames: "ALARM_NAME" }; // e.g., "Web_Server_CPU_Utilization"

// Create CloudWatch service object
const cw = new CloudWatchClient(REGION);

const run = async () => {
  try {
    const data = await cw.send(new DisableAlarmActionsCommand(params));
    console.log("Success, alarm disabled:", data.$metadata.requestId);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node cw_disablealarmactions.ts // If you prefer JavaScript, enter 'node cw_disablealarmactions.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/cw_disablealarmactions.ts)\.