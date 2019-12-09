# Using Alarm Actions in Amazon CloudWatch<a name="cloudwatch-examples-using-alarm-actions"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to change the state of your Amazon EC2 instances automatically based on a CloudWatch alarm\.

## The Scenario<a name="cloudwatch-examples-using-alarm-actions-scenario"></a>

Using alarm actions, you can create alarms that automatically stop, terminate, reboot, or recover your Amazon EC2 instances\. You can use the stop or terminate actions when you no longer need an instance to be running\. You can use the reboot and recover actions to automatically reboot those instances\. 

In this example, a series of Node\.js modules are used to define an alarm action in CloudWatch that triggers the reboot of an Amazon EC2 instance\. The Node\.js modules use the SDK for JavaScript to manage Amazon EC2 instances using these methods of the `CloudWatch` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#enableAlarmActions-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#enableAlarmActions-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#disableAlarmActions-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#disableAlarmActions-property)

For more information about CloudWatch alarm actions, see [Create Alarms to Stop, Terminate, Reboot, or Recover an Instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/UsingAlarmActions.html) in the *Amazon CloudWatch User Guide*\.

## Prerequisite Tasks<a name="cloudwatch-examples-using-alarm-actions-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](http://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.
+ Create an IAM role whose policy grants permission to describe, reboot, stop, or terminate an Amazon EC2 instance\. For more information about creating an IAM role, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

Use the following role policy when creating the IAM role\.

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Allow",
         "Action": [
            "cloudwatch:Describe*",
            "ec2:Describe*",
            "ec2:RebootInstances",
            "ec2:StopInstances*",
            "ec2:TerminateInstances"
         ],
         "Resource": [
            "*"
         ]
      }
   ]
}
```

## <a name="cloudwatch-examples-using-alarm-actions-configure-sdk"></a>

Configure the SDK for JavaScript by creating a global configuration object then setting the AWS Region for your code\. In this example, the Region is set to `us-west-2`\.

```
// Load the SDK for JavaScript
var AWS = require('aws-sdk');
// Set the Region 
AWS.config.update({region: 'us-west-2'});
```

## Creating and Enabling Actions on an Alarm<a name="cloudwatch-examples-using-alarm-actions-enabling"></a>

Create a Node\.js module with the file name `cw_enablealarmactions.js`\. Be sure to configure the SDK as previously shown\. To access CloudWatch, create an `AWS.CloudWatch` service object\.

Create a JSON object to hold the parameters for creating an alarm, specifying `ActionsEnabled` as `true` and an array of ARNs for the actions the alarm will trigger\. Call the `putMetricAlarm` method of the `AWS.CloudWatch` service object, which creates the alarm if it does not exist or updates it if the alarm does exist\.

In the callback function for the `putMetricAlarm`, upon successful completion create a JSON object containing the name of the CloudWatch alarm\. Call the `enableAlarmActions` method to enable the alarm action\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create CloudWatch service object
var cw = new AWS.CloudWatch({apiVersion: '2010-08-01'});

var params = {
  AlarmName: 'Web_Server_CPU_Utilization',
  ComparisonOperator: 'GreaterThanThreshold',
  EvaluationPeriods: 1,
  MetricName: 'CPUUtilization',
  Namespace: 'AWS/EC2',
  Period: 60,
  Statistic: 'Average',
  Threshold: 70.0,
  ActionsEnabled: true,
  AlarmActions: ['ACTION_ARN'],
  AlarmDescription: 'Alarm when server CPU exceeds 70%',
  Dimensions: [
    {
      Name: 'InstanceId',
      Value: 'INSTANCE_ID'
    },
  ],
  Unit: 'Percent'
};

cw.putMetricAlarm(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Alarm action added", data);
    var paramsEnableAlarmAction = {
      AlarmNames: [params.AlarmName]
    };
    cw.enableAlarmActions(paramsEnableAlarmAction, function(err, data) {
      if (err) {
        console.log("Error", err);
      } else {
        console.log("Alarm action enabled", data);
      }
    });
  }
});
```

To run the example, type the following at the command line\.

```
node cw_enablealarmactions.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/cloudwatch/cw_enablealarmactions.js)\.

## Disabling Actions on an Alarm<a name="cloudwatch-examples-using-alarm-actions-disabling"></a>

Create a Node\.js module with the file name `cw_disablealarmactions.js`\. Be sure to configure the SDK as previously shown\. To access CloudWatch, create an `AWS.CloudWatch` service object\. Create a JSON object containing the name of the CloudWatch alarm\. Call the `disableAlarmActions` method to disable the actions for this alarm\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create CloudWatch service object
var cw = new AWS.CloudWatch({apiVersion: '2010-08-01'});

cw.disableAlarmActions({AlarmNames: ['Web_Server_CPU_Utilization']}, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node cw_disablealarmactions.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/cloudwatch/cw_disablealarmactions.js)\.