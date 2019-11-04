# Using an Amazon S3 Bucket as a Static Web Host<a name="s3-example-static-web-host"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to set up an Amazon S3 bucket as a static web host\.

## The Scenario<a name="s3-example-static-web-host-scenario"></a>

In this example, a series of Node\.js modules are used to configure any of your buckets to act as a static web host\. The Node\.js modules use the SDK for JavaScript to configure a selected Amazon S3 bucket using these methods of the Amazon S3 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getBucketWebsite-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getBucketWebsite-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putBucketWebsite-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putBucketWebsite-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#deleteBucketWebsite-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#deleteBucketWebsite-property)

For more information about using an Amazon S3 bucket as a static web host, see [Hosting a Static Website on Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html) in the *Amazon Simple Storage Service Developer Guide*\.

## Prerequisite Tasks<a name="s3-example-static-web-host-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Configuring the SDK<a name="s3-example-static-web-host-configure-sdk"></a>

Configure the SDK for JavaScript by creating a global configuration object then setting the Region for your code\. In this example, the Region is set to `us-west-2`\.

```
// Load the SDK for JavaScript
var AWS = require('aws-sdk');
// Set the Region 
AWS.config.update({region: 'us-west-2'});
```

## Retrieving the Current Bucket Website Configuration<a name="s3-example-static-web-host-get-website"></a>

Create a Node\.js module with the file name `s3_getbucketwebsite.js`\. The module takes a single command\-line argument that specifies the bucket whose website configuration you want\. Configure the SDK as previously shown\.

Create an `AWS.S3` service object\. Create a function that retrieves the current bucket website configuration for the bucket selected in the bucket list\. The only parameter you need to pass is the name of the selected bucket when calling the `getBucketWebsite` method\. If the bucket currently has a website configuration, that configuration is returned by Amazon S3 in the `data` parameter passed to the callback function\.

If the selected bucket has no website configuration, that information is returned to the callback function in the `err` parameter\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create S3 service object
s3 = new AWS.S3({apiVersion: '2006-03-01'});

var bucketParams = {Bucket: process.argv[2]};

// call S3 to retrieve the website configuration for selected bucket
s3.getBucketWebsite(bucketParams, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else if (data) {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node s3_getbucketwebsite.js BUCKET_NAME
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/s3/s3_getbucketwebsite.js)\.

## Setting a Bucket Website Configuration<a name="s3-example-static-web-host-set-website"></a>

Create a Node\.js module with the file name `s3_setbucketwebsite.js`\. Make sure to configure the SDK as previously shown\. Create an `AWS.S3` service object\. 

Create a function that applies a bucket website configuration\. The configuration allows the selected bucket to serve as a static web host\. Website configurations are specified in JSON\. First, create a JSON object that contains all the values to specify the website configuration, except for the `Key` value that identifies the error document, and the `Suffix` value that identifies the index document\.

Insert the values of the text input elements into the JSON object\. Prepare the parameters for the `putBucketWebsite` method, including the name of the bucket and the JSON website configuration\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create S3 service object
s3 = new AWS.S3({apiVersion: '2006-03-01'});

// Create JSON for putBucketWebsite parameters
var staticHostParams = {
  Bucket: '',
  WebsiteConfiguration: {
    ErrorDocument: {
      Key: ''
    },
    IndexDocument: {
      Suffix: ''
    },
  }
};

// Insert specified bucket name and index and error documents into params JSON
// from command line arguments
staticHostParams.Bucket = process.argv[2];
staticHostParams.WebsiteConfiguration.IndexDocument.Suffix = process.argv[3];
staticHostParams.WebsiteConfiguration.ErrorDocument.Key = process.argv[4];

// set the new website configuration on the selected bucket
s3.putBucketWebsite(staticHostParams, function(err, data) {
  if (err) {
    // display error message
    console.log("Error", err);
  } else {
    // update the displayed website configuration for the selected bucket
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node s3_setbucketwebsite.js BUCKET_NAME INDEX_PAGE ERROR_PAGE
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/s3/s3_setbucketwebsite.js)\.

## Deleting a Bucket Website Configuration<a name="s3-example-static-web-host-delete-website"></a>

Create a Node\.js module with the file name `s3_deletebucketwebsite.js`\. Make sure to configure the SDK as previously shown\. Create an `AWS.S3` service object\. 

Create a function that deletes the website configuration for the selected bucket\. The only parameter you need to pass when calling the `deleteBucketWebsite` method is the name of the selected bucket\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create S3 service object
s3 = new AWS.S3({apiVersion: '2006-03-01'});

var bucketParams = {Bucket: process.argv[2]};

// call S3 to delete website configuration for selected bucket
s3.deleteBucketWebsite(bucketParams, function(error, data) {
  if (error) {
    console.log("Error", err);
  } else if (data) {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node s3_deletebucketwebsite.js BUCKET_NAME
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/s3/s3_deletebucketwebsite.js)\.