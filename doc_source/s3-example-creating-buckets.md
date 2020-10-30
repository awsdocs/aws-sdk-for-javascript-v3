--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Creating and using Amazon S3 buckets<a name="s3-example-creating-buckets"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to obtain and display a list of Amazon S3 buckets in your account\.
+ How to create an Amazon S3 bucket\.
+ How to upload an object to a specified bucket\.

## The scenario<a name="s3-example-creating-buckets-scenario"></a>

In this example, a series of Node\.js modules are used to obtain a list of existing Amazon S3 buckets, create a bucket, and upload a file to a specified bucket\. These Node\.js modules use the SDK for JavaScript to get information from and upload files to an Amazon S3 bucket using these methods of the Amazon S3 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#listBuckets-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#listBuckets-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#createBucket-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#createBucket-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#listObjects-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#listObjects-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#upload-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#upload-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#deleteBucket-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#deleteBucket-property)

## Prerequisite tasks<a name="s3-example-creating-buckets-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so with minor adjustments these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Displaying a list of Amazon S3 buckets<a name="s3-example-creating-buckets-list-buckets"></a>

Create a Node\.js module with the file name `s3_listbuckets.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. To access Amazon Simple Storage Service, create an `S3` client service object\. Call the `listBuckets` method of the Amazon S3 client service object to retrieve a list of your buckets\. The `data` parameter of the callback function has a `Buckets` property containing an array of maps to represent the buckets\. Display the bucket list by logging it to the console\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  ListObjectsCommand,
  ListBucketsCommand,
} = require("@aws-sdk/client-s3");
// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"

// Create S3 service object
const s3 = new ListObjectsCommand(REGION);

const run = async () => {
  try {
    const data = await s3.send(new ListBucketsCommand({}));
    console.log("Success", data.Buckets);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node s3_listbuckets.ts // If you prefer JavaScript, enter 'node s3_listbuckets.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_listbuckets.ts)\.

## Creating an Amazon S3 bucket<a name="s3-example-creating-buckets-new-bucket"></a>

Create a Node\.js module with the file name `s3_createbucket.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `S3` client service object\. The module will take a single command\-line argument to specify a name for the new bucket\.

Add a variable to hold the parameters used to call the `createBucket` method of the Amazon S3 client service object, including the name for the newly created bucket\. The callback function logs the new bucket's location to the console after Amazon S3 successfully creates it\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import required AWS SDK clients and commands for Node.js
const { S3Client, CreateBucketCommand } = require("@aws-sdk/client-s3");

// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the bucket parameters
const bucketParams = { Bucket: "BUCKET_NAME" };

// Create S3 service object
const s3 = new S3Client(REGION);

//Attempt to create the bucket
const run = async () => {
  try {
    const data = await s3.send(new CreateBucketCommand(bucketParams));
    console.log("Success", data.$metadata.httpHeaders.location);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node s3_createbucket.ts // If you prefer JavaScript, enter 'node s3_listbuckets.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_createbucket.ts)\.

## Uploading a file to an Amazon S3 bucket<a name="s3-example-creating-buckets-upload-file"></a>

Create a Node\.js module with the file name `s3_upload.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `S3` client service object\. The module will take two command\-line arguments, the first one to specify the destination bucket and the second to specify the file to upload\.

Create a variable with the parameters needed to call the `putObject` method of the Amazon S3 service object\. Provide the name of the target bucket in the `Bucket` parameter\. The `Key` parameter is set to the name of the selected file, which you can obtain using the Node\.js `path` module\. The `Body` parameter is set to the contents of the file, which you can obtain using `createReadStream` from the Node\.js `fs` module\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import required AWS SDK clients and commands for Node.js
const { S3Client } = require("@aws-sdk/client-s3");
const path = require("path");
const fs = require("fs");

// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const uploadParams = { Bucket: "BUCKET_NAME", Key: "KEY", Body: "BODY" }; //BUCKET_NAME, KEY (the name of the selected file),
// BODY (the contents of the uploaded file - leave blank/remove to retain contents of original file.)
const file = "FILE_NAME"; //FILE_NAME (the name of the file to upload (if you don't specify KEY))

// Create S3 service object
const s3 = new S3Client(REGION);

// call S3 to retrieve upload file to specified bucket
const run = async () => {
  // Configure the file stream and obtain the upload parameters
  const fileStream = fs.createReadStream(file);
  fileStream.on("error", function (err) {
    console.log("File Error", err);
  });
  uploadParams.Key = path.basename(file);
  // call S3 to retrieve upload file to specified bucket
  try {
    const data = await s3.send(new PutObjectCommand(uploadParams));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node s3_upload.ts // If you prefer JavaScript, enter 'node s3_listbuckets.js' 
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_upload.ts)\.

## Listing objects in an Amazon S3 bucket<a name="s3-example-listing-objects"></a>

This example lists up to 1000 objects in an Amazon S3 Bucket\.

**Note**  
To list more than 1000 objects, see [Listing more than 1000 objects in an Amazon S3 bucket](#s3-example-listing-1000plus-objects)\.

Create a Node\.js module with the file name `s3_listobjects.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `S3` client service object\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

Add a variable to hold the parameters used to call the `listObjects` method of the Amazon S3 service object, including the name of the bucket to read\. The callback function logs a list of objects \(files\) or a failure message\.

```
// Import required AWS SDK clients and commands for Node.js
const { S3Client, ListObjectsCommand } = require("@aws-sdk/client-s3");

// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"

// Create the parameters for the bucket
const bucketParams = { Bucket: "BUCKET_NAME" };

// Create S3 service object
const s3 = new S3Client(REGION);

const run = async () => {
  try {
    const data = await s3.send(new ListObjectsCommand(bucketParams));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node s3_listobjects.ts // If you prefer JavaScript, enter 'node s3_listbuckets.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_listobjects.ts)\.

## Listing more than 1000 objects in an Amazon S3 bucket<a name="s3-example-listing-1000plus-objects"></a>

This example lists more than 1000 objects in an Amazon S3 Bucket\.

Create a Node\.js module with the file name `s3_list1000plusobjects.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `S3` client service object\. 

Use a while loop to list each 1000 items until all items have been listed\. Then declare `truncated` as a flag with a value of `true`, and a while loop that prints 1,000 items at at time, until the the flag is `false`\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import required AWS SDK clients and commands for Node.js
const { S3Client, ListObjectsCommand } = require("@aws-sdk/client-s3");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Create the parameters for the bucket
const bucketParams = { Bucket: "BUCKET_NAME" };

// Create S3 service object
const s3 = new S3Client(REGION);

async function run() {
  // Declare truncated as a flag that we will base our while loop on
  let truncated = true;
  // Declare a variable that we will assign the key of the last element in the response to
  let pageMarker;
  // While loop that runs until response.truncated is false
  while (truncated) {
    try {
      const response = await s3.send(new ListObjectsCommand(bucketParams));
      response.Contents.forEach((item) => {
        console.log(item.Key);
      });
      // Log the Key of every item in the response to standard output
      truncated = response.IsTruncated;
      // If 'truncated' is true, assign the key of the final element in the response to our variable 'pageMarker'
      if (truncated) {
        pageMarker = response.Contents.slice(-1)[0].Key;
      }
      // At end of the list, response.truncated is false and our function exits the while loop.
    } catch (err) {
      console.log("Error", err);
      truncated = false;
    }
  }
}
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node s3_list1000plusobjects.ts // If you prefer JavaScript, enter 'node s3_listbuckets.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_list1000plusobjects.ts)\.

## Deleting an Amazon S3 bucket<a name="s3-example-deleting-buckets"></a>

Create a Node\.js module with the file name `s3_deletebucket.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `S3` client service object\. 

Add a variable to hold the parameters used to call the `deleteBucket` method of the Amazon S3 service object, including the name of the bucket to delete\. The bucket must be empty to delete it\. The callback function logs a success or failure message\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import required AWS SDK clients and commands for Node.js
const { S3Client, DeleteBucketCommand } = require("@aws-sdk/client-s3/");

// Set the AWS region
const REGION = "region"; //e.g. "us-east-1"

// Set the bucket parameters
const bucketParams = { Bucket: "BUCKET_NAME" };

// Create S3 service object
const s3 = new S3Client();

const run = async () => {
  try {
    const data = await s3.send(new DeleteBucketCommand(bucketParams));
    console.log("Success - bucket deleted");
  } catch (err) {
    console.log("Error", err);
  }
};
// Invoke run() so these examples run out of the box.
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node s3_deletebucket.ts // If you prefer JavaScript, enter 'node s3_listbuckets.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_deletebucket.ts)\.