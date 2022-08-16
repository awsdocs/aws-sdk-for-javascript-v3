--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Creating alarms in Amazon CloudWatch<a name="cloudwatch-examples-creating-alarms"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve basic information about your CloudWatch alarms\.
+ How to create and delete a CloudWatch alarm\.

## The scenario<a name="cloudwatch-examples-creating-alarms-scenario"></a>

An alarm watches a single metric over a time period you specify, and performs one or more actions based on the value of the metric relative to a given threshold over a number of time periods\.

In this example, a series of Node\.js modules are used to create alarms in CloudWatch\. The Node\.js modules use the SDK for JavaScript to create alarms using these methods of the `CloudWatch` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch/classes/describealarmscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch/classes/describealarmscommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch/classes/putmetricalarmcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch/classes/putmetricalarmcommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch/classes/deletealarmscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch/classes/deletealarmscommand.html)

For more information about CloudWatch alarms, see [Creating Amazon CloudWatch alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the *Amazon CloudWatch User Guide*\.

## Prerequisite tasks<a name="cloudwatch-examples-creating-alarms-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/cloudwatch/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples use ECMAScript6 \(ES6\)\. This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.  
However, if you prefer to use CommonJS syntax, please refer to [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)

## Describing alarms<a name="cloudwatch-examples-creating-alarms-describing"></a>

Create a `libs` directory, and create a Node\.js module with the file name `cloudWatchClient.js`\. Copy and paste the code below into it, which creates the CloudWatch client object\. Replace *REGION* with your AWS region\.

```
import { CloudWatchClient } from "@aws-sdk/client-cloudwatch";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon CloudWatch service client object.
export const cwClient = new CloudWatchClient({ region: REGION });
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/libs/cloudWatchClient.js)\.

Create a Node\.js module with the file name `describeAlarms.js`\. Be sure to configure the SDK as previously shown, including downloading the `CloudWatch` client\. Create a JSON object to hold the parameters for retrieving alarm descriptions, limiting the alarms returned to those with a state of `INSUFFICIENT_DATA`\. Then call the `DescribeAlarmsCommand` method of the `CloudWatch` client service object\.

```
// Import required AWS SDK clients and commands for Node.js
import { DescribeAlarmsCommand } from "@aws-sdk/client-cloudwatch";
import { cwClient } from "./libs/cloudWatchClient.js";

// Set the parameters
export const params = { StateValue: "INSUFFICIENT_DATA" };

export const run = async () => {
  try {
    const data = await cwClient.send(new DescribeAlarmsCommand(params));
    console.log("Success", data);
    return data;
    data.MetricAlarms.forEach(function (item, index, array) {
      console.log(item.AlarmName);
      return data;
    });
  } catch (err) {
    console.log("Error", err);
  }
};
// Uncomment this line to run execution within this file.
// run();
```

To run the example, enter the following at the command prompt\.

```
node describeAlarms.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/describeAlarms.js)\. 

## Creating an alarm for a CloudWatch metric<a name="cloudwatch-examples-creating-alarms-putmetricalarm"></a>

Create a `libs` directory, and create a Node\.js module with the file name `cloudWatchClient.js`\. Copy and paste the code below into it, which creates the CloudWatch client object\. Replace *REGION* with your AWS region\.

```
import { CloudWatchClient } from "@aws-sdk/client-cloudwatch";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon CloudWatch service client object.
export const cwClient = new CloudWatchClient({ region: REGION });
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/libs/cloudWatchClient.js)\.

Create a Node\.js module with the file name `putMetricAlarm.js`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\. Create a JSON object for the parameters needed to create an alarm based on a metric, in this case the CPU utilization of an Amazon EC2 instance\. The remaining parameters are set so the alarm triggers when the metric exceeds a threshold of 70 percent\. Then call the `DescribeAlarmsCommand` method of the `CloudWatch` client service object\.

**Note**  
Replace *INSTANCE\_ID* with the ID of the Amazon EC2 instance\.

```
// Import required AWS SDK clients and commands for Node.js
import { PutMetricAlarmCommand } from "@aws-sdk/client-cloudwatch";
import { cwClient } from "./libs/cloudWatchClient.js";

// Set the parameters
export const params = {
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

export const run = async () => {
  try {
    const data = await cwClient.send(new PutMetricAlarmCommand(params));
    console.log("Success", data);
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
node putMetricAlarm.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/putMetricAlarm.js)\.

## Deleting an alarm<a name="cloudwatch-examples-creating-alarms-deleting"></a>

Create a `libs` directory, and create a Node\.js module with the file name `cloudWatchClient.js`\. Copy and paste the code below into it, which creates the CloudWatch client object\. Replace *REGION* with your AWS region\.

```
import { CloudWatchClient } from "@aws-sdk/client-cloudwatch";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon CloudWatch service client object.
export const cwClient = new CloudWatchClient({ region: REGION });
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/libs/cloudWatchClient.js)\.

Create a Node\.js module with the file name `deleteAlarms.js`\. Be sure to configure the SDK as previously shown, including downloading the CloudWatch client\. Create a JSON object to hold the names of the alarms to delete\. Then call the `DeleteAlarmsCommand` method of the `CloudWatch` client service object\.

**Note**  
Replace *ALARM\_NAMES* with the names of the alarms\.

```
// Import required AWS SDK clients and commands for Node.js
import { DeleteAlarmsCommand } from "@aws-sdk/client-cloudwatch";
import { cwClient } from "./libs/cloudWatchClient.js";

// Set the parameters
export const params = { AlarmNames: "ALARM_NAME" }; // e.g., "Web_Server_CPU_Utilization"

export const run = async () => {
  try {
    const data = await cwClient.send(new DeleteAlarmsCommand(params));
    console.log("Success, alarm deleted; requestID:", data);
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
node deleteAlarms.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/deleteAlarms.js)\.