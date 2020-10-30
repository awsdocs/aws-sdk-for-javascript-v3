--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Creating alarms in Amazon CloudWatch<a name="cloudwatch-examples-creating-alarms"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve basic information about your CloudWatch alarms\.
+ How to create and delete a CloudWatch alarm\.

## The scenario<a name="cloudwatch-examples-creating-alarms-scenario"></a>

An alarm watches a single metric over a time period you specify, and performs one or more actions based on the value of the metric relative to a given threshold over a number of time periods\.

In this example, a series of Node\.js modules are used to create alarms in CloudWatch\. The Node\.js modules use the SDK for JavaScript to create alarms using these methods of the `CloudWatch` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#describeAlarms-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#describeAlarms-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#putMetricAlarm-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#putMetricAlarm-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#deleteAlarms-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#deleteAlarms-property)

For more information about CloudWatch alarms, see [Creating Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the *Amazon CloudWatch User Guide*\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

## Prerequisite tasks<a name="cloudwatch-examples-creating-alarms-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/cloudwatch/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Describing alarms<a name="cloudwatch-examples-creating-alarms-describing"></a>

Create a Node\.js module with the file name `cw_describealarms.ts`\. Be sure to configure the SDK as previously shown, including downloading the `CloudWatch` client\. To access CloudWatch, create a `CloudWatch` client service object\. Create a JSON object to hold the parameters for retrieving alarm descriptions, limiting the alarms returned to those with a state of `INSUFFICIENT_DATA`\. Then call the `DescribeAlarmsCommand` method of the `CloudWatch` client service object\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  CloudWatchClient,
  DescribeAlarmsCommand
} = require("@aws-sdk/client-cloudwatch");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { StateValue: "INSUFFICIENT_DATA" };

// Create CloudWatch service object
const cw = new CloudWatchClient(REGION);

const run = async () => {
  try {
    const data = await cw.send(new DescribeAlarmsCommand(params));
    console.log("Success", data);
    data.MetricAlarms.forEach(function (item, index, array) {
      console.log(item.AlarmName);
    });
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node cw_describealarms.ts //If you prefer JavaScript, enter 'node cw-describealarms.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/cw_describealarms.ts)\. 

## Creating an alarm for a CloudWatch metric<a name="cloudwatch-examples-creating-alarms-putmetricalarm"></a>

Create a Node\.js module with the file name `cw_putmetricalarm.ts`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\. To access CloudWatch, create a `CloudWatch` service object\. Create a JSON object for the parameters needed to create an alarm based on a metric, in this case the CPU utilization of an Amazon EC2 instance\. The remaining parameters are set so the alarm triggers when the metric exceeds a threshold of 70 percent\. Then call the `DescribeAlarmsCommand` method of the `CloudWatch` client service object\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *INSTANCE\_ID* with the ID of the Amazon EC2 instance\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  CloudWatchClient,
  PutMetricAlarmCommand,
} = require("@aws-sdk/client-cloudwatch");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  AlarmName: "Web_Server_CPU_Utilization",
  ComparisonOperator: "GreaterThanThreshold",
  EvaluationPeriods: 1,
  MetricName: "CPUUtilization",
  Namespace: "AWS/EC2",
  Period: 60,
  Statistic: "Average",
  Threshold: 70.0,
  ActionsEnabled: false,
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
    console.log(
      "Success",
      data.$metadata.requestId
    );
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node cw_putmetricalarm.ts // If you prefer JavaScript, enter 'node cw_putmetricalarm.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/cw_putmetricalarm.ts)\.

## Deleting an alarm<a name="cloudwatch-examples-creating-alarms-deleting"></a>

Create a Node\.js module with the file name `cw_deletealarms.ts`\. Be sure to configure the SDK as previously shown, including downloading the CloudWatch client\. To access CloudWatch, create an `CloudWatch` client service object\. Create a JSON object to hold the names of the alarms to delete\. Then call the `DeleteAlarmsCommand` method of the `CloudWatch` client service object\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *ALARM\_NAMES* with the names of the alarms\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  CloudWatchClient,
  DeleteAlarmsCommand,
} = require("@aws-sdk/client-cloudwatch");

// Set the AWS Region
const REGION = "REGION"; //e.g., "us-east-1"

// Set the parameters
const params = { AlarmNames: "ALARM_NAMES" }; // e.g., "Web_Server_CPU_Utilization"

// Create CloudWatch service object
const cw = new CloudWatchClient(REGION);

const run = async () => {
  try {
    const data = await cw.send(new DeleteAlarmsCommand(params));
    console.log("Success, alarm deleted; requestID:", data.$metadata.requestId);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node cw_deletealarms.ts // If you prefer to use JavaScript, enter 'node cw_deletealarms.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/cw_deletealarms.ts)\.