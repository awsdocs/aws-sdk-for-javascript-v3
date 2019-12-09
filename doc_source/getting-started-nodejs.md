# Getting Started in Node\.js<a name="getting-started-nodejs"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create the `package.json` manifest for your project\.
+ How to install and include the modules that your project uses\.
+ How to create an Amazon Simple Storage Service \(Amazon S3\) service object from the `AWS.S3` client class\.
+ How to create an Amazon S3 bucket and upload an object to that bucket\.

## The Scenario<a name="getting-started-nodejs-scenario"></a>

The example shows how to set up and run a simple Node\.js module that creates an Amazon S3 bucket, then adds a text object to it\. 

Because bucket names in Amazon S3 must be globally unique, this example includes a third\-party Node\.js module that generates a unique ID value that you can incorporate into the bucket name\. This additional module is named `uuid`\.

## Prerequisite Tasks<a name="getting-started-nodejs-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Create a working directory for developing your Node\.js module\. For this example we'll use `awsnodesample`\. Note that the directory must be created in a location that can be updated by applications\. For example, in Windows, do not create the directory under `C:\Program Files`\.
+ Install Node\.js\. For more information, see the [Node\.js website](https://nodejs.org)\. You can find downloads of the current and LTS versions of Node\.js for a variety of operating systems at [https://nodejs\.org/en/download/current/](https://nodejs.org/en/download/current/)\.

**Contents**
+ [The Scenario](#getting-started-nodejs-scenario)
+ [Prerequisite Tasks](#getting-started-nodejs-prerequisites)
+ [Step 1: Configure Your Credentials](#getting-started-nodejs-credentials)
+ [Step 2: Create the Package JSON for the Project](#getting-started-nodejs-download)
+ [Step 3: Install the Amazon S3 Package and Dependencies](#getting-started-nodejs-install-sdk)
+ [Step 4: Write the Node\.js Code](#getting-started-nodejs-js-code)
+ [Step 5: Run the Example](#getting-started-nodejs-run-sample)

## Step 1: Configure Your Credentials<a name="getting-started-nodejs-credentials"></a>

You need to provide credentials to AWS so that only your account and its resources are accessed by the SDK\. For more information about obtaining your account credentials, see [Getting Your Credentials](getting-your-credentials.md)\.

## Step 2: Create the Package JSON for the Project<a name="getting-started-nodejs-download"></a>

After you create the `awsnodesample` project directory, you create and add a `package.json` file for holding the metadata for your Node\.js project\. For details about using `package.json` in a Node\.js project, see [What is the file package\.json?](https://nodejs.org/en/knowledge/getting-started/npm/what-is-the-file-package-json/)

In the project directory, create a new file named `package.json`\. Then add this JSON to the file\.

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

 Save the file\. As you install the modules you need, the `dependencies` portion of the file will be completed\. You can find a JSON file that shows an example of these dependencies [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_codenodegetstarted/example_package.json)\. 

## Step 3: Install the Amazon S3 Package and Dependencies<a name="getting-started-nodejs-install-sdk"></a>

Install the Amazon S3 package using [npm \(the Node\.js package manager\)](https://www.npmjs.com)\. 

From the `awsnodesample` directory in the package, enter the following command to install the Amazon S3 package\.

```
npm install @aws-sdk/client-s3
```

This command installs the Amazon S3 package in your project, and updates `package.json` to list Amazon S3 as a project dependency\. You can find information about this package by searching for "aws\-sdk" on the [npm website](https://www.npmjs.com)\.

Next, install the `uuid` module to the project by typing the following at the command line, which installs the module and updates `package.json`\. For more information about `uuid`, see the module's page at [https://www\.npmjs\.com/package/uuid](https://www.npmjs.com/package/uuid)\.

```
npm install uuid
```

If you see the following error message:

```
npm WARN deprecated node-uuid@1.4.8: Use uuid module instead
```

Type these commands at the command line:

```
npm uninstall --save node-uuid
npm install --save uuid
```

These packages and their associated code are installed in the `node_modules` subdirectory of your project\.

For more information about installing Node\.js packages, see [Downloading and installing packages locally](https://docs.npmjs.com/getting-started/installing-npm-packages-locally) and [Creating Node\.js Modules](https://docs.npmjs.com/getting-started/creating-node-modules) on the [npm \(Node\.js package manager\) website](https://www.npmjs.com)\. For information about downloading and installing the AWS SDK for JavaScript, see [Installing the SDK for JavaScript](installing-jssdk.md)\.

## Step 4: Write the Node\.js Code<a name="getting-started-nodejs-js-code"></a>

Create a new file named `sample.js` to contain the example code\. Begin by adding the `require` function calls to include the Amazon S3 and `uuid` packages\.

Create a unique bucket name for the new Amazon S3 bucket by appending a unique ID value to a recognizable prefix, in this case `'node-sdk-sample-'`\. Generate the unique ID by calling the `uuid` module\. Then create a name for the `Key` parameter used to upload an object to the bucket\.

Create `CreateBucketCommand` and `PutObjectCommand` objects to encapsulate the properties of the associated `S3Client` service object requests\. Call the `send` command with the `CreateBucketCommand` and `PutObjectCommand` objects to create a new Amazon S3 bucket and upload the object to it\.

```
(async function() {
  // Load the S3 client and commands for Node.js
  const {
    S3Client,
    CreateBucketCommand,
    PutObjectCommand
  } = require('@aws-sdk/client-s3')
  
  var uuid = require('uuid')

  // Unique bucket name
  const bucketName = 'node-sdk-sample-' + uuid.v4()
  // Name for uploaded object
  const keyName = 'hello_world.txt'

  const client = new S3Client({})

  const createCommand = new CreateBucketCommand({
    Bucket: bucketName
  })

  const putCommand = new PutObjectCommand({
    Bucket: bucketName,
    Key: keyName,
    Body: 'Hello World!'
  })

  try {
    await client.send(createCommand)
    await client.send(putCommand)
    console.log('Successfully uploaded data to ' + bucketName + '/' + keyName)
  } catch (err) {
    console.error(err, err.stack)
  }
})()
```

The example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/nodegetstarted/sampleV3.js)\.

## Step 5: Run the Example<a name="getting-started-nodejs-run-sample"></a>

Enter the following command to run the example\.

```
node sample.js
```

If the upload is successful, you'll see a confirmation message at the command line\. You can also find the bucket and the uploaded text object in the [Amazon S3 console](https://console.aws.amazon.com/s3/)\.