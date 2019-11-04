# Creating and Using Amazon S3 Buckets<a name="s3-example-creating-buckets"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to obtain and display a list of Amazon S3 buckets in your account\.
+ How to create an Amazon S3 bucket\.
+ How to upload an object to a specified bucket\.

## The Scenario<a name="s3-example-creating-buckets-scenario"></a>

In this example, a series of Node\.js modules are used to obtain a list of existing Amazon S3 buckets, create a bucket, and upload a file to a specified bucket\. These Node\.js modules use the SDK for JavaScript to get information from and upload files to an Amazon S3 bucket using these methods of the Amazon S3 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#listBuckets-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#listBuckets-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#createBucket-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#createBucket-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#listObjects-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#listObjects-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#upload-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#upload-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#deleteBucket-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#deleteBucket-property)

## Prerequisite Tasks<a name="s3-example-creating-buckets-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Configuring the SDK<a name="s3-example-creating-buckets-configure-sdk"></a>

Configure the SDK for JavaScript by creating a global configuration object then setting the Region for your code\. In this example, the Region is set to `us-west-2`\.

```
// Load the SDK for JavaScript
var AWS = require('aws-sdk');
// Set the Region 
AWS.config.update({region: 'us-west-2'});
```

## Displaying a List of Amazon S3 Buckets<a name="s3-example-creating-buckets-list-buckets"></a>

Create a Node\.js module with the file name `s3_listbuckets.js`\. Make sure to configure the SDK as previously shown\. To access Amazon Simple Storage Service, create an `AWS.S3` service object\. Call the `listBuckets` method of the Amazon S3 service object to retrieve a list of your buckets\. The `data` parameter of the callback function has a `Buckets` property containing an array of maps to represent the buckets\. Display the bucket list by logging it to the console\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create S3 service object
s3 = new AWS.S3({apiVersion: '2006-03-01'});

// Call S3 to list the buckets
s3.listBuckets(function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.Buckets);
  }
});
```

To run the example, type the following at the command line\.

```
node s3_listbuckets.js
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/s3/s3_listbuckets.js)\.

## Creating an Amazon S3 Bucket<a name="s3-example-creating-buckets-new-bucket"></a>

Create a Node\.js module with the file name `s3_createbucket.js`\. Make sure to configure the SDK as previously shown\. Create an `AWS.S3` service object\. The module will take a single command\-line argument to specify a name for the new bucket\.

Add a variable to hold the parameters used to call the `createBucket` method of the Amazon S3 service object, including the name for the newly created bucket\. The callback function logs the new bucket's location to the console after Amazon S3 successfully creates it\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create S3 service object
s3 = new AWS.S3({apiVersion: '2006-03-01'});

// Create the parameters for calling createBucket
var bucketParams = {
  Bucket : process.argv[2],
  ACL : 'public-read'
};

// call S3 to create the bucket
s3.createBucket(bucketParams, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.Location);
  }
});
```

To run the example, type the following at the command line\.

```
node s3_createbucket.js BUCKET_NAME
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/s3/s3_createbucket.js)\.

## Uploading a File to an Amazon S3 Bucket<a name="s3-example-creating-buckets-upload-file"></a>

Create a Node\.js module with the file name `s3_upload.js`\. Make sure to configure the SDK as previously shown\. Create an `AWS.S3` service object\. The module will take two command\-line arguments, the first one to specify the destination bucket and the second to specify the file to upload\.

Create a variable with the parameters needed to call the `upload` method of the Amazon S3 service object\. Provide the name of the target bucket in the `Bucket` parameter\. The `Key` parameter is set to the name of the selected file, which you can obtain using the Node\.js `path` module\. The `Body` parameter is set to the contents of the file, which you can obtain using `createReadStream` from the Node\.js `fs` module\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create S3 service object
s3 = new AWS.S3({apiVersion: '2006-03-01'});

// call S3 to retrieve upload file to specified bucket
var uploadParams = {Bucket: process.argv[2], Key: '', Body: ''};
var file = process.argv[3];

// Configure the file stream and obtain the upload parameters
var fs = require('fs');
var fileStream = fs.createReadStream(file);
fileStream.on('error', function(err) {
  console.log('File Error', err);
});
uploadParams.Body = fileStream;
var path = require('path');
uploadParams.Key = path.basename(file);

// call S3 to retrieve upload file to specified bucket
s3.upload (uploadParams, function (err, data) {
  if (err) {
    console.log("Error", err);
  } if (data) {
    console.log("Upload Success", data.Location);
  }
});
```

To run the example, type the following at the command line\.

```
node s3_upload.js BUCKET_NAME FILE_NAME
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/s3/s3_upload.js)\.

## Listing Objects in an Amazon S3 Bucket<a name="s3-example-listing-objects"></a>

Create a Node\.js module with the file name `s3_listobjects.js`\. Make sure to configure the SDK as previously shown\. Create an `AWS.S3` service object\. 

Add a variable to hold the parameters used to call the `listObjects` method of the Amazon S3 service object, including the name of the bucket to read\. The callback function logs a list of objects \(files\) or a failure message\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create S3 service object
s3 = new AWS.S3({apiVersion: '2006-03-01'});

// Create the parameters for calling listObjects
var bucketParams = {
  Bucket : 'BUCKET_NAME',
};

// Call S3 to obtain a list of the objects in the bucket
s3.listObjects(bucketParams, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node s3_listobjects.js
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/s3/s3_listobjects.js)\.

## Deleting an Amazon S3 Bucket<a name="s3-example-deleting-buckets"></a>

Create a Node\.js module with the file name `s3_deletebucket.js`\. Make sure to configure the SDK as previously shown\. Create an `AWS.S3` service object\. 

Add a variable to hold the parameters used to call the `createBucket` method of the Amazon S3 service object, including the name of the bucket to delete\. The bucket must be empty in order to delete it\. The callback function logs a success or failure message\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region
AWS.config.update({region: 'REGION'});

// Create S3 service object
s3 = new AWS.S3({apiVersion: '2006-03-01'});

// Create params for S3.deleteBucket
var bucketParams = {
  Bucket : 'BUCKET_NAME'
};

// Call S3 to delete the bucket
s3.deleteBucket(bucketParams, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node s3_deletebucket.js
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/s3/s3_deletebucket.js)\.