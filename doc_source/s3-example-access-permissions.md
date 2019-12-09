# Managing Amazon S3 Bucket Access Permissions<a name="s3-example-access-permissions"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve or set the access control list for an Amazon S3 bucket\.

## The Scenario<a name="w6aac18c25c25c13b9"></a>

In this example, a Node\.js module is used to display the bucket access control list \(ACL\) for a selected bucket and apply changes to the ACL for a selected bucket\. The Node\.js module uses the SDK for JavaScript to manage Amazon S3 bucket access permissions using these methods of the Amazon S3 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getBucketAcl-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getBucketAcl-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putBucketAcl-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putBucketAcl-property)

For more information about access control lists for Amazon S3 buckets, see [ Managing Access with ACLs](https://docs.aws.amazon.com/AmazonS3/latest/dev/S3_ACLs_UsingACLs.html) in the *Amazon Simple Storage Service Developer Guide*\.

## Prerequisite Tasks<a name="w6aac18c25c25c13c11"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Configuring the SDK<a name="s3-example-access-permissions-configure-sdk"></a>

Configure the SDK for JavaScript by creating a global configuration object then setting the Region for your code\. In this example, the Region is set to `us-west-2`\.

```
// Load the SDK for JavaScript
var AWS = require('aws-sdk');
// Set the Region 
AWS.config.update({region: 'us-west-2'});
```

## Retrieving the Current Bucket Access Control List<a name="s3-example-access-permissions-get-acl"></a>

Create a Node\.js module with the file name `s3_getbucketacl.js`\. The module will take a single command\-line argument to specify the bucket whose ACL configuration you want\. Make sure to configure the SDK as previously shown\. 

Create an `AWS.S3` service object\. The only parameter you need to pass is the name of the selected bucket when calling the `getBucketAcl` method\. The current access control list configuration is returned by Amazon S3 in the `data` parameter passed to the callback function\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create S3 service object
s3 = new AWS.S3({apiVersion: '2006-03-01'});

var bucketParams = {Bucket: process.argv[2]};
// call S3 to retrieve policy for selected bucket
s3.getBucketAcl(bucketParams, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else if (data) {
    console.log("Success", data.Grants);
  }
});
```

To run the example, type the following at the command line\.

```
node s3_getbucketacl.js BUCKET_NAME
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/s3/s3_getbucketacl.js)\.