--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Working with Amazon S3 bucket policies<a name="s3-example-bucket-policies"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve the bucket policy of an Amazon S3 bucket\.
+ How to add or update the bucket policy of an Amazon S3 bucket\.
+ How to delete the bucket policy of an Amazon S3 bucket\.

## The scenario<a name="s3-bucket-policy-examples"></a>

In this example, a series of Node\.js modules are used to retrieve, set, or delete a bucket policy on an Amazon S3 bucket\. The Node\.js modules use the SDK for JavaScript to configure policy for a selected Amazon S3 bucket using these methods of the Amazon S3 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/getbucketpolicycommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/getbucketpolicycommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/petbucketpolicycommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/petbucketpolicycommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/deletebucketpolicycommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/deletebucketpolicycommand.html)

For more information about bucket policies for Amazon S3 buckets, see [ Using bucket policies and user policies](https://docs.aws.amazon.com/AmazonS3/latest/dev/using-iam-policies.html) in the *Amazon Simple Storage Service User Guide*\.

## Prerequisite tasks<a name="s3-bucket-policy-examples-prereqs"></a>

To set up and run this example, you must first complete these tasks:
+ Set up a project environment to run Node JavaScript examples by following the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Retrieving the current bucket policy<a name="s3-example-bucket-policies-get-policy"></a>

Create a `libs` directory, and create a Node\.js module with the file name `s3Client.js`\. Copy and paste the code below into it, which creates the Amazon S3 client object\. Replace *REGION* with your AWS region\.

```
// Create service client module using ES6 syntax.
import { S3Client } from "@aws-sdk/client-s3";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon S3 service client object.
const s3Client = new S3Client({ region: REGION });
export { s3Client };
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/libs/s3Client.js)\.

Create a Node\.js module with the file name `s3_getbucketpolicy.js`\. The module takes a single command\-line argument that specifies the bucket whose policy you want\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. 

Create an `S3` service object\. The only parameter you need to pass is the name of the selected bucket when calling the `GetBucketPolicyCommand` method\. If the bucket currently has a policy, that policy is returned by Amazon S3 in the `data` parameter passed to the callback function\.

If the selected bucket has no policy, that information is returned to the callback function in the `error` parameter\.

```
// Import required AWS SDK clients and commands for Node.js.
import { GetBucketPolicyCommand } from "@aws-sdk/client-s3";
import { s3Client } from "./libs/s3Client.js"; // Helper function that creates an Amazon S3 service client module.

// Create the parameters for calling
export const bucketParams = { Bucket: "BUCKET_NAME" };

export const run = async () => {
  try {
    const data = await s3Client.send(new GetBucketPolicyCommand(bucketParams));
    console.log("Success", data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node s3_getbucketpolicy.js  
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_getbucketpolicy.js)\.

## Setting a simple bucket policy<a name="s3-example-bucket-policies-set-policy"></a>

Create a `libs` directory, and create a Node\.js module with the file name `s3Client.js`\. Copy and paste the code below into it, which creates the Amazon S3 client object\. Replace *REGION* with your AWS region\.

```
// Create service client module using ES6 syntax.
import { S3Client } from "@aws-sdk/client-s3";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon S3 service client object.
const s3Client = new S3Client({ region: REGION });
export { s3Client };
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/libs/s3Client.js)\.

Create a Node\.js module with the file name `s3_setbucketpolicy.js`\. The module takes a single command\-line argument that specifies the bucket whose policy you want to apply\. Configure the SDK as previously shown, including installing the required clients and packages\. 

Bucket policies are specified in JSON\. First, create a JSON object that contains all of the values to specify the policy except for the `Resource` value that identifies the bucket\.

Format the `Resource` string required by the policy, incorporating the name of the selected bucket\. Insert that string into the JSON object\. Prepare the parameters for the `PutBucketPolicyCommand` method, including the name of the bucket and the JSON policy converted to a string value\.

```
// Import required AWS SDK clients and commands for Node.js.
import { CreateBucketCommand, PutBucketPolicyCommand } from "@aws-sdk/client-s3";
import { s3Client } from "./libs/s3Client.js"; // Helper function that creates an Amazon S3 service client module.

const BUCKET_NAME = "BUCKET_NAME";
export const bucketParams = {
  Bucket: BUCKET_NAME,
};
// Create the policy in JSON for the S3 bucket.
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

// Create selected bucket resource string for bucket policy.
const bucketResource = "arn:aws:s3:::" + BUCKET_NAME + "/*"; //BUCKET_NAME
readOnlyAnonUserPolicy.Statement[0].Resource[0] = bucketResource;

// Convert policy JSON into string and assign into parameters.
const bucketPolicyParams = {
  Bucket: BUCKET_NAME,
  Policy: JSON.stringify(readOnlyAnonUserPolicy),
};

export const run = async () => {
  try {
    const data = await s3Client.send(
        new CreateBucketCommand(bucketParams)
    );
    console.log('Success, bucket created.', data)
    try {
      const response = await s3Client.send(
          new PutBucketPolicyCommand(bucketPolicyParams)
      );
      console.log("Success, permissions added to bucket", response);
    }
    catch (err) {
        console.log("Error adding policy to S3 bucket.", err);
      }
  } catch (err) {
    console.log("Error creating S3 bucket.", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node s3_setbucketpolicy.js  
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_putbucketpolicy.js)\.

## Deleting a bucket policy<a name="s3-example-bucket-policies-delete-policy"></a>

Create a `libs` directory, and create a Node\.js module with the file name `s3Client.js`\. Copy and paste the code below into it, which creates the Amazon S3 client object\. Replace *REGION* with your AWS region\.

```
// Create service client module using ES6 syntax.
import { S3Client } from "@aws-sdk/client-s3";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon S3 service client object.
const s3Client = new S3Client({ region: REGION });
export { s3Client };
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/libs/s3Client.js)\.

Create a Node\.js module with the file name `s3_deletebucketpolicy.js`\. The module takes a single command\-line argument that specifies the bucket whose policy you want to delete\. Configure the SDK as previously shown, including installing the required clients and packages\.

 The only parameter you need to pass when calling the `DeleteBucketPolicy` method is the name of the selected bucket\.

```
// Import required AWS SDK clients and commands for Node.js.
import { DeleteBucketPolicyCommand } from "@aws-sdk/client-s3";
import { s3Client } from "./libs/s3Client.js"; // Helper function that creates an Amazon S3 service client module.

// Set the bucket parameters
export const bucketParams = { Bucket: "BUCKET_NAME" };

export const run = async () => {
  try {
    const data = await s3Client.send(new DeleteBucketPolicyCommand(bucketParams));
    console.log("Success", data + ", bucket policy deleted");
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
// Invoke run() so these examples run out of the box.
run();
```

To run the example, enter the following at the command prompt\.

```
node s3_deletebucketpolicy.js  
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_deletebucketpolicy.js)\.