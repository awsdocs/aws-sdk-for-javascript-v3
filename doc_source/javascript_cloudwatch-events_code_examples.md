--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# CloudWatch Events examples using SDK for JavaScript \(v3\)<a name="javascript_cloudwatch-events_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for JavaScript \(v3\) with CloudWatch Events\.

*Actions* are code excerpts that show you how to call individual service functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple functions within the same service\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#actions)

## Actions<a name="actions"></a>

### Adding a target<a name="cloudwatch-events_PutTargets_javascript_topic"></a>

The following code example shows how to add a target to an Amazon CloudWatch Events event\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cloudwatch-events#code-examples)\. 
Create the client in a separate module and export it\.  

```
import { CloudWatchEventsClient } from "@aws-sdk/client-cloudwatch-events";
import { DEFAULT_REGION } from "libs/utils/util-aws-sdk.js";

export const client = new CloudWatchEventsClient({ region: DEFAULT_REGION });
```
Import the SDK and client modules and call the API\.  

```
import { PutTargetsCommand } from "@aws-sdk/client-cloudwatch-events";
import { client } from "../libs/client.js";

const run = async () => {
  const command = new PutTargetsCommand({
    // The name of the Amazon CloudWatch Events rule.
    Rule: process.env.CLOUDWATCH_EVENTS_RULE,

    // The targets to add to the rule.
    Targets: [
      {
        Arn: process.env.CLOUDWATCH_EVENTS_TARGET_ARN,
        // The ID of the target. Choose a unique ID for each target.
        Id: process.env.CLOUDWATCH_EVENTS_TARGET_ID,
      },
    ],
  });

  try {
    return await client.send(command);
  } catch (err) {
    console.error(err);
  }
};

export default run();
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/cloudwatch-examples-sending-events.html#cloudwatch-examples-sending-events-targets)\. 
+  For API details, see [PutTargets](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch-events/classes/puttargetscommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript \(v2\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/cloudwatch-events#code-examples)\. 
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create CloudWatchEvents service object
var cwevents = new AWS.CloudWatchEvents({apiVersion: '2015-10-07'});

var params = {
  Rule: 'DEMO_EVENT',
  Targets: [
    {
      Arn: 'LAMBDA_FUNCTION_ARN',
      Id: 'myCloudWatchEventsTarget',
    }
  ]
};

cwevents.putTargets(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/cloudwatch-examples-sending-events.html#cloudwatch-examples-sending-events-targets)\. 
+  For API details, see [PutTargets](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/monitoring-2010-08-01/PutTargets) in *AWS SDK for JavaScript API Reference*\. 

### Create a scheduled rule<a name="cloudwatch-events_PutRule_javascript_topic"></a>

The following code example shows how to create an Amazon CloudWatch Events scheduled rule\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cloudwatch-events#code-examples)\. 
Create the client in a separate module and export it\.  

```
import { CloudWatchEventsClient } from "@aws-sdk/client-cloudwatch-events";
import { DEFAULT_REGION } from "libs/utils/util-aws-sdk.js";

export const client = new CloudWatchEventsClient({ region: DEFAULT_REGION });
```
Import the SDK and client modules and call the API\.  

```
import { PutRuleCommand } from "@aws-sdk/client-cloudwatch-events";
import { client } from "../libs/client.js";

const run = async () => {
  // Request parameters for PutRule.
  // https://docs.aws.amazon.com/eventbridge/latest/APIReference/API_PutRule.html#API_PutRule_RequestParameters
  const command = new PutRuleCommand({
    Name: process.env.CLOUDWATCH_EVENTS_RULE,

    // The event pattern for the rule.
    //  Example: {"source": ["my.app"]}
    EventPattern: process.env.CLOUDWATCH_EVENTS_RULE_PATTERN,

    // The state of the rule. Valid values: ENABLED, DISABLED
    State: "ENABLED",
  });

  try {
    return await client.send(command);
  } catch (err) {
    console.error(err);
  }
};

export default run();
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/cloudwatch-examples-sending-events.html#cloudwatch-examples-sending-events-rules)\. 
+  For API details, see [PutRule](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch-events/classes/putrulecommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript \(v2\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/cloudwatch-events#code-examples)\. 
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create CloudWatchEvents service object
var cwevents = new AWS.CloudWatchEvents({apiVersion: '2015-10-07'});

var params = {
  Name: 'DEMO_EVENT',
  RoleArn: 'IAM_ROLE_ARN',
  ScheduleExpression: 'rate(5 minutes)',
  State: 'ENABLED'
};

cwevents.putRule(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.RuleArn);
  }
});
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/cloudwatch-examples-sending-events.html#cloudwatch-examples-sending-events-rules)\. 
+  For API details, see [PutRule](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/monitoring-2010-08-01/PutRule) in *AWS SDK for JavaScript API Reference*\. 

### Send events<a name="cloudwatch-events_PutEvents_javascript_topic"></a>

The following code example shows how to send Amazon CloudWatch Events events\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cloudwatch-events#code-examples)\. 
Create the client in a separate module and export it\.  

```
import { CloudWatchEventsClient } from "@aws-sdk/client-cloudwatch-events";
import { DEFAULT_REGION } from "libs/utils/util-aws-sdk.js";

export const client = new CloudWatchEventsClient({ region: DEFAULT_REGION });
```
Import the SDK and client modules and call the API\.  

```
import { PutEventsCommand } from "@aws-sdk/client-cloudwatch-events";
import { client } from "../libs/client.js";

const run = async () => {
  const command = new PutEventsCommand({
    // The list of events to send to Amazon CloudWatch Events.
    Entries: [
      {
        // The name of the application or service that is sending the event.
        Source: "my.app",

        // The name of the event that is being sent.
        DetailType: "My Custom Event",

        // The data that is sent with the event.
        Detail: JSON.stringify({ timeOfEvent: new Date().toISOString() }),
      },
    ],
  });

  try {
    return await client.send(command);
  } catch (err) {
    console.error(err);
  }
};

export default run();
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/cloudwatch-examples-sending-events.html#cloudwatch-examples-sending-events-putevents)\. 
+  For API details, see [PutEvents](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch-events/classes/puteventscommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript \(v2\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/cloudwatch-events#code-examples)\. 
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create CloudWatchEvents service object
var cwevents = new AWS.CloudWatchEvents({apiVersion: '2015-10-07'});

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

cwevents.putEvents(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.Entries);
  }
});
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/cloudwatch-examples-sending-events.html#cloudwatch-examples-sending-events-putevents)\. 
+  For API details, see [PutEvents](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/monitoring-2010-08-01/PutEvents) in *AWS SDK for JavaScript API Reference*\. 