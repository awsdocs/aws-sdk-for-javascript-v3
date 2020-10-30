--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Using subscription filters in Amazon CloudWatch Logs<a name="cloudwatch-examples-subscriptions"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create and delete filters for log events in CloudWatch Logs\.

## The scenario<a name="cloudwatch-examples-subscriptions-scenario"></a>

Subscriptions provide access to a real\-time feed of log events from CloudWatch Logs and deliver that feed to other services, such as an Amazon Kinesis stream or AWS Lambda, for custom processing, analysis, or loading to other systems\. A subscription filter defines the pattern to use for filtering which log events are delivered to your AWS resource\.

In this example, a series of Node\.js modules are used to list, create, and delete a subscription filter in CloudWatch Logs\. The destination for the log events is a Lambda function\. The Node\.js modules use the SDK for JavaScript to manage subscription filters using these methods of the `CloudWatchLogs` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchLogs.html#putSubscriptionFilters-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchLogs.html#putSubscriptionFilters-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchLogs.html#describeSubscriptionFilters-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchLogs.html#describeSubscriptionFilters-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchLogs.html#deleteSubscriptionFilter-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchLogs.html#deleteSubscriptionFilter-property)

For more information about CloudWatch Logs subscriptions, see [Real\-time processing of log data with subscriptions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Subscriptions.html) in the *Amazon CloudWatch Logs User Guide*\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

## Prerequisite tasks<a name="cloudwatch-examples-subscriptions-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/cloudwatch/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so with minor adjustments these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create an AWS Lambda function as the destination for log events\. You will need to use the Amazon Resource Name \(ARN\) of this function\. For more information about setting up a Lambda function, see [Subscription filters with Lambda](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/SubscriptionFilters.html#LambdaFunctionExample) in the *Amazon CloudWatch Logs User Guide*\.
+ Create an IAM role whose policy grants permission to invoke the Lambda function you created and grants full access to CloudWatch Logs or apply the following policy to the execution role you create for the Lambda function\. For more information about creating an IAM role, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

Use the following role policy when creating the IAM role\.

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Allow",
         "Action": [
            "logs:CreateLogGroup",
            "logs:CrateLogStream",
            "logs:PutLogEvents"
         ],
         "Resource": "arn:aws:logs:*:*:*"
      },
      {
         "Effect": "Allow",
         "Action": [
            "lambda:InvokeFunction"
         ],
         "Resource": [
            "*"
         ]
      }
   ]
}
```

## Describing existing subscription filters<a name="cloudwatch-examples-subscriptions-describing"></a>

Create a Node\.js module with the file name `cwl_describesubscriptionfilters.ts`\. Be sure to configure the SDK as previously shown, including downloading the CloudWatch client\. To access CloudWatch Logs, create an `CloudWatchLogs` client service object\. Create a JSON object containing the parameters needed to describe your existing filters, including the name of the log group and the maximum number of filters to describe\. Call the `DescribeSubscriptionFiltersCommand` method\.

**Note**  
Replace *REGION* with your AWS Region, and *GROUP\_NAME* with the name of the group\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  CloudWatchLogsClient,
  DescribeSubscriptionFiltersCommand,
} = require("@aws-sdk/client-cloudwatch-logs");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  logGroupName: "GROUP_NAME", //GROUP_NAME
  limit: 5,
};

// Create CloudWatch service object
const cwl = new CloudWatchLogsClient(REGION);

const run = async () => {
  try {
    const data = await cwl.send(new DescribeSubscriptionFiltersCommand(params));
    console.log("Success", data.subscriptionFilters);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node cwl_describesubscriptionfilters.ts // If you prefer JavaScript, enter 'node cwl_describesubscriptionfilters.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/cwl_describesubscriptionfilters.ts)\.

## Creating a subscription filter<a name="cloudwatch-examples-subscriptions-creating"></a>

Create a Node\.js module with the file name `cwl_putsubscriptionfilter.ts`\. Be sure to configure the SDK as previously shown, including downloading the CloudWatch client\. To access CloudWatch Logs, create an `CloudWatchLogs` client service object\. Create a JSON object containing the parameters needed to create a filter, including the ARN of the destination Lambda function, the name of the filter, the string pattern for filtering, and the name of the log group\. Call the `PutSubscriptionFiltersCommand` method\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *LAMBDA\_FUNCTION\-ARN* with the ARN of the Lambda function, *FILTER\_NAME* with the name of the filter, and *LOG\_GROUP* with the log group\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  CloudWatchLogsClient,
  PutSubscriptionFilterCommand,
} = require("@aws-sdk/client-cloudwatch-logs");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  destinationArn: "LAMBDA_FUNCTION_ARN", //LAMBDA_FUNCTION_ARN
  filterName: "FILTER_NAME", //FILTER_NAME
  filterPattern: "ERROR",
  logGroupName: "LOG_GROUP", //LOG_GROUP
};

// Create CloudWatch service object
const cwl = new CloudWatchLogsClient(REGION);

const run = async () => {
  try {
    const data = await cwl.send(new PutSubscriptionFilterCommand(params));
    console.log("Success", data.subscriptionFilters);
  } catch (err) {
    console.log("Error", err);
  }
};
```

To run the example, enter the following at the command prompt\.

```
ts-node cwl_putsubscriptionfilter.ts // If you prefer JavaScript, enter 'node cwl_putsubscriptionfilter.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/cwl_putsubscriptionfilter.ts)\.

## Deleting a subscription filter<a name="cloudwatch-examples-subscriptions-deleting"></a>

Create a Node\.js module with the file name `cwl_deletesubscriptionfilters.ts`\. Be sure to configure the SDK as previously shown, including downloading the CloudWatch client\. To access CloudWatch Logs, create an `CloudWatchLogs` service object\. Create a JSON object containing the parameters needed to delete a filter, including the names of the filter and the log group\. Call the `DeleteSubscriptionFiltersCommand` method\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *FILTER\_NAME* with the name of the filter, and *LOG\_GROUP* with the log group\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  CloudWatchLogsClient,
  DeleteSubscriptionFilterCommand,
} = require("@aws-sdk/client-cloudwatch-logs");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  filterName: "FILTER", //FILTER
  logGroupName: "LOG_GROUP", //LOG_GROUP
};

// Create CloudWatch service object
const cwl = new CloudWatchLogsClient(REGION);

const run = async () => {
  try {
    const data = await cwl.send(new DeleteSubscriptionFilterCommand(params));
    console.log(
      "Success, subscription filter deleted; requestId: ",
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
ts-node cwl_deletesubscriptionfilter.ts // If you prefer JavaScript, enter 'node cwl_deletesubscriptionfilter.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cloudwatch/src/cwl_deletesubscriptionfilter.ts)\.