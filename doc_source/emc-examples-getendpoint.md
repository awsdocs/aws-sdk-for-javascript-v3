# Getting Your Account\-Specific Endpoint for MediaConvert<a name="emc-examples-getendpoint"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve your account\-specific endpoint from MediaConvert\.

## The Scenario<a name="emc-example-getendpoint-scenario"></a>

In this example, you use a Node\.js module to call MediaConvert and retrieve your account\-specific endpoint\. You can retrieve your endpoint URL from the service default endpoint and so do not yet need your account\-specific endpoint\. The code uses the SDK for JavaScript to retrieve this endpoint by using this method of the MediaConvert client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#describeEndpoints-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#describeEndpoints-property)

## Prerequisite Tasks<a name="emc-example-getendpoint-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Install Node\.js\. For more information, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.
+ Create an IAM role that gives MediaConvert access to your input files and the Amazon S3 buckets where your output files are stored\. For details, see [Set Up IAM Permissions](https://docs.aws.amazon.com/mediaconvert/latest/ug/iam-role.html) in the *AWS Elemental MediaConvert User Guide*\.

## Getting Your Endpoint URL<a name="emc-example-getendpoint-url"></a>

Create a Node\.js module with the file name `emc_getendpoint.js`\. Be sure to configure the SDK as previously shown\.

Create an object to pass the empty request parameters for the `describeEndpoints` method of the AWS\.MediaConvert client class\. To call the `describeEndpoints` method, create a promise for invoking an MediaConvert service object, passing the parameters\. Handle the response in the promise callback\. 

```
// Load the SDK for JavaScript
var AWS = require('aws-sdk');
// Set the Region 
AWS.config.update({region: 'us-west-2'});

// Create empty request parameters
var params = {
  MaxResults: 0,
};

// Create a promise on a MediaConvert object
var endpointPromise = new AWS.MediaConvert({apiVersion: '2017-08-29'}).describeEndpoints(params).promise();


endpointPromise.then(
  function(data) {
    console.log("Your MediaConvert endpoint is ", data.Endpoints);
  },
  function(err) {
    console.log("Error", err);
  }
);
```

To run the example, type the following at the command line\.

```
node emc_getendpoint.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/mediaconvert/emc_getendpoint.js)\.