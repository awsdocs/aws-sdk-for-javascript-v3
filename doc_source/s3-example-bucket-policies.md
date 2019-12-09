# Working with Amazon S3 Bucket Policies<a name="s3-example-bucket-policies"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve the bucket policy of an Amazon S3 bucket\.
+ How to add or update the bucket policy of an Amazon S3 bucket\.
+ How to delete the bucket policy of an Amazon S3 bucket\.

## The Scenario<a name="w6aac18c25c25c15b9"></a>

In this example, a series of Node\.js modules are used to retrieve, set, or delete a bucket policy on an Amazon S3 bucket\. The Node\.js modules use the SDK for JavaScript to configure policy for a selected Amazon S3 bucket using these methods of the Amazon S3 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getBucketPolicy-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getBucketPolicy-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putBucketPolicy-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putBucketPolicy-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#deleteBucketPolicy-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#deleteBucketPolicy-property)

For more information about bucket policies for Amazon S3 buckets, see [ Using Bucket Policies and User Policies](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-iam-policies.html) in the *Amazon Simple Storage Service Developer Guide*\.

## Prerequisite Tasks<a name="w6aac18c25c25c15c11"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Configuring the SDK<a name="s3-example-bucket-policies-configure-sdk"></a>

Configure the SDK for JavaScript by creating a global configuration object then setting the Region for your code\. In this example, the Region is set to `us-west-2`\.

```
// Load the SDK for JavaScript
var AWS = require('aws-sdk');
// Set the Region 
AWS.config.update({region: 'us-west-2'});
```

## Retrieving the Current Bucket Policy<a name="s3-example-bucket-policies-get-policy"></a>

Create a Node\.js module with the file name `s3_getbucketpolicy.js`\. The module takes a single command\-line argument that specifies the bucket whose policy you want\. Make sure to configure the SDK as previously shown\. 

Create an `AWS.S3` service object\. The only parameter you need to pass is the name of the selected bucket when calling the `getBucketPolicy` method\. If the bucket currently has a policy, that policy is returned by Amazon S3 in the `data` parameter passed to the callback function\.

If the selected bucket has no policy, that information is returned to the callback function in the `error` parameter\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create S3 service object
s3 = new AWS.S3({apiVersion: '2006-03-01'});

var bucketParams = {Bucket: process.argv[2]};
// call S3 to retrieve policy for selected bucket
s3.getBucketPolicy(bucketParams, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else if (data) {
    console.log("Success", data.Policy);
  }
});
```

To run the example, type the following at the command line\.

```
node s3_getbucketpolicy.js BUCKET_NAME
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/s3/s3_getbucketpolicy.js)\.

## Setting a Simple Bucket Policy<a name="s3-example-bucket-policies-set-policy"></a>

Create a Node\.js module with the file name `s3_setbucketpolicy.js`\. The module takes a single command\-line argument that specifies the bucket whose policy you want to apply\. Configure the SDK as previously shown\. 

Create an `AWS.S3` service object\. Bucket policies are specified in JSON\. First, create a JSON object that contains all of the values to specify the policy except for the `Resource` value that identifies the bucket\.

Format the `Resource` string required by the policy, incorporating the name of the selected bucket\. Insert that string into the JSON object\. Prepare the parameters for the `putBucketPolicy` method, including the name of the bucket and the JSON policy converted to a string value\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create S3 service object
s3 = new AWS.S3({apiVersion: '2006-03-01'});

var readOnlyAnonUserPolicy = {
  Version: "2012-10-17",
  Statement: [
    {
      Sid: "AddPerm",
      Effect: "Allow",
      Principal: "*",
      Action: [
        "s3:GetObject"
      ],
      Resource: [
        ""
      ]
    }
  ]
};

// create selected bucket resource string for bucket policy
var bucketResource = "arn:aws:s3:::" + process.argv[2] + "/*";
readOnlyAnonUserPolicy.Statement[0].Resource[0] = bucketResource;

// convert policy JSON into string and assign into params
var bucketPolicyParams = {Bucket: process.argv[2], Policy: JSON.stringify(readOnlyAnonUserPolicy)};

// set the new policy on the selected bucket
s3.putBucketPolicy(bucketPolicyParams, function(err, data) {
  if (err) {
    // display error message
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node s3_setbucketpolicy.js BUCKET_NAME
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/s3/s3_setbucketpolicy.js)\.

## Deleting a Bucket Policy<a name="s3-example-bucket-policies-delete-policy"></a>

Create a Node\.js module with the file name `s3_deletebucketpolicy.js`\. The module takes a single command\-line argument that specifies the bucket whose policy you want to delete\. Configure the SDK as previously shown\.

 Create an `AWS.S3` service object\. The only parameter you need to pass when calling the `deleteBucketPolicy` method is the name of the selected bucket\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create S3 service object
s3 = new AWS.S3({apiVersion: '2006-03-01'});

var bucketParams = {Bucket: process.argv[2]};
// call S3 to delete policy for selected bucket
s3.deleteBucketPolicy(bucketParams, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else if (data) {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node s3_deletebucketpolicy.js BUCKET_NAME
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/s3/s3_deletebucketpolicy.js)\.