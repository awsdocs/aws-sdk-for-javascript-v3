# Configuring Amazon S3 Buckets<a name="s3-example-configuring-buckets"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to configure the cross\-origin resource sharing \(CORS\) permissions for a bucket\.

## The Scenario<a name="s3-example-configuring-buckets-scenario"></a>

In this example, a series of Node\.js modules are used to list your Amazon S3 buckets and to configure CORS and bucket logging\. The Node\.js modules use the SDK for JavaScript to configure a selected Amazon S3 bucket using these methods of the Amazon S3 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getBucketCors-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getBucketCors-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putBucketCors-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putBucketCors-property)

For more information about using CORS configuration with an Amazon S3 bucket, see [Cross\-Origin Resource Sharing \(CORS\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/cors.html) in the *Amazon Simple Storage Service Developer Guide*\.

## Prerequisite Tasks<a name="s3-example-configuring-buckets-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Configuring the SDK<a name="s3-example-configuring-buckets-configure-sdk"></a>

Configure the SDK for JavaScript by creating a global configuration object then setting the Region for your code\. In this example, the Region is set to `us-west-2`\.

```
// Load the SDK for JavaScript
var AWS = require('aws-sdk');
// Set the Region 
AWS.config.update({region: 'us-west-2'});
```

## Retrieving a Bucket CORS Configuration<a name="s3-example-configuring-buckets-get-cors"></a>

Create a Node\.js module with the file name `s3_getcors.js`\. The module will take a single command\-line argument to specify the bucket whose CORS configuration you want\. Make sure to configure the SDK as previously shown\. Create an `AWS.S3` service object\. 

The only parameter you need to pass is the name of the selected bucket when calling the `getBucketCors` method\. If the bucket currently has a CORS configuration, that configuration is returned by Amazon S3 as the `CORSRules` property of the `data` parameter passed to the callback function\.

If the selected bucket has no CORS configuration, that information is returned to the callback function in the `error` parameter\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create S3 service object
s3 = new AWS.S3({apiVersion: '2006-03-01'});

// Set the parameters for S3.getBucketCors
var bucketParams = {Bucket: process.argv[2]};

// call S3 to retrieve CORS configuration for selected bucket
s3.getBucketCors(bucketParams, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else if (data) {
    console.log("Success", JSON.stringify(data.CORSRules));
  }
});
```

To run the example, type the following at the command line\.

```
node s3_getcors.js BUCKET_NAME
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/s3/s3_getcors.js)\.

## Setting a Bucket CORS Configuration<a name="s3-example-configuring-buckets-put-cors"></a>

Create a Node\.js module with the file name `s3_setcors.js`\. The module takes multiple command\-line arguments, the first of which specifies the bucket whose CORS configuration you want to set\. Additional arguments enumerate the HTTP methods \(POST, GET, PUT, PATCH, DELETE, POST\) you want to allow for the bucket\. Configure the SDK as previously shown\.

 Create an `AWS.S3` service object\. Next create a JSON object to hold the values for the CORS configuration as required by the `putBucketCors` method of the `AWS.S3` service object\. Specify `"Authorization"` for the `AllowedHeaders` value and `"*"` for the `AllowedOrigins` value\. Set the value of `AllowedMethods` as empty array initially\.

Specify the allowed methods as command line parameters to the Node\.js module, adding each of the methods that match one of the parameters\. Add the resulting CORS configuration to the array of configurations contained in the `CORSRules` parameter\. Specify the bucket you want to configure for CORS in the `Bucket` parameter\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create S3 service object
s3 = new AWS.S3({apiVersion: '2006-03-01'});

// Create initial parameters JSON for putBucketCors
var thisConfig = {
  AllowedHeaders:["Authorization"],
  AllowedMethods:[],
  AllowedOrigins:["*"],
  ExposeHeaders:[],
  MaxAgeSeconds:3000
};

// Assemble the list of allowed methods based on command line parameters
var allowedMethods = [];
process.argv.forEach(function (val, index, array) {
  if (val.toUpperCase() === "POST") {allowedMethods.push("POST")};
  if (val.toUpperCase() === "GET") {allowedMethods.push("GET")};
  if (val.toUpperCase() === "PUT") {allowedMethods.push("PUT")};
  if (val.toUpperCase() === "PATCH") {allowedMethods.push("PATCH")};
  if (val.toUpperCase() === "DELETE") {allowedMethods.push("DELETE")};
  if (val.toUpperCase() === "HEAD") {allowedMethods.push("HEAD")};
});

// Copy the array of allowed methods into the config object
thisConfig.AllowedMethods = allowedMethods;
// Create array of configs then add the config object to it
var corsRules = new Array(thisConfig);

// Create CORS params
var corsParams = {Bucket: process.argv[2], CORSConfiguration: {CORSRules: corsRules}};

// set the new CORS configuration on the selected bucket
s3.putBucketCors(corsParams, function(err, data) {
  if (err) {
    // display error message
    console.log("Error", err);
  } else {
    // update the displayed CORS config for the selected bucket
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line including one or more HTTP methods as shown\.

```
node s3_setcors.js BUCKET_NAME get put
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/s3/s3_setcors.js)\.