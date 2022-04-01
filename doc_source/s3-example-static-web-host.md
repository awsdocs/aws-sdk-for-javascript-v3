--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Using an Amazon S3 bucket as a static web host<a name="s3-example-static-web-host"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to set up an Amazon S3 bucket as a static web host\.

## The scenario<a name="s3-example-static-web-host-scenario"></a>

In this example, a series of Node\.js modules are used to configure any of your buckets to act as a static web host\. The Node\.js modules use the SDK for JavaScript to configure a selected Amazon S3 bucket using these methods of the Amazon S3 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/getbucketwebsitecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/getbucketwebsitecommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/putbucketwebsitecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/putbucketwebsitecommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/deletebucketwebsitecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/deletebucketwebsitecommand.html)

For more information about using an Amazon S3 bucket as a static web host, see [Hosting a static website on Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html) in the *Amazon Simple Storage Service User Guide*\.

## Prerequisite tasks<a name="s3-example-static-web-host-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up a project environment to run Node JavaScript examples by following the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Retrieving the current bucket website configuration<a name="s3-example-static-web-host-get-website"></a>

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

Create a Node\.js module with the file name `s3_getbucketwebsite.js`\. The module takes a single command\-line argument that specifies the bucket whose website configuration you want\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create a function that retrieves the current bucket website configuration for the bucket selected in the bucket list\. The only parameter you need to pass is the name of the selected bucket when calling the `GetBucketWebsiteCommand` method\. If the bucket currently has a website configuration, that configuration is returned by Amazon S3 in the `data` parameter passed to the callback function\.

If the selected bucket has no website configuration, that information is returned to the callback function in the `err` parameter\.

```
// Import required AWS SDK clients and commands for Node.js.
import { GetBucketWebsiteCommand } from "@aws-sdk/client-s3";
import { s3Client } from "./libs/s3Client.js"; // Helper function that creates an Amazon S3 service client module.

// Create the parameters for calling
export const bucketParams = { Bucket: "BUCKET_NAME" };

export const run = async () => {
  try {
    const data = await s3Client.send(new GetBucketWebsiteCommand(bucketParams));
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
node s3_getbucketwebsite.js 
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_getbucketwebsite.js)\.

## Setting a bucket website configuration<a name="s3-example-static-web-host-set-website"></a>

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

Create a Node\.js module with the file name `s3_setbucketwebsite.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `S3` client service object\. 

Create a function that applies a bucket website configuration\. The configuration allows the selected bucket to serve as a static web host\. Website configurations are specified in JSON\. First, create a JSON object that contains all the values to specify the website configuration, except for the `Key` value that identifies the error document, and the `Suffix` value that identifies the index document\.

Insert the values of the text input elements into the JSON object\. Prepare the parameters for the `PutBucketWebsiteCommand` method, including the name of the bucket and the JSON website configuration\.

```
// Import required AWS SDK clients and commands for Node.js.
import { PutBucketWebsiteCommand } from "@aws-sdk/client-s3";
import { s3Client } from "./libs/s3Client.js"; // Helper function that creates an Amazon S3 service client module.

// Create the parameters for the bucket
export const bucketParams = { Bucket: "BUCKET_NAME" };
export const staticHostParams = {
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

export const run = async () => {
  // Insert specified bucket name and index and error documents into parameters JSON
  // from command line arguments
  staticHostParams.Bucket = bucketParams;
  staticHostParams.WebsiteConfiguration.IndexDocument.Suffix = "INDEX_PAGE"; // The index document inserted into parameters JSON.
  staticHostParams.WebsiteConfiguration.ErrorDocument.Key = "ERROR_PAGE"; // The error document inserted into parameters JSON.
  // Set the new website configuration on the selected bucket.
  try {
    const data = await s3Client.send(new PutBucketWebsiteCommand(staticHostParams));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node s3_setbucketwebsite.js 
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_setbucketwebsite.js)\.

## Deleting a bucket website configuration<a name="s3-example-static-web-host-delete-website"></a>

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

Create a Node\.js module with the file name `s3_deletebucketwebsite.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `S3` client service object\. 

Create a function that deletes the website configuration for the selected bucket\. The only parameter you need to pass when calling the `DeleteBucketWebsiteCommand` method is the name of the selected bucket\.

```
// Import required AWS SDK clients and commands for Node.js.

import { DeleteBucketWebsiteCommand } from "@aws-sdk/client-s3";
import { s3Client } from "./libs/s3Client.js"; // Helper function that creates an Amazon S3 service client module.

// Create the parameters for calling
export const bucketParams = { Bucket: "BUCKET_NAME" };

export const run = async () => {
  try {
    const data = await s3Client.send(new DeleteBucketWebsiteCommand(bucketParams));
    return data; // For unit tests.
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node s3_deletebucketwebsite.js 
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_deletebucketwebsite.js)\.