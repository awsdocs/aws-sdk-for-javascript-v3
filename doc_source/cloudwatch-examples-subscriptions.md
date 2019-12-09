# Using Subscription Filters in Amazon CloudWatch Logs<a name="cloudwatch-examples-subscriptions"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create and delete filters for log events in CloudWatch Logs\.

## The Scenario<a name="cloudwatch-examples-subscriptions-scenario"></a>

Subscriptions provide access to a real\-time feed of log events from CloudWatch Logs and deliver that feed to other services, such as an Amazon Kinesis stream or AWS Lambda, for custom processing, analysis, or loading to other systems\. A subscription filter defines the pattern to use for filtering which log events are delivered to your AWS resource\.

In this example, a series of Node\.js modules are used to list, create, and delete a subscription filter in CloudWatch Logs\. The destination for the log events is a Lambda function\. The Node\.js modules use the SDK for JavaScript to manage subscription filters using these methods of the `CloudWatchLogs` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchLogs.html#putSubscriptionFilters-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchLogs.html#putSubscriptionFilters-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchLogs.html#describeSubscriptionFilters-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchLogs.html#describeSubscriptionFilters-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchLogs.html#deleteSubscriptionFilter-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchLogs.html#deleteSubscriptionFilter-property)

For more information about CloudWatch Logs subscriptions, see [Real\-time Processing of Log Data with Subscriptions](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Subscriptions.html) in the *Amazon CloudWatch Logs User Guide*\.

## Prerequisite Tasks<a name="cloudwatch-examples-subscriptions-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.
+ Create an AWS Lambda function as the destination for log events\. You will need to use the Amazon Resource Name \(ARN\) of this function\. For more information about setting up a Lambda function, see [Subscription Filters with AWS Lambda](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/SubscriptionFilters.html#LambdaFunctionExample) in the *Amazon CloudWatch Logs User Guide*\.
+ Create an IAM role whose policy grants permission to invoke the Lambda function you created and grants full access to CloudWatch Logs or apply the following policy to the execution role you create for the Lambda function\. For more information about creating an IAM role, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

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

## Describing Existing Subscription Filters<a name="cloudwatch-examples-subscriptions-describing"></a>

Create a Node\.js module with the file name `cwl_describesubscriptionfilters.js`\. Be sure to configure the SDK as previously shown\. To access CloudWatch Logs, create an `AWS.CloudWatchLogs` service object\. Create a JSON object containing the parameters needed to describe your existing filters, including the name of the log group and the maximum number of filters you want described\. Call the `describeSubscriptionFilters` method\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the CloudWatchLogs service object
var cwl = new AWS.CloudWatchLogs({apiVersion: '2014-03-28'});

var params = {
  logGroupName: 'GROUP_NAME',
  limit: 5
};

cwl.describeSubscriptionFilters(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.subscriptionFilters);
  }
});
```

To run the example, type the following at the command line\.

```
node cwl_describesubscriptionfilters.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/cloudwatch/cwl_describesubscriptionfilters.js)\.

## Creating a Subscription Filter<a name="cloudwatch-examples-subscriptions-creating"></a>

Create a Node\.js module with the file name `cwl_putsubscriptionfilter.js`\. Be sure to configure the SDK as previously shown\. To access CloudWatch Logs, create an `AWS.CloudWatchLogs` service object\. Create a JSON object containing the parameters needed to create a filter, including the ARN of the destination Lambda function, the name of the filter, the string pattern for filtering, and the name of the log group\. Call the `putSubscriptionFilters` method\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the CloudWatchLogs service object
var cwl = new AWS.CloudWatchLogs({apiVersion: '2014-03-28'});

var params = {
  destinationArn: 'LAMBDA_FUNCTION_ARN',
  filterName: 'FILTER_NAME',
  filterPattern: 'ERROR',
  logGroupName: 'LOG_GROUP',
};

cwl.putSubscriptionFilter(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node cwl_putsubscriptionfilter.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/cloudwatch/cwl_putsubscriptionfilter.js)\.

## Deleting a Subscription Filter<a name="cloudwatch-examples-subscriptions-deleting"></a>

Create a Node\.js module with the file name `cwl_deletesubscriptionfilters.js`\. Be sure to configure the SDK as previously shown\. To access CloudWatch Logs, create an `AWS.CloudWatchLogs` service object\. Create a JSON object containing the parameters needed to delete a filter, including the names of the filter and the log group\. Call the `deleteSubscriptionFilters` method\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the CloudWatchLogs service object
var cwl = new AWS.CloudWatchLogs({apiVersion: '2014-03-28'});

var params = {
  filterName: 'FILTER',
  logGroupName: 'LOG_GROUP'
};

cwl.deleteSubscriptionFilter(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node cwl_deletesubscriptionfilter.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/cloudwatch/cwl_deletesubscriptionfilter.js)\.