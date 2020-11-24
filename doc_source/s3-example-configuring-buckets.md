--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Configuring Amazon S3 buckets<a name="s3-example-configuring-buckets"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to configure the cross\-origin resource sharing \(CORS\) permissions for a bucket\.

## The scenario<a name="s3-example-configuring-buckets-scenario"></a>

In this example, a series of Node\.js modules are used to list your Amazon S3 buckets and to configure CORS and bucket logging\. The Node\.js modules use the SDK for JavaScript to configure a selected Amazon S3 bucket using these methods of the Amazon S3 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getBucketCors-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#getBucketCors-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putBucketCors-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putBucketCors-property)

For more information about using CORS configuration with an Amazon S3 bucket, see [Cross\-origin resource sharing \(CORS\)](https://docs.aws.amazon.com/AmazonS3/latest/dev/cors.html) in the *Amazon Simple Storage Service Developer Guide*\.

## Prerequisite tasks<a name="s3-example-configuring-buckets-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up a project environment to run Node TypeScript examples by following the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so with minor adjustments these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Retrieving a bucket CORS configuration<a name="s3-example-configuring-buckets-get-cors"></a>

Create a Node\.js module with the file name `s3_getcors.ts`\. The module will take a single command\-line argument to specify the bucket whose CORS configuration you want\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `S3` client service object\. 

The only parameter you need to pass is the name of the selected bucket when calling the `GetBucketCorsCommand` method\. If the bucket currently has a CORS configuration, that configuration is returned by Amazon S3 as the `CORSRules` property of the `data` parameter passed to the callback function\.

If the selected bucket has no CORS configuration, that information is returned to the callback function in the `error` parameter\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import required AWS SDK clients and commands for Node.js
const { S3Client, GetBucketCorsCommand } = require("@aws-sdk/client-s3");

// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"

// Create the parameters for calling
const bucketParams = { Bucket: "BUCKET_NAME" };

// Create S3 service object
const s3 = new S3Client(REGION);

const run = async () => {
  try {
    const data = await s3.send(new GetBucketCorsCommand(bucketParams));
    console.log("Success", JSON.stringify(data.CORSRules));
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node s3_getcors.ts // If you prefer JavaScript, enter 'node s3_getcors.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_getcors.ts)\.

## Setting a bucket CORS configuration<a name="s3-example-configuring-buckets-put-cors"></a>

Create a Node\.js module with the file name `s3_setcors.ts`\. The module takes multiple command\-line arguments, the first of which specifies the bucket whose CORS configuration you want to set\. Additional arguments enumerate the HTTP methods \(POST, GET, PUT, PATCH, DELETE, POST\) you want to allow for the bucket\. Configure the SDK as previously shown, including installing the required clients and packages\.

 Create an `S3` client service object\. Next create a JSON object to hold the values for the CORS configuration as required by the `PutBucketCorsCommand` method of the `S3` service object\. Specify `"Authorization"` for the `AllowedHeaders` value and `"*"` for the `AllowedOrigins` value\. Set the value of `AllowedMethods` as empty array initially\.

Specify the allowed methods as command line parameters to the Node\.js module, adding each of the methods that match one of the parameters\. Add the resulting CORS configuration to the array of configurations contained in the `CORSRules` parameter\. Specify the bucket you want to configure for CORS in the `Bucket` parameter\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
async function run() {
  // Import required AWS-SDK clients and commands for Node.js
  const { S3Client, PutBucketCorsCommand } = require("@aws-sdk/client-s3");

  // Set the AWS region
  const REGION = "REGION";

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
  const corsParams = {
    Bucket: "BUCKET_NAME",
    CORSConfiguration: { CORSRules: corsRules },
  };

  // Create S3 service object
  const s3 = new S3Client(REGION);

  try {
    const data = await s3.send(new PutBucketCorsCommand(corsParams));
    console.log("Success", data);
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

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_setcors.ts)\.