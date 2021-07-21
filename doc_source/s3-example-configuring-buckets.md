--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Configuring Amazon S3 buckets<a name="s3-example-configuring-buckets"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to configure the cross\-origin resource sharing \(CORS\) permissions for a bucket\.

## The scenario<a name="s3-example-configuring-buckets-scenario"></a>

In this example, a series of Node\.js modules are used to list your Amazon S3 buckets and to configure CORS and bucket logging\. The Node\.js modules use the SDK for JavaScript to configure a selected Amazon S3 bucket using these methods of the Amazon S3 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/getbucketcorscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/getbucketcorscommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/putbucketcorscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/putbucketcorscommand.html)

For more information about using CORS configuration with an Amazon S3 bucket, see [Cross\-origin resource sharing \(CORS\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/cors.html) in the *Amazon Simple Storage Service Developer Guide*\.

## Prerequisite tasks<a name="s3-example-configuring-buckets-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up a project environment to run Node JavaScript examples by following the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Retrieving a bucket CORS configuration<a name="s3-example-configuring-buckets-get-cors"></a>

Create a `libs` directory, and create a Node\.js module with the file name `s3Client.js`\. Copy and paste the code below into it, which creates the Amazon S3 client object\. Replace *REGION* with your AWS region\.

```
import { S3Client} from "@aws-sdk/client-s3";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon S3 service client object.
const s3Client = new S3Client({ region: REGION });
export { s3Client };
```

Create a Node\.js module with the file name `s3_getcors.js`\. The module will take a single command\-line argument to specify the bucket whose CORS configuration you want\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `S3` client service object\. 

The only parameter you need to pass is the name of the selected bucket when calling the `GetBucketCorsCommand` method\. If the bucket currently has a CORS configuration, that configuration is returned by Amazon S3 as the `CORSRules` property of the `data` parameter passed to the callback function\.

If the selected bucket has no CORS configuration, that information is returned to the callback function in the `error` parameter\.

```
// Import required AWS SDK clients and commands for Node.js
import { GetBucketCorsCommand } from "@aws-sdk/client-s3";
import { s3Client } from "./libs/s3Client.js"; // Helper function that creates Amazon S3 service client module.

// Create the parameters for calling
export const bucketParams = { Bucket: "BUCKET_NAME" };

export const run = async () => {
  try {
    const data = await s3Client.send(new GetBucketCorsCommand(bucketParams));
    console.log("Success", JSON.stringify(data.CORSRules));
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node s3_getcors.js 
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_getcors.js)\.

## Setting a bucket CORS configuration<a name="s3-example-configuring-buckets-put-cors"></a>

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

Create a Node\.js module with the file name `s3_setcors.js`\. The module takes multiple command\-line arguments, the first of which specifies the bucket whose CORS configuration you want to set\. Additional arguments enumerate the HTTP methods \(POST, GET, PUT, PATCH, DELETE, POST\) you want to allow for the bucket\. Configure the SDK as previously shown, including installing the required clients and packages\.

 Next create a JSON object to hold the values for the CORS configuration as required by the `PutBucketCorsCommand` method of the `S3` service object\. Specify `"Authorization"` for the `AllowedHeaders` value and `"*"` for the `AllowedOrigins` value\. Set the value of `AllowedMethods` as empty array initially\.

Specify the allowed methods as command line parameters to the Node\.js module, adding each of the methods that match one of the parameters\. Add the resulting CORS configuration to the array of configurations contained in the `CORSRules` parameter\. Specify the bucket you want to configure for CORS in the `Bucket` parameter\.

```
  // Import required AWS-SDK clients and commands for Node.js
 import { PutBucketCorsCommand } from "@aws-sdk/client-s3";
 import { s3Client } from "./libs/s3Client.js"; // Helper function that creates Amazon S3 service client module.

  // Set params
  // Create initial parameters JSON for putBucketCors
  const thisConfig = {
    AllowedHeaders: ["Authorization"],
    AllowedMethods: [],
    AllowedOrigins: ["*"],
    ExposeHeaders: [],
    MaxAgeSeconds: 3000,
  };

  // Assemble the list of allowed methods based on command line parameters
  const allowedMethods = [];
  process.argv.forEach(function (val, index, array) {
    if (val.toUpperCase() === "POST") {
      allowedMethods.push("POST");
    }
    if (val.toUpperCase() === "GET") {
      allowedMethods.push("GET");
    }
    if (val.toUpperCase() === "PUT") {
      allowedMethods.push("PUT");
    }
    if (val.toUpperCase() === "PATCH") {
      allowedMethods.push("PATCH");
    }
    if (val.toUpperCase() === "DELETE") {
      allowedMethods.push("DELETE");
    }
    if (val.toUpperCase() === "HEAD") {
      allowedMethods.push("HEAD");
    }
  });

  // Copy the array of allowed methods into the config object
  thisConfig.AllowedMethods = allowedMethods;

  // Create array of configs then add the config object to it
  const corsRules = new Array(thisConfig);

  // Create CORS params
export  const corsParams = {
    Bucket: "BUCKET_NAME",
    CORSConfiguration: { CORSRules: corsRules },
  };
export async function run() {
  try {
    const data = await s3Client.send(new PutBucketCorsCommand(corsParams));
    console.log("Success", data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
}
run();
```

To run the example, enter the following at the command prompt including one or more HTTP methods as shown\.

```
node s3_setcors.js
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_setcors.js)\.