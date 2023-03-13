--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# CloudWatch Logs examples using SDK for JavaScript \(v3\)<a name="javascript_cloudwatch-logs_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for JavaScript \(v3\) with CloudWatch Logs\.

*Actions* are code excerpts that show you how to call individual service functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple functions within the same service\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#actions)

## Actions<a name="actions"></a>

### Create a log group<a name="cloudwatch-logs_CreateLogGroup_javascript_topic"></a>

The following code example shows how to create a new CloudWatch Logs log group\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cloudwatch-logs#code-examples)\. 
  

```
import { CreateLogGroupCommand } from "@aws-sdk/client-cloudwatch-logs";
import { client } from "../libs/client.js";

const run = async () => {
  const command = new CreateLogGroupCommand({
    // The name of the log group.
    logGroupName: process.env.CLOUDWATCH_LOGS_LOG_GROUP,
  });

  try {
    return await client.send(command);
  } catch (err) {
    console.error(err);
  }
};

export default run();
```
+  For API details, see [CreateLogGroup](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch-logs/classes/createloggroupcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create a subscription filter<a name="cloudwatch-logs_PutSubscriptionFilter_javascript_topic"></a>

The following code example shows how to create an Amazon CloudWatch Logs subscription filter\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cloudwatch-logs#code-examples)\. 
  

```
import { PutSubscriptionFilterCommand } from "@aws-sdk/client-cloudwatch-logs";
import { client } from "../libs/client.js";

const run = async () => {
  const command = new PutSubscriptionFilterCommand({
    // An ARN of a same-account Kinesis stream, Kinesis Firehose
    // delivery stream, or Lambda function.
    // https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/SubscriptionFilters.html
    destinationArn: process.env.CLOUDWATCH_LOGS_DESTINATION_ARN,

    // A name for the filter.
    filterName: process.env.CLOUDWATCH_LOGS_FILTER_NAME,

    // A filter pattern for subscribing to a filtered stream of log events.
    // https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/FilterAndPatternSyntax.html
    filterPattern: process.env.CLOUDWATCH_LOGS_FILTER_PATTERN,

    // The name of the log group. Messages in this group matching the filter pattern
    // will be sent to the destination ARN.
    logGroupName: process.env.CLOUDWATCH_LOGS_LOG_GROUP,
  });

  try {
    return await client.send(command);
  } catch (err) {
    console.error(err);
  }
};

export default run();
```
+  For API details, see [PutSubscriptionFilter](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch-logs/classes/putsubscriptionfiltercommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript \(v2\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/cloudwatch-logs#code-examples)\. 
  

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
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/cloudwatch-examples-subscriptions.html#cloudwatch-examples-subscriptions-creating)\. 
+  For API details, see [PutSubscriptionFilter](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/logs-2014-03-28/PutSubscriptionFilter) in *AWS SDK for JavaScript API Reference*\. 

### Delete a log group<a name="cloudwatch-logs_DeleteLogGroup_javascript_topic"></a>

The following code example shows how to delete an existing CloudWatch Logs log group\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cloudwatch-logs#code-examples)\. 
  

```
import { DeleteLogGroupCommand } from "@aws-sdk/client-cloudwatch-logs";
import { client } from "../libs/client.js";

const run = async () => {
  const command = new DeleteLogGroupCommand({
    // The name of the log group.
    logGroupName: process.env.CLOUDWATCH_LOGS_LOG_GROUP,
  });

  try {
    return await client.send(command);
  } catch (err) {
    console.error(err);
  }
};

export default run();
```
+  For API details, see [DeleteLogGroup](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch-logs/classes/deleteloggroupcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Delete a subscription filter<a name="cloudwatch-logs_DeleteSubscriptionFilter_javascript_topic"></a>

The following code example shows how to delete an Amazon CloudWatch Logs subscription filter\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cloudwatch-logs#code-examples)\. 
  

```
import { DeleteSubscriptionFilterCommand } from "@aws-sdk/client-cloudwatch-logs";
import { client } from "../libs/client.js";

const run = async () => {
  const command = new DeleteSubscriptionFilterCommand({
    // The name of the filter.
    filterName: process.env.CLOUDWATCH_LOGS_FILTER_NAME,
    // The name of the log group.
    logGroupName: process.env.CLOUDWATCH_LOGS_LOG_GROUP,
  });

  try {
    return await client.send(command);
  } catch (err) {
    console.error(err);
  }
};

export default run();
```
+  For API details, see [DeleteSubscriptionFilter](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch-logs/classes/deletesubscriptionfiltercommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript \(v2\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/cloudwatch-logs#code-examples)\. 
  

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
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/cloudwatch-examples-subscriptions.html#cloudwatch-examples-subscriptions-deleting)\. 
+  For API details, see [DeleteSubscriptionFilter](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/logs-2014-03-28/DeleteSubscriptionFilter) in *AWS SDK for JavaScript API Reference*\. 

### Describe existing subscription filters<a name="cloudwatch-logs_DescribeSubscriptionFilters_javascript_topic"></a>

The following code example shows how to describe Amazon CloudWatch Logs existing subscription filters\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cloudwatch-logs#code-examples)\. 
  

```
import { DescribeSubscriptionFiltersCommand } from "@aws-sdk/client-cloudwatch-logs";
import { client } from "../libs/client.js";

const run = async () => {
  // This will return a list of all subscription filters in your account
  // matching the log group name.
  const command = new DescribeSubscriptionFiltersCommand({
    logGroupName: process.env.CLOUDWATCH_LOGS_LOG_GROUP,
    limit: 1,
  });

  try {
    return await client.send(command);
  } catch (err) {
    console.error(err);
  }
};

export default run();
```
+  For API details, see [DescribeSubscriptionFilters](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch-logs/classes/describesubscriptionfilterscommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript \(v2\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/cloudwatch-logs#code-examples)\. 
  

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
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/cloudwatch-examples-subscriptions.html#cloudwatch-examples-subscriptions-describing)\. 
+  For API details, see [DescribeSubscriptionFilters](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/logs-2014-03-28/DescribeSubscriptionFilters) in *AWS SDK for JavaScript API Reference*\. 

### Describe log groups<a name="cloudwatch-logs_DescribeLogGroups_javascript_topic"></a>

The following code example shows how to describe CloudWatch Logs log groups\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cloudwatch-logs#code-examples)\. 
  

```
import { DescribeLogGroupsCommand } from "@aws-sdk/client-cloudwatch-logs";
import { client } from "../libs/client.js";

const run = async () => {
  // This command will return a list of all log groups in your account.
  const command = new DescribeLogGroupsCommand({});

  try {
    return await client.send(command);
  } catch (err) {
    console.error(err);
  }
};

export default run();
```
+  For API details, see [DescribeLogGroups](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch-logs/classes/describeloggroupscommand.html) in *AWS SDK for JavaScript API Reference*\. 