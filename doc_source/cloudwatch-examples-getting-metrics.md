# Getting Metrics from Amazon CloudWatch<a name="cloudwatch-examples-getting-metrics"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve a list of published CloudWatch metrics\.
+ How to publish data points to CloudWatch metrics\.

## The Scenario<a name="cloudwatch-examples-getting-metrics-scenario"></a>

Metrics are data about the performance of your systems\. You can enable detailed monitoring of some resources, such as your Amazon EC2 instances, or your own application metrics\. 

In this example, a series of Node\.js modules are used to get metrics from CloudWatch and to send events to Amazon CloudWatch Events\. The Node\.js modules use the SDK for JavaScript to get metrics from CloudWatch using these methods of the `CloudWatch` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#listMetrics-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#listMetrics-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#putMetricData-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CloudWatch.html#putMetricData-property)

For more information about CloudWatch metrics, see [Using Amazon CloudWatch Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/working_with_metrics.html) in the *Amazon CloudWatch User Guide*\.

## Prerequisite Tasks<a name="cloudwatch-examples-getting-metrics-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Listing Metrics<a name="cloudwatch-examples-getting-metrics-listing"></a>

Create a Node\.js module with the file name `cw_listmetrics.js`\. Be sure to configure the SDK as previously shown\. To access CloudWatch, create an `AWS.CloudWatch` service object\. Create a JSON object containing the parameters needed to list metrics within the `AWS/Logs` namespace\. Call the `listMetrics` method to list the `IncomingLogEvents` metric\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create CloudWatch service object
var cw = new AWS.CloudWatch({apiVersion: '2010-08-01'});

var params = {
  Dimensions: [
    {
      Name: 'LogGroupName', /* required */
    },
  ],
  MetricName: 'IncomingLogEvents',
  Namespace: 'AWS/Logs'
};

cw.listMetrics(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Metrics", JSON.stringify(data.Metrics));
  }
});
```

To run the example, type the following at the command line\.

```
node cw_listmetrics.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/cloudwatch/cw_listmetrics.js)\.

## Submitting Custom Metrics<a name="cloudwatch-examples-getting-metrics-publishing-custom"></a>

Create a Node\.js module with the file name `cw_putmetricdata.js`\. Be sure to configure the SDK as previously shown\. To access CloudWatch, create an `AWS.CloudWatch` service object\. Create a JSON object containing the parameters needed to submit a data point for the `PAGES_VISITED` custom metric\. Call the `putMetricData` method\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create CloudWatch service object
var cw = new AWS.CloudWatch({apiVersion: '2010-08-01'});

// Create parameters JSON for putMetricData
var params = {
  MetricData: [
    {
      MetricName: 'PAGES_VISITED',
      Dimensions: [
        {
          Name: 'UNIQUE_PAGES',
          Value: 'URLS'
        },
      ],
      Unit: 'None',
      Value: 1.0
    },
  ],
  Namespace: 'SITE/TRAFFIC'
};

cw.putMetricData(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", JSON.stringify(data));
  }
});
```

To run the example, type the following at the command line\.

```
node cw_putmetricdata.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/cloudwatch/cw_putmetricdata.js)\.