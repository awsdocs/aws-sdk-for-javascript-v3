--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Managing Amazon S3 bucket access permissions<a name="s3-example-access-permissions"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve or set the access control list for an Amazon S3 bucket\.

## The scenario<a name="s3-manage-bucket-access"></a>

In this example, a Node\.js module is used to display the bucket access control list \(ACL\) for a selected bucket and apply changes to the ACL for a selected bucket\. The Node\.js module uses the SDK for JavaScript to manage Amazon S3 bucket access permissions using these methods of the Amazon S3 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getBucketAcl-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getBucketAcl-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putBucketAcl-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putBucketAcl-property)

For more information about access control lists for Amazon S3 buckets, see [ Managing access with ACLs](https://docs.aws.amazon.com/AmazonS3/latest/dev/S3_ACLs_UsingACLs.html) in the *Amazon Simple Storage Service Developer Guide*\.

## Prerequisite tasks<a name="s3-manage-bucket-access-prereqs"></a>

To set up and run this example, you must first complete these tasks:
+ Set up a project environment to run Node TypeScript examples by following the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so with minor adjustments these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Retrieving the current bucket Access Control List<a name="s3-example-access-permissions-get-acl"></a>

Create a Node\.js module with the file name `s3_getbucketacl.ts`\. The module will take a single command\-line argument to specify the bucket whose ACL configuration you want\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. 

Create an `S3` client service object\. The only parameter you need to pass is the name of the selected bucket when calling the `GetBucketAclCommand` method\. The current access control list configuration is returned by Amazon S3 in the `data` parameter passed to the callback function\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import required AWS SDK clients and commands for Node.js
const { S3Client, GetBucketAclCommand } = require("@aws-sdk/client-s3/");

// Set the AWS region
const REGION = "region"; //e.g. "us-east-1"

// Create the parameters for calling
const bucketParams = { Bucket: "BUCKET_NAME" };

// Create S3 service object
const s3 = new S3Client(REGION);

const run = async () => {
  try {
    const data = await s3.send(new GetBucketAclCommand(bucketParams));
    console.log("Success", data.Grants);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
 ts-node s3_getbucketacl.ts // If you prefer JavaScript, enter 'node s3_getbucketacl.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_getbucketacl.ts)\.