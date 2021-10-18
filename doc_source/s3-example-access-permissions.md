--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Managing Amazon S3 bucket access permissions<a name="s3-example-access-permissions"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve or set the access control list for an Amazon S3 bucket\.

## The scenario<a name="s3-manage-bucket-access"></a>

In this example, a Node\.js module is used to display the bucket access control list \(ACL\) for a selected bucket and apply changes to the ACL for a selected bucket\. The Node\.js module uses the SDK for JavaScript to manage Amazon S3 bucket access permissions using these methods of the Amazon S3 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/getbucketcorscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/getbucketcorscommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/putbucketaclcommand.htm](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/putbucketaclcommand.htm)

For more information about access control lists for Amazon S3 buckets, see [ Managing access with ACLs](https://docs.aws.amazon.com/AmazonS3/latest/dev/S3_ACLs_UsingACLs.html) in the *Amazon Simple Storage Service User Guide*\.

## Prerequisite tasks<a name="s3-manage-bucket-access-prereqs"></a>

To set up and run this example, you must first complete these tasks:
+ Set up a project environment to run Node JavaScript examples by following the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Retrieving the current bucket Access Control List<a name="s3-example-access-permissions-get-acl"></a>

Create a `libs` directory, and create a Node\.js module with the file name `s3Client.js`\. Copy and paste the code below into it, which creates the Amazon S3 client object\. Replace *REGION* with your AWS region\.

```
import { S3Client} from "@aws-sdk/client-s3";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon S3 service client object.
const s3Client = new S3Client({ region: REGION });
export { s3Client };
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/libs/s3Client.js)\.

Create a Node\.js module with the file name `s3_getbucketacl.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. 

Create an `S3Client` client service object\. The only parameter you need to pass is the name of the selected bucket when calling the `GetBucketAclCommand` method\. The current access control list configuration is returned by Amazon S3 in the `data` parameter passed to the callback function\.

```
// Import required AWS SDK clients and commands for Node.js
import { GetBucketAclCommand } from "@aws-sdk/client-s3/";
import { s3Client } from "./libs/s3Client.js"; // Helper function that creates Amazon S3 service client module.

// Create the parameters.
export const bucketParams = { Bucket: "BUCKET_NAME" };

export const run = async () => {
  try {
    const data = await s3Client.send(new GetBucketAclCommand(bucketParams));
    console.log("Success", data.Grants);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
 node s3_getbucketacl.js 
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_getbucketacl.js)\.

## Attaching Access Control List permissions to an Amazon S3 bucket<a name="s3-example-access-permissions-put-acl"></a>

Create a `libs` directory, and create a Node\.js module with the file name `s3Client.js`\. Copy and paste the code below into it, which creates the Amazon S3 client object\. Replace *REGION* with your AWS region\.

```
import { S3Client} from "@aws-sdk/client-s3";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon S3 service client object.
const s3Client = new S3Client({ region: REGION });
export { s3Client };
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/libs/s3Client.js)\.

Create a Node\.js module with the file name `s3_putbucketacl.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. 

Replace *BUCKET\_NAME* with the name of the Amazon S3 bucket\. Replace *GRANTEE\_1* and *GRANTEE\_2* with users you want to grant respective access contol permission\. 

```
/ Import required AWS SDK; clients and commands for Node.js.
const { S3Client, PutBucketAclCommand } = require("@aws-sdk/client-s3");

// Set the parameters. 
const bucketParams = {
    Bucket: "BUCKET_NAME",
    // 'GrantFullControl' allows grantee the read, write, read ACP, and write ACP permissions on the bucket.
    // For example, an AWS account Canonical User ID in the format:
    // id=002160194XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXa7a49125274
    GrantFullControl:
        "GRANTEE_1",
    // 'GrantWrite' allows grantee to create, overwrite, and delete any object in the bucket..
    // For example, 'uri=http://acs.amazonaws.com/groups/s3/LogDelivery'
    GrantWrite: "GRANTEE_2"
};

// Create an Amazon S3 client service object.
const s3 = new S3Client({});

const run = async () => {
    try {
        const data = await s3.send(new PutBucketAclCommand(bucketParams));
        console.log("Success, permissions added to bucket", data);
    } catch (err) {
        console.log("Error", err);
    }
};
run();
```

To run the example, enter the following at the command prompt\.

```
 node s3_putbucketacl.js 
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_putbucketacl.js)\.