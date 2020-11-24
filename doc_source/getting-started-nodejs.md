--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Getting started in Node\.js<a name="getting-started-nodejs"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create the `package.json` manifest for your project\.
+ How to install and include the modules that your project uses\.
+ How to create an Amazon Simple Storage Service \(Amazon S3\) service object from the `S3` client class\.
+ How to create an Amazon S3 bucket and upload an object to that bucket\.

## The scenario<a name="getting-started-nodejs-scenario"></a>

The example shows how to set up and run a simple Node\.js module that creates an Amazon S3 bucket, then adds a text object to it\. 

## Prerequisite tasks<a name="getting-started-nodejs-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/nodegetstarted/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so with minor adjustments these example can also be run in JavaScript\.

## Step 1: Configure your credentials<a name="getting-started-nodejs-credentials"></a>

You need to provide credentials to AWS so that only your account and its resources are accessed by the SDK\. For more information about obtaining your account credentials, see [Getting your credentials](getting-your-credentials.md)\.

## Step 2: Create the package JSON for the project<a name="getting-started-nodejs-download"></a>

In the `src` project directory, you create and add a `package.json` file for holding the metadata for your Node\.js project\. For details about using `package.json` in a Node\.js project, see [What is the file package\.json?](https://nodejs.org/en/knowledge/getting-started/npm/what-is-the-file-package-json/)

```
{
  "dependencies": {},
  "name": "aws-nodejs-sample",
  "description": "A simple Node.js application illustrating usage of the AWS SDK for Node.js.",
  "version": "1.0.1",
  "main": "sample.js",
  "devDependencies": {},
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "NAME",
  "license": "ISC"
}
```

 Save the file\. As you install the modules you need, the `dependencies` portion of the file will be completed\. You can find a JSON file that shows an example of these dependencies [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/nodegetstarted/package.json)\. Alternatively, you can specify the dependencies in the `package.json`, and install them by running `npm install` at a command prompt\. For more information, see [Install package and dependencies from your package\.json](#getting-started-nodejs-install-sdk-json-first)\.

## Step 3: Install the Amazon S3 package and dependencies<a name="getting-started-nodejs-install-sdk"></a>

This section demonstrates two methods of installing the Amazon S3 client package and dependencies:
+ Install client packages and dependencies individually with [npm \(the Node\.js package manager\)](https://www.npmjs.com)\.
+ Install client packages and dependencies from `package.json`\.

### Install package and dependencies individually<a name="getting-started-nodejs-install-sdk-dependencies-first"></a>

1. [Install npm](https://npmjs.com/get-npm)\.

1. From the `src` directory in the package, enter the following command to install the Amazon S3 package\.

   ```
   npm install @aws-sdk/client-s3
   ```

   This command installs the Amazon S3 package in your project, and updates `package.json` to list Amazon S3 as a project dependency\. You can find information about this package by searching for "aws\-sdk" on the [npm website](https://www.npmjs.com)\.

These packages and their associated code are installed in the `node_modules` subdirectory of your project\.

For more information about installing Node\.js packages, see [Downloading and installing packages locally](https://docs.npmjs.com/getting-started/installing-npm-packages-locally) and [Creating Node\.js modules](https://docs.npmjs.com/getting-started/creating-node-modules) on the [npm \(Node\.js package manager\) website](https://www.npmjs.com)\. For information about downloading and installing the AWS SDK for JavaScript, see [Installing the SDK for JavaScript](installing-jssdk.md)\.

### Install package and dependencies from your package\.json<a name="getting-started-nodejs-install-sdk-json-first"></a>

1. [Install npm](https://npmjs.com/get-npm)\.

1. Add the packages and dependencies to your `package.json`\.

   ```
   {
     "name": "aws-sdk-v3-iam-examples",
     "version": "1.0.0",
     "main": "index.js",
     "dependencies": {
      "@aws-sdk/client-s3": "^1.0.0-gamma.6",
       "@aws-sdk/node-http-handler": "^1.0.0-gamma.5",
       "@aws-sdk/types": "^0.1.0-gamma.6",
       "ts-node": "^9.0.0"
     },
     "devDependencies": {
       "@types/node": "^14.0.23",
       "typescript": "^4.0.2"
     }
   }
   ```
**Note**  
The example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/nodegetstarted/package.json)\.

1. From the `nodegetstarted` directory containing the `package.json` enter the following command\.

   ```
   npm install
   ```

   The packages and dependencies are installed\. For example, the Amazon S3 client packages is in `~/node_modules/@aws-sdk`\.

## Step 4: Write the Node\.js code<a name="getting-started-nodejs-js-code"></a>

Create a file named `sample.ts` to contain the example code\. Begin by adding the `require` function calls to include the Amazon S3 package\.

Create an Amazon S3 bucket in the Amazon Console\. For more information, see [Create an Amazon Amazon S3 bucket](https://docs.aws.amazon.com/quickstarts/latest/s3backup/step-1-create-bucket.html)\.

Then add a name for the `Key` parameter used to upload an object to the bucket\.

Create the `PutObjectCommand` object to encapsulate the properties of the associated `S3` service object requests\. Call the `send` command with the `createBucket` and `PutObjectCommand` objects to create a new Amazon S3 bucket and upload the object to it\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  S3Client,
  PutObjectCommand,
  CreateBucketCommand
} = require("@aws-sdk/client-s3");

// Set the AWS region
const REGION = "REGION"; // e.g., "us-east-1"

// Set the bucket parameters
const bucketName = "BUCKET_NAME";
const bucketParams = { Bucket: bucketName };

// Create name for uploaded object key
const keyName = "hello_world.txt";
const objectParams = { Bucket: bucketName, Key: keyName, Body: "Hello World!" };

// Create an S3 client service object
const s3 = new S3Client(REGION);

const run = async () => {
  // Create S3 bucket
  try {
    const data = await s3.send(new CreateBucketCommand(bucketParams));
    console.log("Success. Bucket created.");
  } catch (err) {
    console.log("Error", err);
  }
  try {
    const results = await s3.send(new PutObjectCommand(objectParams));
    console.log("Successfully uploaded data to " + bucketName + "/" + keyName);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

The example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/nodegetstarted/src/sample.ts)\.

## Step 5: Run the example<a name="getting-started-nodejs-run-sample"></a>

Enter the following command to run the example\.

```
node sample.js
```

If the upload is successful, you'll see a confirmation message at the command prompt\. You can also find the bucket and the uploaded text object in the [Amazon S3 console](https://console.aws.amazon.com/s3/)\.