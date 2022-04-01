--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# EventBridge examples using SDK for JavaScript V3<a name="javascript_eventbridge_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for JavaScript V3 with EventBridge\.

*Actions* are code excerpts that show you how to call individual EventBridge functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple EventBridge functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w362aac23b9c19c13)

## Actions<a name="w362aac23b9c19c13"></a>

### Add a Lambda function target<a name="eventbridge_PutTargets_javascript_topic"></a>

The following code example shows how to add an AWS Lambda function target to an Amazon EventBridge event\.

**SDK for JavaScript V3**  
Create the client in a separate module and export it\.  

```
import { EventBridgeClient } from "@aws-sdk/client-eventbridge";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon EventBridge service client object.
export const ebClient = new EventBridgeClient({ region: REGION });
```
Import the SDK and client modules and call the API\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { PutTargetsCommand } from "@aws-sdk/client-eventbridge";
import { ebClient } from "./libs/eventBridgeClient.js";

// Set the parameters.
export const params = {
  Rule: "DEMO_EVENT",
  Targets: [
    {
      Arn: "LAMBDA_FUNCTION_ARN", //LAMBDA_FUNCTION_ARN
      Id: "myCloudWatchEventsTarget",
    },
  ],
};

export const run = async () => {
  try {
    const data = await ebClient.send(new PutTargetsCommand(params));
    console.log("Success, target added; requestID: ", data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
// Uncomment this line to run execution within this file.
// run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/eventbridge#code-examples)\. 
+  For API details, see [PutTargets](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-eventbridge/classes/puttargetscommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region
AWS.config.update({region: 'REGION'});

// Create CloudWatchEvents service object
var ebevents = new AWS.EventBridge({apiVersion: '2015-10-07'});

var params = {
  Rule: 'DEMO_EVENT',
  Targets: [
    {
      Arn: 'LAMBDA_FUNCTION_ARN',
      Id: 'myEventBridgeTarget',
    }
  ]
};

ebevents.putTargets(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/eventbridge#code-examples)\. 
+  For API details, see [PutTargets](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/eventbridge-2015-10-07/PutTargets) in *AWS SDK for JavaScript API Reference*\. 

### Create a scheduled rule<a name="eventbridge_PutRule_javascript_topic"></a>

The following code example shows how to create an Amazon EventBridge scheduled rule\.

**SDK for JavaScript V3**  
Create the client in a separate module and export it\.  

```
import { EventBridgeClient } from "@aws-sdk/client-eventbridge";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon EventBridge service client object.
export const ebClient = new EventBridgeClient({ region: REGION });
```
Import the SDK and client modules and call the API\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { PutRuleCommand } from "@aws-sdk/client-eventbridge";
import { ebClient } from "./libs/eventBridgeClient.js";

// Set the parameters.
export const params = {
  Name: "DEMO_EVENT",
  RoleArn: "IAM_ROLE_ARN", //IAM_ROLE_ARN
  ScheduleExpression: "rate(5 minutes)",
  State: "ENABLED",
};

export const run = async () => {
  try {
    const data = await ebClient.send(new PutRuleCommand(params));
    console.log("Success, scheduled rule created; Rule ARN:", data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
// Uncomment this line to run execution within this file.
// run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/eventbridge#code-examples)\. 
+  For API details, see [PutRule](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-eventbridge/classes/putrulecommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region
AWS.config.update({region: 'REGION'});

// Create CloudWatchEvents service object
var ebevents = new AWS.EventBridge({apiVersion: '2015-10-07'});

var params = {
  Name: 'DEMO_EVENT',
  RoleArn: 'IAM_ROLE_ARN',
  ScheduleExpression: 'rate(5 minutes)',
  State: 'ENABLED'
};

ebevents.putRule(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.RuleArn);
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/eventbridge#code-examples)\. 
+  For API details, see [PutRule](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/eventbridge-2015-10-07/PutRule) in *AWS SDK for JavaScript API Reference*\. 

### Send events<a name="eventbridge_PutEvents_javascript_topic"></a>

The following code example shows how to send Amazon EventBridge events\.

**SDK for JavaScript V3**  
Create the client in a separate module and export it\.  

```
import { EventBridgeClient } from "@aws-sdk/client-eventbridge";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon EventBridge service client object.
export const ebClient = new EventBridgeClient({ region: REGION });
```
Import the SDK and client modules and call the API\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { PutEventsCommand } from "@aws-sdk/client-eventbridge";
import { ebClient } from "./libs/eventBridgeClient.js";

// Set the parameters.
export const params = {
  Entries: [
    {
      Detail: '{ "key1": "value1", "key2": "value2" }',
      DetailType: "appRequestSubmitted",
      Resources: [
        "RESOURCE_ARN", //RESOURCE_ARN
      ],
      Source: "com.company.app",
    },
  ],
};

export const run = async () => {
  try {
    const data = await ebClient.send(new PutEventsCommand(params));
    console.log("Success, event sent; requestID:", data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
// Uncomment this line to run execution within this file.
// run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/eventbridge#code-examples)\. 
+  For API details, see [PutEvents](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-eventbridge/classes/puteventscommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region
AWS.config.update({region: 'REGION'});

// Create CloudWatchEvents service object
var ebevents = new AWS.EventBridge({apiVersion: '2015-10-07'});

var params = {
  Entries: [
    {
      Detail: '{ \"key1\": \"value1\", \"key2\": \"value2\" }',
      DetailType: 'appRequestSubmitted',
      Resources: [
        'RESOURCE_ARN',
      ],
      Source: 'com.company.app'
    }
  ]
};

ebevents.putEvents(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.Entries);
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/eventbridge#code-examples)\. 
+  For API details, see [PutEvents](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/eventbridge-2015-10-07/PutEvents) in *AWS SDK for JavaScript API Reference*\. 