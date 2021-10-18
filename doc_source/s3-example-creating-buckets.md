--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Creating and using Amazon S3 buckets<a name="s3-example-creating-buckets"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to obtain and display a list of Amazon S3 buckets in your account\.
+ How to create an Amazon S3 bucket\.
+ How to upload an object to a specified bucket\.

## The scenario<a name="s3-example-creating-buckets-scenario"></a>

In this example, a series of Node\.js modules are used to obtain a list of existing Amazon S3 buckets, create a bucket, and upload a file to a specified bucket\. These Node\.js modules use the SDK for JavaScript to get information from and upload files to an Amazon S3 bucket using these methods of the Amazon S3 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/listbucketscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/listbucketscommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/createbucketcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/createbucketcommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/listobjectscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/listobjectscommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/putobjectcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/putobjectcommand.html)
+  [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/uploadpartcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/uploadpartcommand.html), 
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/getobjectcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/getobjectcommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/deletebucketcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/deletebucketcommand.html)

There is also an example that uses the following method of *node\-fetch* to generate a presigned URL:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#s3-create-presigendurl](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#s3-create-presigendurl)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#s3-create-presigendurl](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#s3-create-presigendurl)

## Prerequisite tasks<a name="s3-example-creating-buckets-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up a project environment to run Node JavaScript examples by following the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Displaying a list of Amazon S3 buckets<a name="s3-example-creating-buckets-list-buckets"></a>

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

Create a Node\.js module with the file name `s3_listbuckets.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. To access Amazon Simple Storage Service, create an `S3` client service object\. Call the `listBuckets` method of the Amazon S3 client service object to retrieve a list of your buckets\. The `data` parameter of the callback function has a `Buckets` property containing an array of maps to represent the buckets\. Display the bucket list by logging it to the console\.

```
// Import required AWS SDK clients and commands for Node.js
import { ListBucketsCommand } from "@aws-sdk/client-s3";
import { s3Client } from "./libs/s3Client.js"; // Helper function that creates Amazon S3 service client module.

export const run = async () => {
  try {
    const data = await s3Client.send(new ListBucketsCommand({}));
    console.log("Success", data.Buckets);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node s3_listbuckets.js
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_listbuckets.js)\.

## Creating an Amazon S3 bucket<a name="s3-example-creating-buckets-new-bucket-2"></a>

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

Create a Node\.js module with the file name `s3_createbucket.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `S3` client service object\. The module will take a single command\-line argument to specify a name for the new bucket\.

Add a variable to hold the parameters used to call the `createBucket` method of the Amazon S3 client service object, including the name for the newly created bucket\. The callback function logs the new bucket's location to the console after Amazon S3 successfully creates it\.

```
// Get service clients module and commands using ES6 syntax.
 import { CreateBucketCommand } from "@aws-sdk/client-s3";
 import { s3 } from "./libs/s3Client.js";

// Set the bucket parameters
const bucketParams = { Bucket: "BUCKET_NAME" };

// Create the Amazon S3 bucket.
const run = async () => {
  try {
    const data = await s3.send(new CreateBucketCommand(bucketParams));
    console.log("Success", data.Location);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node s3_createbucket.js
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_createbucket.js)\.

## Uploading a file to an Amazon S3 bucket<a name="s3-example-creating-buckets-upload-file"></a>

This section describes how to:
+ Create a new object and upload it to an Amazon S3 bucket\.
+ Upload an existing object to an Amazon S3 bucket\.

### Create and upload an object to an Amazon S3 bucket<a name="s3-example-upload-new-object"></a>

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

Create a Node\.js module with the file name `s3_create_and_upload_object.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. 

Create a variable with the parameters needed to call the `PutObjectCommand` method of the Amazon S3 service object\. Provide the name of the target bucket in the `Bucket` parameter\. For the `Key` parameter, provide a name for the object\. 

**Note**  
To create a directory for the object, use the format `directoryY_NAME/OBJECT_NAME`\.

```
// Import required AWS SDK clients and commands for Node.js
import { PutObjectCommand } from "@aws-sdk/client-s3";
import { s3Client } from "./libs/s3Client.js"; // Helper function that creates Amazon S3 service client module.

// Set the parameters.
export const bucketParams = {
  Bucket: "BUCKET_NAME",
  // Specify the name of the new object. For example, 'index.html'.
  // To create a directory for the object, use '/'. For example, 'myApp/package.json'.
  Key: "OBJECT_NAME",
  // Content of the new object.
  Body: "BODY",
};

// Create and upload the object to the specified Amazon S3 bucket.
export const run = async () => {
  try {
    const data = await s3Client.send(new PutObjectCommand(bucketParams));
    return data; // For unit tests.
    console.log(
      "Successfully uploaded object: " +
        bucketParams.Bucket +
        "/" +
        bucketParams.Key
    );
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node s3_create_and_upload_object.js 
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_create_and_upload_object.js)\.

### Upload an existing object to an Amazon S3 bucket<a name="s3-example-upload-existing-object"></a>

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

Create a Node\.js module with the file name `s3_upload_object.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. 

Create a variable with the parameters needed to call the `PutObjectCommand` method of the Amazon S3 service object\. Provide the name of the target bucket in the `Bucket` parameter\. Provide the name of the existing object and the path to it\. The `Key` parameter is set to the name of the selected file, which you can obtain using the Node\.js `path` module\. 

```
// Import required AWS SDK clients and commands for Node.js.
import { PutObjectCommand } from "@aws-sdk/client-s3";
import { s3Client } from "./libs/s3Client.js"; // Helper function that creates Amazon S3 service client module.
import {path} from "path";
import {fs} from "fs";

const file = "OBJECT_PATH_AND_NAME"; // Path to and name of object. For example '../myFiles/index.js'.
const fileStream = fs.createReadStream(file);

// Set the parameters
export const uploadParams = {
  Bucket: "BUCKET_NAME",
  // Add the required 'Key' parameter using the 'path' module.
  Key: path.basename(file),
  // Add the required 'Body' parameter
  Body: fileStream,
};


// Upload file to specified bucket.
export const run = async () => {
  try {
    const data = await s3Client.send(new PutObjectCommand(uploadParams));
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
node s3_upload_object.js 
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_upload_object.js)\.

## Getting a file from an Amazon S3 bucket<a name="s3-example-creating-buckets-get-object"></a>

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

Create a Node\.js module with the file name `s3_getobject.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. The module will take two command\-line arguments, the first one to specify the target bucket and the second to specify the file to get\.

Create a variable with the parameters needed to call the `GetObjectCommand` method of the Amazon S3 service object\. Provide the name of the target bucket in the `Bucket` parameter\. The `Key` parameter is set to the name of the file, which you can obtain using the Node\.js `path` module\. 

```
// Import required AWS SDK clients and commands for Node.js.
import { GetObjectCommand } from "@aws-sdk/client-s3";
import { s3Client } from "./libs/s3Client.js"; // Helper function that creates Amazon S3 service client module.

export const bucketParams = {
  Bucket: "BUCKET_NAME",
  Key: "KEY",
};

export const run = async () => {
  try {
    // Create a helper function to convert a ReadableStream to a string.
    const streamToString = (stream) =>
      new Promise((resolve, reject) => {
        const chunks = [];
        stream.on("data", (chunk) => chunks.push(chunk));
        stream.on("error", reject);
        stream.on("end", () => resolve(Buffer.concat(chunks).toString("utf8")));
      });

    // Get the object} from the Amazon S3 bucket. It is returned as a ReadableStream.
    const data = await s3Client.send(new GetObjectCommand(bucketParams));
      return data; // For unit tests.
    // Convert the ReadableStream to a string.
    const bodyContents = await streamToString(data.Body);
    console.log(bodyContents);
      return bodyContents;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node s3_upload.js  
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_getobject.js)\.

## Listing objects in an Amazon S3 bucket<a name="s3-example-listing-objects"></a>

This example lists up to 1000 objects in an Amazon S3 Bucket\.

**Note**  
To list more than 1000 objects, see [Listing more than 1000 objects in an Amazon S3 bucket](#s3-example-listing-1000plus-objects)\.

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

Create a Node\.js module with the file name `s3_listobjects.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. 

Add a variable to hold the parameters used to call the `ListObjectsCommnad` method of the Amazon S3 service object, including the name of the bucket to read\. The callback function logs a list of objects \(files\) or a failure message\.

```
// Import required AWS SDK clients and commands for Node.js
import { ListObjectsCommand } from "@aws-sdk/client-s3";
import { s3Client } from "./libs/s3Client.js"; // Helper function that creates Amazon S3 service client module.

// Create the parameters for the bucket
export const bucketParams = { Bucket: "BUCKET_NAME" };

export const run = async () => {
  try {
    const data = await s3Client.send(new ListObjectsCommand(bucketParams));
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
node s3_listobjects.js 
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_listobjects.js)\.

## Listing more than 1000 objects in an Amazon S3 bucket<a name="s3-example-listing-1000plus-objects"></a>

This example lists more than 1000 objects in an Amazon S3 Bucket\.

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

Create a Node\.js module with the file name `s3_list1000plusobjects.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `S3` client service object\. 

Use a while loop to list each 1000 items until all items have been listed\. Then declare `truncated` as a flag with a value of `true`, and a while loop that prints 1,000 items at at time, until the the flag is `false`\.

```
// Import required AWS SDK clients and commands for Node.js
import { ListObjectsCommand } from "@aws-sdk/client-s3";
import { s3Client } from "./libs/s3Client.js"; // Helper function that creates Amazon S3 service client module.

// Create the parameters for the bucket
export const bucketParams = { Bucket: "BUCKET_NAME" };

export async function run() {
  // Declare truncated as a flag that we will base our while loop on
  let truncated = true;
  // Declare a variable that we will assign the key of the last element in the response to
  let pageMarker;
  // While loop that runs until response.truncated is false
  while (truncated) {
    try {
      const response = await s3Client.send(new ListObjectsCommand(bucketParams));
      // return response; //For unit tests
      response.Contents.forEach((item) => {
        console.log(item.Key);
      });
      // Log the Key of every item in the response to standard output
      truncated = response.IsTruncated;
      // If 'truncated' is true, assign the key of the final element in the response to our variable 'pageMarker'
      if (truncated) {
        pageMarker = response.Contents.slice(-1)[0].Key;
        // Assign value of pageMarker to bucketParams so that the next iteration will start} from the new pageMarker.
        bucketParams.Marker = pageMarker;
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
node s3_list1000plusobjects.js 
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_list1000plusobjects.js)\.

## Deleting an Amazon S3 bucket<a name="s3-example-deleting-buckets"></a>

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

Create a Node\.js module with the file name `s3_deletebucket.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. 

Add a variable to hold the parameters used to call the `deleteBucket` method of the Amazon S3 service object, including the name of the bucket to delete\. The bucket must be empty to delete it\. The callback function logs a success or failure message\.

```
// Import required AWS SDK clients and commands for Node.js
import { DeleteBucketCommand } from "@aws-sdk/client-s3/";
import { s3Client } from "./libs/s3Client.js"; // Helper function that creates Amazon S3 service client module.

// Set the bucket parameters
export const bucketParams = { Bucket: "BUCKET_NAME" };

export const run = async () => {
  try {
    const data = await s3Client.send(new DeleteBucketCommand(bucketParams));
    return data; // For unit tests.
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
node s3_deletebucket.js 
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_deletebucket.js)\.

## Creating a presigned URL<a name="s3-create-presigendurl"></a>

This section demonstrated how to create presigned URLs to get and put objects in Amazon S3 buckets\.

### Create a presigned URL to upload objects to an Amazon S3 bucket<a name="s3-create-presigendurl-put"></a>

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

Create a Node\.js module with the file name `s3_presignedURL_v3.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. 

Create a function that creates a bucket and an object to upload, creates a presigned url to upload the object, and then uploads the object\. In this example, the object and bucket are automatically deleted to ensure you don't incur any unnecessary expense\.

Create a variable with the parameters needed to call the `PutObjectCommand` command of the Amazon S3 service object\. The bucket name, filename, or key, body, and duration to expiration are prepopulated in this example\.

For more information on creating presigned URLs, see [https://docs.aws.amazon.com/AmazonS3/latest/dev/PresignedUrlUploadObject.html](https://docs.aws.amazon.com/AmazonS3/latest/dev/PresignedUrlUploadObject.html)\.

```
// Import the required AWS SDK clients and commands for Node.js
import {
  CreateBucketCommand,
  DeleteObjectCommand,
  PutObjectCommand,
  DeleteBucketCommand }
from "@aws-sdk/client-s3";
import { s3Client } from "./libs/s3Client.js"; // Helper function that creates Amazon S3 service client module.
import { getSignedUrl } from "@aws-sdk/s3-request-presigner";
import fetch from "node-fetch";

// Set parameters
// Create a random names for the Amazon Simple Storage Service (Amazon S3) bucket and key
export const bucketParams = {
  Bucket: `test-bucket-${Math.ceil(Math.random() * 10 ** 10)}`,
  Key: `test-object-${Math.ceil(Math.random() * 10 ** 10)}`,
  Body: "BODY"
};
export const run = async () => {
  try {
    // Create an Amazon S3 bucket.
    console.log(`Creating bucket ${bucketParams.Bucket}`);
    await s3Client.send(new CreateBucketCommand({ Bucket: bucketParams.Bucket }));
    console.log(`Waiting for "${bucketParams.Bucket}" bucket creation...`);
  } catch (err) {
    console.log("Error creating bucket", err);
  }
  try {
    // Create the command.
    const command = new PutObjectCommand(bucketParams);

    // Create the presigned URL.
    const signedUrl = await getSignedUrl(s3Client, command, {
      expiresIn: 3600,
    });
    console.log(
      `\nPutting "${bucketParams.Key}" using signedUrl with body "${bucketParams.Body}" in v3`
    );
    console.log(signedUrl);
    const response = await fetch(signedUrl);
    console.log(
      `\nResponse returned by signed URL: ${await response.text()}\n`
    );
    return response;
  } catch (err) {
    console.log("Error creating presigned URL", err);
  }
  try {
    // Delete the object.
    console.log(`\nDeleting object "${bucketParams.Key}"} from bucket`);
    await s3Client.send(
      new DeleteObjectCommand({ Bucket: bucketParams.Bucket, Key: bucketParams.Key })
    );
  } catch (err) {
    console.log("Error deleting object", err);
  }
  try {
    // Delete the Amazon S3 bucket.
    console.log(`\nDeleting bucket ${bucketParams.Bucket}`);
    await s3.send(new DeleteBucketCommand({ Bucket: bucketParams.Bucket }));
  } catch (err) {
    console.log("Error deleting bucket", err);
  }
};
run();
```

To run the example, type the following at the command line\.

```
node s3_put_presignedURL_v3.js
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/src/s3_put_presignedURL.js)\.

### Create a presigned URL to get objects from an Amazon S3 bucket<a name="s3-create-presigendurl-get"></a>

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

Create a Node\.js module with the file name `s3_get_presignedURL_v3.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. 

Create a function that creates a bucket and uploads, and object, then creates a presigned url to get the object from the bucket\. In this example, the object and bucket are automatically deleted to ensure you don't incur any unnecessary expense\.

Create a variable with the parameters needed to call the `GetObjectCommand` command of the Amazon S3 service object\. The bucket name, filename, or key, body, and duration to expiration are prepopulated in this example\.

For more information on creating presigned URLs, see [https://docs.aws.amazon.com/AmazonS3/latest/dev/PresignedUrlUploadObject.html](https://docs.aws.amazon.com/AmazonS3/latest/dev/PresignedUrlUploadObject.html)\.

```
// Import the required AWS SDK clients and commands for Node.js
import {
  CreateBucketCommand,
  PutObjectCommand,
  GetObjectCommand,
  DeleteObjectCommand,
  DeleteBucketCommand }
from "@aws-sdk/client-s3";
import { s3Client } from "./libs/s3Client.js"; // Helper function that creates Amazon S3 service client module.
import { getSignedUrl } from "@aws-sdk/s3-request-presigner";
const fetch = require("node-fetch");

// Set parameters
// Create random names for the Amazon Simple Storage Service (Amazon S3) bucket and key.
export const bucketParams = {
  Bucket: `test-bucket-${Math.ceil(Math.random() * 10 ** 10)}`,
  Key: `test-object-${Math.ceil(Math.random() * 10 ** 10)}`,
  Body: "BODY"
};

export const run = async () => {
  // Create an Amazon S3 bucket.
  try {
    console.log(`Creating bucket ${bucketParams.Bucket}`);
    const data = await s3Client.send(
      new CreateBucketCommand({ Bucket: bucketParams.Bucket })
    );
    return data; // For unit tests.
    console.log(`Waiting for "${bucketParams.Bucket}" bucket creation...\n`);
  } catch (err) {
    console.log("Error creating bucket", err);
  }
  // Put the object in the Amazon S3 bucket.
  try {
    console.log(`Putting object "${bucketParams.Key}" in bucket`);
    const data = await s3Client.send(
      new PutObjectCommand({
        Bucket: bucketParams.Bucket,
        Key: bucketParams.Key,
        Body: bucketParams.Body,
      })
    );
    return data; // For unit tests.
  } catch (err) {
    console.log("Error putting object", err);
  }
  // Create a presigned URL.
  try {
    // Create the command.
    const command = new GetObjectCommand(bucketParams);

    // Create the presigned URL.
    const signedUrl = await getSignedUrl(s3Client, command, {
      expiresIn: 3600,
    });
    console.log(
      `\nGetting "${bucketParams.Key}" using signedUrl with body "${bucketParams.Body}" in v3`
    );
    console.log(signedUrl);
    const response = await fetch(signedUrl);
    console.log(
      `\nResponse returned by signed URL: ${await response.text()}\n`
    );
  } catch (err) {
    console.log("Error creating presigned URL", err);
  }
  // Delete the object.
  try {
    console.log(`\nDeleting object "${bucketParams.Key}"} from bucket`);
    const data = await s3Client.send(
      new DeleteObjectCommand({ Bucket: bucketParams.Bucket, Key: bucketParams.Key })
    );
    return data; // For unit tests.
  } catch (err) {
    console.log("Error deleting object", err);
  }
  // Delete the bucket.
  try {
    console.log(`\nDeleting bucket ${bucketParams.Bucket}`);
    const data = await s3Client.send(
      new DeleteBucketCommand({ Bucket: bucketParams.Bucket, Key: bucketParams.Key })
    );
    return data; // For unit tests.
  } catch (err) {
    console.log("Error deleting object", err);
  }
};
run();
```

To run the example, type the following at the command line\.

```
node s3_get_presignedURL_v3.js
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/src/s3_get_presignedURL.js)\.