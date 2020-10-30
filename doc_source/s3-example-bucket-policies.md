--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Working with Amazon S3 bucket policies<a name="s3-example-bucket-policies"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve the bucket policy of an Amazon S3 bucket\.
+ How to add or update the bucket policy of an Amazon S3 bucket\.
+ How to delete the bucket policy of an Amazon S3 bucket\.

## The scenario<a name="s3-bucket-policy-examples"></a>

In this example, a series of Node\.js modules are used to retrieve, set, or delete a bucket policy on an Amazon S3 bucket\. The Node\.js modules use the SDK for JavaScript to configure policy for a selected Amazon S3 bucket using these methods of the Amazon S3 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getBucketPolicy-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getBucketPolicy-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putBucketPolicy-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putBucketPolicy-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#deleteBucketPolicy-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#deleteBucketPolicy-property)

For more information about bucket policies for Amazon S3 buckets, see [ Using bucket policies and user policies](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-iam-policies.html) in the *Amazon Simple Storage Service Developer Guide*\.

## Prerequisite tasks<a name="s3-bucket-policy-examples-prereqs"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so with minor adjustments these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Retrieving the current bucket policy<a name="s3-example-bucket-policies-get-policy"></a>

Create a Node\.js module with the file name `s3_getbucketpolicy.ts`\. The module takes a single command\-line argument that specifies the bucket whose policy you want\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. 

Create an `S3` service object\. The only parameter you need to pass is the name of the selected bucket when calling the `GetBucketPolicyCommand` method\. If the bucket currently has a policy, that policy is returned by Amazon S3 in the `data` parameter passed to the callback function\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

If the selected bucket has no policy, that information is returned to the callback function in the `error` parameter\.

```
// Import required AWS SDK clients and commands for Node.js
const { S3Client, GetBucketPolicyCommand } = require("@aws-sdk/client-s3");

// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"

// Create the parameters for calling
const bucketParams = { Bucket: "BUCKET_NAME" };

// Create S3 service object
const s3 = new S3Client(REGION);

const run = async () => {
  try {
    const data = await s3.send(new GetBucketPolicyCommand(bucketParams));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node s3_getbucketpolicy.ts // If you prefer JavaScript, enter 'node s3_getbucketpolicy.js' 
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_getbucketpolicy.ts)\.

## Setting a simple bucket policy<a name="s3-example-bucket-policies-set-policy"></a>

Create a Node\.js module with the file name `s3_setbucketpolicy.ts`\. The module takes a single command\-line argument that specifies the bucket whose policy you want to apply\. Configure the SDK as previously shown, including installing the required clients and packages\. 

Create an `S3` client service object\. Bucket policies are specified in JSON\. First, create a JSON object that contains all of the values to specify the policy except for the `Resource` value that identifies the bucket\.

Format the `Resource` string required by the policy, incorporating the name of the selected bucket\. Insert that string into the JSON object\. Prepare the parameters for the `PutBucketPolicyCommand` method, including the name of the bucket and the JSON policy converted to a string value\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import required AWS SDK clients and commands for Node.js
const { S3Client, PutBucketPolicyCommand } = require("@aws-sdk/client-s3");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Create params JSON for S3.createBucket
const BUCKET_NAME = "BUCKET_NAME";
const bucketParams = {
  Bucket: BUCKET_NAME,
};
// Create the policy
const readOnlyAnonUserPolicy = {
  Version: "2012-10-17",
  Statement: [
    {
      Sid: "AddPerm",
      Effect: "Allow",
      Principal: "*",
      Action: ["s3:GetObject"],
      Resource: [""],
    },
  ],
};

// create selected bucket resource string for bucket policy
const bucketResource = "arn:aws:s3:::" + BUCKET_NAME + "/*"; //BUCKET_NAME
readOnlyAnonUserPolicy.Statement[0].Resource[0] = bucketResource;

// // convert policy JSON into string and assign into params
const bucketPolicyParams = {
  Bucket: BUCKET_NAME,
  Policy: JSON.stringify(readOnlyAnonUserPolicy),
};

// // Instantiate an S3 client
const s3 = new S3Client({});

const run = async () => {
  try {
    // const response = await s3.putBucketPolicy(bucketPolicyParams);
    const response = await s3.send(
      new PutBucketPolicyCommand(bucketPolicyParams)
    );
    console.log("Success, permissions added to bucket", response);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node s3_setbucketpolicy.ts  // If you prefer JavaScript, enter 'node s3_setbucketpolicy.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_putbucketpolicy.ts)\.

## Deleting a bucket policy<a name="s3-example-bucket-policies-delete-policy"></a>

Create a Node\.js module with the file name `s3_deletebucketpolicy.ts`\. The module takes a single command\-line argument that specifies the bucket whose policy you want to delete\. Configure the SDK as previously shown, including installing the required clients and packages\.

 Create an `S3` client service object\. The only parameter you need to pass when calling the `DeleteBucketPolicy` method is the name of the selected bucket\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import required AWS SDK clients and commands for Node.js
const { S3Client, DeleteBucketPolicyCommand } = require("@aws-sdk/client-s3/");

// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the bucket parameters
const bucketParams = { Bucket: "BUCKET_NAME" };

// Create S3 service object
const s3 = new S3Client(REGION);

const run = async () => {
  try {
    const data = await s3.send(new DeleteBucketPolicyCommand(bucketParams));
    console.log("Success", data + ", bucket policy deleted");
  } catch (err) {
    console.log("Error", err);
  }
};
// Invoke run() so these examples run out of the box.
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node s3_deletebucketpolicy.ts  // If you prefer JavaScript, enter 'node s3_deletebucketpolicy.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_deletebucketpolicy.ts)\.