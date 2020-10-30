--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Using an Amazon S3 bucket as a static web host<a name="s3-example-static-web-host"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to set up an Amazon S3 bucket as a static web host\.

## The scenario<a name="s3-example-static-web-host-scenario"></a>

In this example, a series of Node\.js modules are used to configure any of your buckets to act as a static web host\. The Node\.js modules use the SDK for JavaScript to configure a selected Amazon S3 bucket using these methods of the Amazon S3 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getBucketWebsite-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getBucketWebsite-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putBucketWebsite-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putBucketWebsite-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#deleteBucketWebsite-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#deleteBucketWebsite-property)

For more information about using an Amazon S3 bucket as a static web host, see [Hosting a static website on Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html) in the *Amazon Simple Storage Service Developer Guide*\.

## Prerequisite tasks<a name="s3-example-static-web-host-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so with minor adjustments these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Retrieving the current bucket website configuration<a name="s3-example-static-web-host-get-website"></a>

Create a Node\.js module with the file name `s3_getbucketwebsite.ts`\. The module takes a single command\-line argument that specifies the bucket whose website configuration you want\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an `S3` client service object\. Create a function that retrieves the current bucket website configuration for the bucket selected in the bucket list\. The only parameter you need to pass is the name of the selected bucket when calling the `GetBucketWebsiteCommand` method\. If the bucket currently has a website configuration, that configuration is returned by Amazon S3 in the `data` parameter passed to the callback function\.

If the selected bucket has no website configuration, that information is returned to the callback function in the `err` parameter\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
 */

// Import required AWS SDK clients and commands for Node.js
const { S3Client, GetBucketWebsiteCommand } = require("@aws-sdk/client-s3");

// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"

// Create the parameters for calling
const bucketParams = { Bucket: "BUCKET_NAME" };

// Create S3 service object
const s3 = new S3Client(REGION);

const run = async () => {
  try {
    const data = await s3.send(new GetBucketWebsiteCommand(bucketParams));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node s3_getbucketwebsite.ts // If you prefer JavaScript, enter 'node s3_getbucketwebsite.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_getbucketwebsite.ts)\.

## Setting a bucket website configuration<a name="s3-example-static-web-host-set-website"></a>

Create a Node\.js module with the file name `s3_setbucketwebsite.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `S3` client service object\. 

Create a function that applies a bucket website configuration\. The configuration allows the selected bucket to serve as a static web host\. Website configurations are specified in JSON\. First, create a JSON object that contains all the values to specify the website configuration, except for the `Key` value that identifies the error document, and the `Suffix` value that identifies the index document\.

Insert the values of the text input elements into the JSON object\. Prepare the parameters for the `PutBucketWebsiteCommand` method, including the name of the bucket and the JSON website configuration\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import required AWS SDK clients and commands for Node.js
const { S3Client, PutBucketWebsiteCommand } = require("@aws-sdk/client-s3");

// Set the AWS region
const REGION = "region"; //e.g. "us-east-1"

// Create the parameters for the bucket
const bucketParams = { Bucket: "BUCKET_NAME" };
const staticHostParams = {
  Bucket: bucketParams,
  WebsiteConfiguration: {
    ErrorDocument: {
      Key: "",
    },
    IndexDocument: {
      Suffix: "",
    },
  },
};

// Create S3 service object
const s3 = new S3Client(REGION);

const run = async () => {
  // Insert specified bucket name and index and error documents into params JSON
  // from command line arguments
  staticHostParams.Bucket = bucketParams;
  staticHostParams.WebsiteConfiguration.IndexDocument.Suffix = "INDEX_PAGE"; // the index document inserted into params JSON
  staticHostParams.WebsiteConfiguration.ErrorDocument.Key = "ERROR_PAGE"; // : the error document inserted into params JSON
  // set the new website configuration on the selected bucket
  try {
    const data = await s3.send(new PutBucketWebsiteCommand(staticHostParams));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node s3_setbucketwebsite.ts // If you prefer JavaScript, enter 'node s3_setbucketwebsite.js' 
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_setbucketwebsite.ts)\.

## Deleting a bucket website configuration<a name="s3-example-static-web-host-delete-website"></a>

Create a Node\.js module with the file name `s3_deletebucketwebsite.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `S3` client service object\. 

Create a function that deletes the website configuration for the selected bucket\. The only parameter you need to pass when calling the `DeleteBucketWebsiteCommand` method is the name of the selected bucket\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import required AWS SDK clients and commands for Node.js

const { S3Client, DeleteBucketWebsiteCommand } = require("@aws-sdk/client-s3");

// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"

// Create the parameters for calling
const bucketParams = { Bucket: "BUCKET_NAME" };

// Create S3 service object
const s3 = new S3Client(REGION);

const run = async () => {
  try {
    const data = await s3.send(new DeleteBucketWebsiteCommand(bucketParams));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node s3_deletebucketwebsite.ts // If you prefer JavaScript, enter 'node s3_deletebucketwebsite..js' 
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_deletebucketwebsite.ts)\.