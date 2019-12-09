# Creating Alarms in Amazon CloudWatch<a name="cloudwatch-examples-creating-alarms"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve basic information about your CloudWatch alarms\.
+ How to create and delete a CloudWatch alarm\.

## The Scenario<a name="cloudwatch-examples-creating-alarms-scenario"></a>

An alarm watches a single metric over a time period you specify, and performs one or more actions based on the value of the metric relative to a given threshold over a number of time periods\.

In this example, a series of Node\.js modules are used to create alarms in CloudWatch\. The Node\.js modules use the SDK for JavaScript to create alarms using these methods of the `AWS.CloudWatch` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#describeAlarms-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#describeAlarms-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#putMetricAlarm-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#putMetricAlarm-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#deleteAlarms-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#deleteAlarms-property)

For more information about CloudWatch alarms, see [Creating Amazon CloudWatch Alarms](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html) in the *Amazon CloudWatch User Guide*\.

## Prerequisite Tasks<a name="cloudwatch-examples-creating-alarms-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Describing Alarms<a name="cloudwatch-examples-creating-alarms-describing"></a>

Create a Node\.js module with the file name `cw_describealarms.js`\. Be sure to configure the SDK as previously shown\. To access CloudWatch, create an `AWS.CloudWatch` service object\. Create a JSON object to hold the parameters for retrieving alarm descriptions, limiting the alarms returned to those with a state of `INSUFFICIENT_DATA`\. Then call the `describeAlarms` method of the `AWS.CloudWatch` service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create CloudWatch service object
var cw = new AWS.CloudWatch({apiVersion: '2010-08-01'});

cw.describeAlarms({StateValue: 'INSUFFICIENT_DATA'}, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    // List the names of all current alarms in the console
    data.MetricAlarms.forEach(function (item, index, array) {
       console.log(item.AlarmName);
    });
  }
});
```

To run the example, type the following at the command line\.

```
node cw_describealarms.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/cloudwatch/cw_describealarms.js)\. 

## Creating an Alarm for a CloudWatch Metric<a name="cloudwatch-examples-creating-alarms-putmetricalarm"></a>

Create a Node\.js module with the file name `cw_putmetricalarm.js`\. Be sure to configure the SDK as previously shown\. To access CloudWatch, create an `AWS.CloudWatch` service object\. Create a JSON object for the parameters needed to create an alarm based on a metric, in this case the CPU utilization of an Amazon EC2 instance\. The remaining parameters are set so the alarm triggers when the metric exceeds a threshold of 70 percent\. Then call the `describeAlarms` method of the `AWS.CloudWatch` service object\.

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
  ActionsEnabled: false,
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
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node cw_putmetricalarm.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/cloudwatch/cw_putmetricalarm.js)\.

## Deleting an Alarm<a name="cloudwatch-examples-creating-alarms-deleting"></a>

Create a Node\.js module with the file name `cw_deletealarms.js`\. Be sure to configure the SDK as previously shown\. To access CloudWatch, create an `AWS.CloudWatch` service object\. Create a JSON object to hold the names of the alarms you want to delete\. Then call the `deleteAlarms` method of the `AWS.CloudWatch` service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create CloudWatch service object
var cw = new AWS.CloudWatch({apiVersion: '2010-08-01'});

var params = {
  AlarmNames: ['Web_Server_CPU_Utilization']
};

cw.deleteAlarms(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node cw_deletealarms.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/cloudwatch/cw_deletealarms.js)\.