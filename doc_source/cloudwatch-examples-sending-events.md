# Sending Events to Amazon CloudWatch Events<a name="cloudwatch-examples-sending-events"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create and update a rule used to trigger an event\.
+ How to define one or more targets to respond to an event\.
+ How to send events that are matched to targets for handling\.

## The Scenario<a name="cloudwatch-examples-sending-events-scenario"></a>

CloudWatch Events delivers a near real\-time stream of system events that describe changes in Amazon Web Services \(AWS\) resources to any of various targets\. Using simple rules, you can match events and route them to one or more target functions or streams\.

In this example, a series of Node\.js modules are used to send events to CloudWatch Events\. The Node\.js modules use the SDK for JavaScript to manage instances using these methods of the `CloudWatchEvents` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchEvents.html#putRule-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchEvents.html#putRule-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchEvents.html#putTargets-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchEvents.html#putTargets-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchEvents.html#putEvents-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatchEvents.html#putEvents-property)

For more information about CloudWatch Events, see [Adding Events with PutEvents](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/AddEventsPutEvents.html) in the *Amazon CloudWatch Events User Guide*\.

## Prerequisite Tasks<a name="cloudwatch-examples-sending-events-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.
+ Create an AWS Lambda function using the **hello\-world** blueprint to serve as the target for events\. To learn how, see [ Step 1: Create an AWS Lambda function](https://docs.aws.amazon.com/AmazonCloudWatch/latest/events/LogEC2InstanceState.html) in the *Amazon CloudWatch Events User Guide*\.
+ Create an IAM role whose policy grants permission to CloudWatch Events and that includes `events.amazonaws.com` as a trusted entity\. For more information about creating an IAM role, see [ Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

Use the following role policy when creating the IAM role\.

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Sid": "CloudWatchEventsFullAccess",
         "Effect": "Allow",
         "Action": "events:*",
         "Resource": "*"
      },
      {
         "Sid": "IAMPassRoleForCloudWatchEvents",
         "Effect": "Allow",
         "Action": "iam:PassRole",
         "Resource": "arn:aws:iam::*:role/AWS_Events_Invoke_Targets"
      }      
   ]
}
```

Use the following trust relationship when creating the IAM role\.

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Allow",
         "Principal": {
            "Service": "events.amazonaws.com"
         },
         "Action": "sts:AssumeRole"
      }      
   ]
}
```

## Creating a Scheduled Rule<a name="cloudwatch-examples-sending-events-rules"></a>

Create a Node\.js module with the file name `cwe_putrule.js`\. Be sure to configure the SDK as previously shown\. To access CloudWatch Events, create an `AWS.CloudWatchEvents` service object\. Create a JSON object containing the parameters needed to specify the new scheduled rule, which include the following: 
+ A name for the rule
+ The ARN of the IAM role you created previously
+ An expression to schedule triggering of the rule every five minutes

Call the `putRule` method to create the rule\. The callback returns the ARN of the new or updated rule\.

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

To run the example, type the following at the command line\.

```
node cwe_putrule.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/cloudwatch/cwe_putrule.js)\.

## Adding a Lambda Function Target<a name="cloudwatch-examples-sending-events-targets"></a>

Create a Node\.js module with the file name `cwe_puttargets.js`\. Be sure to configure the SDK as previously shown\. To access CloudWatch Events, create an `AWS.CloudWatchEvents` service object\. Create a JSON object containing the parameters needed to specify the rule to which you want to attach the target, including the ARN of the Lambda function you created\. Call the `putTargets` method of the `AWS.CloudWatchEvents` service object\.

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

To run the example, type the following at the command line\.

```
node cwe_puttargets.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/cloudwatch/cwe_puttargets.js)\.

## Sending Events<a name="cloudwatch-examples-sending-events-putevents"></a>

Create a Node\.js module with the file name `cwe_putevents.js`\. Be sure to configure the SDK as previously shown\. To access CloudWatch Events, create an `AWS.CloudWatchEvents` service object\. Create a JSON object containing the parameters needed to send events\. For each event, include the source of the event, the ARNs of any resources affected by the event, and details for the event\. Call the `putEvents` method of the `AWS.CloudWatchEvents` service object\.

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

To run the example, type the following at the command line\.

```
node cwe_putevents.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/cloudwatch/cwe_putevents.js)\.