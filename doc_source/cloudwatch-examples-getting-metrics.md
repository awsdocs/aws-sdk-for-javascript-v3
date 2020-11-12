--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Getting metrics from Amazon CloudWatch<a name="cloudwatch-examples-getting-metrics"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve a list of published CloudWatch metrics\.
+ How to publish data points to CloudWatch metrics\.

## The scenario<a name="cloudwatch-examples-getting-metrics-scenario"></a>

Metrics are data about the performance of your systems\. You can enable detailed monitoring of some resources, such as your Amazon EC2 instances, or your own application metrics\. 

In this example, a series of Node\.js modules are used to get metrics from CloudWatch and to send events to Amazon CloudWatch Events\. The Node\.js modules use the SDK for JavaScript to get metrics from CloudWatch using these methods of the `CloudWatch` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#listMetrics-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#listMetrics-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#putMetricData-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#putMetricData-property)

For more information about CloudWatch metrics, see [Using Amazon CloudWatch metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html) in the *Amazon CloudWatch User Guide*\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

## Prerequisite tasks<a name="cloudwatch-examples-getting-metrics-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/cloudwatch/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so with minor adjustments these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Listing metrics<a name="cloudwatch-examples-getting-metrics-listing"></a>

Create a Node\.js module with the file name `cw_listmetrics.ts`\. Be sure to configure the SDK as previously shown, including downloading the CloudWatch client\. To access CloudWatch, create n `CloudWatch` client service object\. Create a JSON object containing the parameters needed to list metrics within the `AWS/Logs` namespace\. Call the `ListMetricsCommand` method to list the `IncomingLogEvents` metric\.

**Note**  
Replace *REGION* with your AWS Region\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  CloudWatchClient,
  ListMetricsCommand
} = require("@aws-sdk/client-cloudwatch");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  Dimensions: [
    {
      Name: "LogGroupName" /* required */,
    },
  ],
  MetricName: "IncomingLogEvents",
  Namespace: "AWS/Logs",
};

// Create CloudWatch service object
const cw = new CloudWatchClient(REGION);

const run = async () => {
  try {
    const data = await cw.send(new ListMetricsCommand(params));
    console.log("Success. Metrics:", JSON.stringify(data.Metrics));
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node cw_listmetrics.ts // If you prefer JavaScript, enter 'node cw_listmetrics.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/cw_listmetrics.ts)\.

## Submitting custom metrics<a name="cloudwatch-examples-getting-metrics-publishing-custom"></a>

Create a Node\.js module with the file name `cw_putmetricdata.ts`\. Be sure to configure the SDK as previously shown, including downloading the CloudWatch client\. To access CloudWatch, create an `CloudWatch` client service object\. Create a JSON object containing the parameters needed to submit a data point for the `PAGES_VISITED` custom metric\. Call the `PutMetricDataCommand` method\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  CloudWatchClient,
  PutMetricDataCommand,
} = require("@aws-sdk/client-cloudwatch");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  MetricData: [
    {
      MetricName: "PAGES_VISITED",
      Dimensions: [
        {
          Name: "UNIQUE_PAGES",
          Value: "URLS",
        },
      ],
      Unit: "None",
      Value: 1.0,
    },
  ],
  Namespace: "SITE/TRAFFIC",
};

// Create CloudWatch service object
const cw = new CloudWatchClient(REGION);

const run = async () => {
  try {
    const data = await cw.send(new PutMetricDataCommand(params));
    console.log("Success", data.$metadata.requestId);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node cw_putmetricdata.ts // If you prefer JavaScript, enter 'node cw_putmetricdata.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/cw_putmetricdata.ts)\.