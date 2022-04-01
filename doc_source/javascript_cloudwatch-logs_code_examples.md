--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# CloudWatch Logs examples using SDK for JavaScript V3<a name="javascript_cloudwatch-logs_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for JavaScript V3 with CloudWatch Logs\.

*Actions* are code excerpts that show you how to call individual CloudWatch Logs functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple CloudWatch Logs functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w362aac23b9c15c13)

## Actions<a name="w362aac23b9c15c13"></a>

### Create a subscription filter<a name="cloudwatch-logs_PutSubscriptionFilter_javascript_topic"></a>

The following code example shows how to create an Amazon CloudWatch Logs subscription filter\.

**SDK for JavaScript V3**  
Create the client in a separate module and export it\.  

```
import { CloudWatchLogsClient } from "@aws-sdk/client-cloudwatch-logs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon CloudWatch Logs service client object.
export const cwlClient = new CloudWatchLogsClient({ region: REGION });
```
Import the SDK and client modules and call the API\.  

```
// Import required AWS SDK clients and commands for Node.js
import {
  PutSubscriptionFilterCommand,
} from "@aws-sdk/client-cloudwatch-logs";
import { cwlClient } from "./libs/cloudWatchLogsClient.js";

// Set the parameters
export const params = {
  destinationArn: "LAMBDA_FUNCTION_ARN", //LAMBDA_FUNCTION_ARN
  filterName: "FILTER_NAME", //FILTER_NAME
  filterPattern: "ERROR",
  logGroupName: "LOG_GROUP", //LOG_GROUP
};

export const run = async () => {
  try {
    const data = await cwlClient.send(new PutSubscriptionFilterCommand(params));
    console.log("Success", data.subscriptionFilters);
    return data; //For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
// Uncomment this line to run execution within this file.
// run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cloudwatch-logs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/cloudwatch-examples-subscriptions.html#cloudwatch-examples-subscriptions-creating)\. 
+  For API details, see [PutSubscriptionFilter](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch-logs/classes/putsubscriptionfiltercommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/cloudwatch-logs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/cloudwatch-examples-subscriptions.html#cloudwatch-examples-subscriptions-creating)\. 
+  For API details, see [PutSubscriptionFilter](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/logs-2014-03-28/PutSubscriptionFilter) in *AWS SDK for JavaScript API Reference*\. 

### Delete a subscription filter<a name="cloudwatch-logs_DeleteSubscriptionFilter_javascript_topic"></a>

The following code example shows how to delete an Amazon CloudWatch Logs subscription filter\.

**SDK for JavaScript V3**  
Create the client in a separate module and export it\.  

```
import { CloudWatchLogsClient } from "@aws-sdk/client-cloudwatch-logs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon CloudWatch Logs service client object.
export const cwlClient = new CloudWatchLogsClient({ region: REGION });
```
Import the SDK and client modules and call the API\.  

```
// Import required AWS SDK clients and commands for Node.js
import {
  DeleteSubscriptionFilterCommand
} from "@aws-sdk/client-cloudwatch-logs";
import { cwlClient } from "./libs/cloudWatchLogsClient.js";

// Set the parameters
export const params = {
  filterName: "FILTER", //FILTER
  logGroupName: "LOG_GROUP", //LOG_GROUP
};

export const run = async () => {
  try {
    const data = await cwlClient.send(
      new DeleteSubscriptionFilterCommand(params)
    );
    console.log(
      "Success, subscription filter deleted",
      data
    );
    return data; //For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
// Uncomment this line to run execution within this file.
// run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cloudwatch-logs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/cloudwatch-examples-subscriptions.html#cloudwatch-examples-subscriptions-deleting)\. 
+  For API details, see [DeleteSubscriptionFilter](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch-logs/classes/deletesubscriptionfiltercommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/cloudwatch-logs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/cloudwatch-examples-subscriptions.html#cloudwatch-examples-subscriptions-deleting)\. 
+  For API details, see [DeleteSubscriptionFilter](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/logs-2014-03-28/DeleteSubscriptionFilter) in *AWS SDK for JavaScript API Reference*\. 

### Describe existing subscription filters<a name="cloudwatch-logs_DescribeSubscriptionFilters_javascript_topic"></a>

The following code example shows how to describe Amazon CloudWatch Logs existing subscription filters\.

**SDK for JavaScript V3**  
Create the client in a separate module and export it\.  

```
import { CloudWatchLogsClient } from "@aws-sdk/client-cloudwatch-logs";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon CloudWatch Logs service client object.
export const cwlClient = new CloudWatchLogsClient({ region: REGION });
```
Import the SDK and client modules and call the API\.  

```
// Import required AWS SDK clients and commands for Node.js
import { DescribeSubscriptionFiltersCommand } from "@aws-sdk/client-cloudwatch-logs";
import { cwlClient } from "./libs/cloudWatchLogsClient.js";

// Set the parameters
export const params = {
  logGroupName: "GROUP_NAME", //GROUP_NAME
  limit: 5
};

export const run = async () => {
  try {
    const data = await cwlClient.send(
      new DescribeSubscriptionFiltersCommand(params)
    );
    console.log("Success", data.subscriptionFilters);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
// Uncomment this line to run execution within this file.
// run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cloudwatch-logs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/cloudwatch-examples-subscriptions.html#cloudwatch-examples-subscriptions-describing)\. 
+  For API details, see [DescribeSubscriptionFilters](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch-logs/classes/describesubscriptionfilterscommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/cloudwatch-logs#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/cloudwatch-examples-subscriptions.html#cloudwatch-examples-subscriptions-describing)\. 
+  For API details, see [DescribeSubscriptionFilters](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/logs-2014-03-28/DescribeSubscriptionFilters) in *AWS SDK for JavaScript API Reference*\. 