--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Using alarm actions in Amazon CloudWatch<a name="cloudwatch-examples-using-alarm-actions"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to change the state of your Amazon EC2 instances automatically based on a CloudWatch alarm\.

## The scenario<a name="cloudwatch-examples-using-alarm-actions-scenario"></a>

Using alarm actions, you can create alarms that automatically stop, terminate, reboot, or recover your Amazon EC2 instances\. You can use the stop or terminate actions when you no longer need an instance to be running\. You can use the reboot and recover actions to automatically reboot those instances\. 

In this example, a series of Node\.js modules are used to define an alarm action in CloudWatch that triggers the reboot of an Amazon EC2 instance\. The Node\.js modules use the SDK for JavaScript to manage Amazon EC2 instances using these methods of the `CloudWatch` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch/classes/enablealarmactionscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch/classes/enablealarmactionscommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch/classes/disablealarmactionscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch/classes/disablealarmactionscommand.html)

For more information about CloudWatch alarm actions, see [Create alarms to stop, terminate, reboot, or recover an instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/UsingAlarmActions.html) in the *Amazon CloudWatch User Guide*\.

## Prerequisite tasks<a name="cloudwatch-examples-using-alarm-actions-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/cloudwatch/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create an IAM role whose policy grants permission to describe, reboot, stop, or terminate an Amazon EC2 instance\. For more information about creating an IAM role, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

**Important**  
These examples use ECMAScript6 \(ES6\)\. This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.  
However, if you prefer to use CommonJS sytax, please refer to [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)

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

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## <a name="cloudwatch-examples-using-alarm-actions-configure-sdk"></a>

## Creating and enabling actions on an alarm<a name="cloudwatch-examples-using-alarm-actions-enabling"></a>

Create a `libs` directory, and create a Node\.js module with the file name `cloudWatchClient.js`\. Copy and paste the code below into it, which creates the CloudWatch client object\. Replace *REGION* with your AWS region\.

```
import { CloudWatchClient } from "@aws-sdk/client-cloudwatch";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon CloudWatch service client object.
export const cwClient = new CloudWatchClient({ region: REGION });
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/libs/cloudWatchClient.js)\.

Create a Node\.js module with the file name `enableAlarmActions.js`\. Be sure to configure the SDK as previously shown, including downloading the CloudWatch client\. 

Create a JSON object to hold the parameters for creating an alarm, specifying `ActionsEnabled` as `true` and an array of Amazon Resource Names \(ARNs\) for the actions the alarm will trigger\. Call the `PutMetricAlarmCommand` method of the `CloudWatch` client service object, which creates the alarm if it does not exist or updates it if the alarm does exist\.

In the callback function for the `PutMetricAlarmCommand`, on successful completion create a JSON object containing the name of the CloudWatch alarm\. Call the `EnableAlarmActionsCommand` method to enable the alarm action\.

**Note**  
Replace *ALARM\_NAME* with the name of the alarm, and *INSTANCE\_ID* with the ID of the Amazon EC2 instance\.

```
// Import required AWS SDK clients and commands for Node.js
import {
  PutMetricAlarmCommand,
  EnableAlarmActionsCommand,
} from "@aws-sdk/client-cloudwatch";
import { cwClient } from "./libs/cloudWatchClient.js";

// Set the parameters
export const params = {
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

export const run = async () => {
  try {
    const data = await cwClient.send(new PutMetricAlarmCommand(params));
    console.log("Alarm action added; RequestID:", data);
    return data;
    const paramsEnableAlarmAction = {
      AlarmNames: [params.AlarmName],
    };
    try {
      const data = await cwClient.send(
        new EnableAlarmActionsCommand(paramsEnableAlarmAction)
      );
      console.log("Alarm action enabled; RequestID:", data.$metadata.requestId);
    } catch (err) {
      console.log("Error", err);
      return data;
    }
  } catch (err) {
    console.log("Error", err);
  }
};
// Uncomment this line to run execution within this file.
// run();
```

To run the example, enter the following at the command prompt\.

```
node enableAlarmActions.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/enableAlarmActions.js)\.

## Disabling actions on an alarm<a name="cloudwatch-examples-using-alarm-actions-disabling"></a>

Create a `libs` directory, and create a Node\.js module with the file name `cloudWatchClient.js`\. Copy and paste the code below into it, which creates the CloudWatch client object\. Replace *REGION* with your AWS region\.

```
import { CloudWatchClient } from "@aws-sdk/client-cloudwatch";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon CloudWatch service client object.
export const cwClient = new CloudWatchClient({ region: REGION });
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/libs/cloudWatchClient.js)\.

Create a Node\.js module with the file name `disableAlarmActions.js`\. Be sure to configure the SDK as previously shown, including downloading the CloudWatch client\. Create a JSON object containing the name of the CloudWatch alarm\. Call the `DisableAlarmActionsCommand` method to disable the actions for this alarm\.

**Note**  
Replace *ALARM\_NAME* with the alarm names\.

```
// Import required AWS SDK clients and commands for Node.js
import { DisableAlarmActionsCommand } from "@aws-sdk/client-cloudwatch";
import { cwClient } from "./libs/cloudWatchClient.js";

// Set the parameters
export const params = { AlarmNames: "ALARM_NAME" }; // e.g., "Web_Server_CPU_Utilization"

export const run = async () => {
  try {
    const data = await cwClient.send(new DisableAlarmActionsCommand(params));
    console.log("Success, alarm disabled:", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
// Uncomment this line to run execution within this file.
// run();
```

To run the example, enter the following at the command prompt\.

```
node disableAlarmActions.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/disableAlarmActions.js)\.