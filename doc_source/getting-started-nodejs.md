--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Getting started in Node\.js<a name="getting-started-nodejs"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to install and include the modules that your project uses\.
+ How to write the Node\.js code to create an Amazon S3 bucket and upload an object to that bucket\.
+ How to run the code\.

## The scenario<a name="getting-started-nodejs-scenario"></a>

The example shows how to set up and run a simple Node\.js module that creates an Amazon S3 bucket, then adds a text object to it\. 

## Prerequisite tasks<a name="getting-started-nodejs-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ [Install npm](https://npmjs.com/get-npm)\.
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/nodegetstarted/README.md)\.
+ You need to provide credentials to AWS so that only your account and its resources are accessed by the SDK\. For more information about obtaining your account credentials, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Step 1: Install the Amazon S3 package and dependencies<a name="getting-started-nodejs-install-sdk"></a>

To install the client package and dependencies:

1. In the `src` project directory, there is a `package.json` file for holding the metadata for your Node\.js project\. 
**Note**  
For details about using `package.json` in a Node\.js project, see [What is the file package\.json?](https://nodejs.org/en/knowledge/getting-started/npm/what-is-the-file-package-json/)\.

   ```
   {
     "name": "aws-sdk-v3-iam-examples",
     "version": "1.0.0",
     "main": "index.js",
     "dependencies": {
      "@aws-sdk/client-s3": "^3.3.0",
       "@aws-sdk/node-http-handler": "^3.3.0",
       "@aws-sdk/types": "^3.3.0",
       "ts-node": "^9.0.0"
     },
     "devDependencies": {
       "@types/node": "^14.0.23",
       "typescript": "^4.0.2"
     }
   }
   ```

   The example code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/nodegetstarted/package.json)\.

1. From the `nodegetstarted` directory containing the `package.json` enter the following command\.

   ```
   npm install
   ```

   The packages and dependencies are installed\. 
**Note**  
You can add dependencies to the `package.json` and install them by running `npm install`\. You can also add dependencies directly through the command line\. For example, to install the AWS SDK for JavaScript v3 client module for Amazon S3, enter the command below in the command line\.  

   ```
   npm install @aws-sdk/client-s3
   ```
The `package.json` dependencies are automatically updated\.

## Step 2: Write the Node\.js code<a name="getting-started-nodejs-js-code"></a>

**Important**  
This examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

Create a file named `sampleClient.js` to contain the for creating the Amazon S3 service client object\. Copy and paste the code below into it\. Replace *REGION* with your AWS Region\.

```
import { S3Client } from "@aws-sdk/client-s3";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon S3 service client object.
const s3Client = new S3Client({ region: REGION });
export { s3Client };
```

The example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/nodegetstarted/src/libs/s3Client.js)\.

First, define the parameters by replacing *BUCKET\_NAME* with the name of the bucket, *KEY* with the name of the new object, *BODY* with some content for the new object\.

Next, create an Amazon S3 client object\. Then create an async wrapper function that runs two try/catch statements in sequence\. The first try/catch statement creates the bucket, and the second creates and uploads the new object\. 

To create the bucket, you create a constant that runs the `CreateBucketCommand` using the `.send` method using the async/await pattern, passing in the name of the new bucket\. The await keyword blocks execution of all the code that follows until the bucket is created\. If an error occurs, the first `catch` statement returns an error\. 

To create and upload an object to the new bucket after it is created, you create a constant that runs the `PutObjectCommand`, also using the `.send` method using the async/await pattern, and passing in the bucket, key, and body parameters\. If an error occurs, the second `catch` statement returns an error\.

```
// Import required AWS SDK clients and commands for Node.js.
import { PutObjectCommand, CreateBucketCommand } from "@aws-sdk/client-s3";
import { s3Client } from "./libs/s3Client.js";

// Set the parameters
const params = {
  Bucket: "BUCKET_NAME", // The name of the bucket. For example, 'sample_bucket_101'.
  Key: "KEY", // The name of the object. For example, 'sample_upload.txt'.
  Body: "BODY", // The content of the object. For example, 'Hello world!".
};

const run = async () => {
  // Create an Amazon S3 bucket.
  try {
    const data = await s3Client.send(
        new CreateBucketCommand({ Bucket: params.Bucket })
    );
    console.log(data);
    console.log("Successfully created a bucket called ", data.Location);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
  // Create an object and upload it to the Amazon S3 bucket.
  try {
    const results = await s3Client.send(new PutObjectCommand(params));
    console.log(
        "Successfully created " +
        params.Key +
        " and uploaded it to " +
        params.Bucket +
        "/" +
        params.Key
    );
    return results; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

The example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/nodegetstarted/src/sample.js)\.

## Step 3: Run the example<a name="getting-started-nodejs-run-sample"></a>

Enter the following command to run the example\.

```
node sample.js
```

If the upload is successful, you'll see a confirmation message at the command prompt\. You can also find the bucket and the uploaded text object in the [Amazon S3 console](https://console.aws.amazon.com/s3/)\.
