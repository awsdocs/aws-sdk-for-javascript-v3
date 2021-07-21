--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Getting metrics from Amazon CloudWatch<a name="cloudwatch-examples-getting-metrics"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve a list of published CloudWatch metrics\.
+ How to publish data points to CloudWatch metrics\.

## The scenario<a name="cloudwatch-examples-getting-metrics-scenario"></a>

Metrics are data about the performance of your systems\. You can enable detailed monitoring of some resources, such as your Amazon EC2 instances, or your own application metrics\. 

In this example, a series of Node\.js modules are used to get metrics from CloudWatch and to send events to Amazon CloudWatch Events\. The Node\.js modules use the SDK for JavaScript to get metrics from CloudWatch using these methods of the `CloudWatch` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch/classes/listmetricscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch/classes/listmetricscommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch/classes/putmetricdatacommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch/classes/putmetricdatacommand.html)

For more information about CloudWatch metrics, see [Using Amazon CloudWatch metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html) in the *Amazon CloudWatch User Guide*\.

## Prerequisite tasks<a name="cloudwatch-examples-getting-metrics-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/cloudwatch/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples use ECMAScript6 \(ES6\)\. This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.  
However, if you prefer to use CommonJS sytax, please refer to [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)

## Listing metrics<a name="cloudwatch-examples-getting-metrics-listing"></a>

Create a `libs` directory, and create a Node\.js module with the file name `cloudWatchClient.js`\. Copy and paste the code below into it, which creates the CloudWatch client object\. Replace *REGION* with your AWS region\.

```
import { CloudWatchClient } from "@aws-sdk/client-cloudwatch";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon CloudWatch service client object.
export const cwClient = new CloudWatchClient({ region: REGION });
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/libs/cloudWatchClient.js)\.

Create a Node\.js module with the file name `listMetrics.js`\. Be sure to configure the SDK as previously shown, including downloading the CloudWatch client\. Create a JSON object containing the parameters needed to list metrics\. Call the `ListMetricsCommand` method to list the `IncomingLogEvents` metric\.

```
// Import required AWS SDK clients and commands for Node.js
import { ListMetricsCommand } from "@aws-sdk/client-cloudwatch";
import { cwClient } from "./libs/cloudWatchClient.js";

// Set the parameters
export const params = {
  Dimensions: [
    {
      Name: "LogGroupName" /* required */,
    },
  ],
  MetricName: "IncomingLogEvents",
  Namespace: "AWS/Logs",
};

export const run = async () => {
  try {
    const data = await cwClient.send(new ListMetricsCommand(params));
    console.log("Success. Metrics:", JSON.stringify(data.Metrics));
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
node listMetrics.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/listMetrics.js)\.

## Submitting custom metrics<a name="cloudwatch-examples-getting-metrics-publishing-custom"></a>

Create a `libs` directory, and create a Node\.js module with the file name `cloudWatchClient.js`\. Copy and paste the code below into it, which creates the CloudWatch client object\. Replace *REGION* with your AWS region\.

```
import { CloudWatchClient } from "@aws-sdk/client-cloudwatch";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon CloudWatch service client object.
export const cwClient = new CloudWatchClient({ region: REGION });
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/libs/cloudWatchClient.js)\.

Create a Node\.js module with the file name `putMetricData.js`\. Be sure to configure the SDK as previously shown, including downloading the CloudWatch client\. Create a JSON object containing the parameters needed to submit a data point for the `PAGES_VISITED` custom metric\. Call the `PutMetricDataCommand` method\.

```
// Import required AWS SDK clients and commands for Node.js
import { PutMetricDataCommand } from "@aws-sdk/client-cloudwatch";
import { cwClient } from "./libs/cloudWatchClient.js";

// Set the parameters
export const params = {
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

export const run = async () => {
  try {
    const data = await cwClient.send(new PutMetricDataCommand(params));
    console.log("Success", data.$metadata.requestId);
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
node putMetricData.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/putMetricData.js)\.