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
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/getobjectcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/getobjectcommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/deletebucketcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/deletebucketcommand.html)

There is also an example that uses the following method of *node\-fetch* to generate a presigned URL:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#s3-create-presigendurl](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#s3-create-presigendurl)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#s3-create-presigendurl](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#s3-create-presigendurl)

## Prerequisite tasks<a name="s3-example-creating-buckets-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up a project environment to run Node TypeScript examples by following the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypeScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so with minor adjustments these examples can also be run in JavaScript\. For more information, see [this article](https://aws.amazon.com/blogs/developer/first-class-typescript-support-in-modular-aws-sdk-for-javascript/) in the AWS Developer Blog\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Displaying a list of Amazon S3 buckets<a name="s3-example-creating-buckets-list-buckets"></a>

Create a Node\.js module with the file name `s3_listbuckets.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. To access Amazon Simple Storage Service, create an `S3` client service object\. Call the `listBuckets` method of the Amazon S3 client service object to retrieve a list of your buckets\. The `data` parameter of the callback function has a `Buckets` property containing an array of maps to represent the buckets\. Display the bucket list by logging it to the console\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import required AWS SDK clients and commands for Node.js
const { S3Client, ListBucketsCommand } = require("@aws-sdk/client-s3");
// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"

// Create S3 service object
const s3 = new S3Client({ region: REGION });

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
const s3 = new S3Client({ region: REGION });

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

Create a variable with the parameters needed to call the `PutObjectCommand` method of the Amazon S3 service object\. Provide the name of the target bucket in the `Bucket` parameter\. The `Key` parameter is set to the name of the selected file, which you can obtain using the Node\.js `path` module\. The `Body` parameter is set to the contents of the file, which you can obtain using `createReadStream` from the Node\.js `fs` module\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import required AWS SDK clients and commands for Node.js
const { S3Client, PutObjectCommand } = require("@aws-sdk/client-s3");
const path = require("path");
const fs = require("fs");

// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const uploadParams = { Bucket: "BUCKET_NAME", Key: "KEY", Body: "BODY" }; //BUCKET_NAME, KEY (the name of the selected file),
// BODY (the contents of the uploaded file - leave blank/remove to retain contents of original file.)
const file = "FILE_NAME"; //FILE_NAME (the name of the file to upload (if you don't specify KEY))

// Create S3 service object
const s3 = new S3Client({ region: REGION });

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

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_putobject.ts)\.

## Getting a file from an Amazon S3 bucket<a name="s3-example-creating-buckets-get-object"></a>

Create a Node\.js module with the file name `s3_getobject.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `S3` client service object\. The module will take two command\-line arguments, the first one to specify the target bucket and the second to specify the file to get\.

Create a variable with the parameters needed to call the `GetObjectCommand` method of the Amazon S3 service object\. Provide the name of the target bucket in the `Bucket` parameter\. The `Key` parameter is set to the name of the file, which you can obtain using the Node\.js `path` module\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

To run the example, enter the following at the command prompt\.

```
ts-node s3_upload.ts // If you prefer JavaScript, enter 'node s3_listbuckets.js' 
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/s3/src/s3_getobject.ts)\.

## Listing objects in an Amazon S3 bucket<a name="s3-example-listing-objects"></a>

This example lists up to 1000 objects in an Amazon S3 Bucket\.

**Note**  
To list more than 1000 objects, see [Listing more than 1000 objects in an Amazon S3 bucket](#s3-example-listing-1000plus-objects)\.

Create a Node\.js module with the file name `s3_listobjects.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `S3` client service object\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

Add a variable to hold the parameters used to call the `ListObjectsCommnad` method of the Amazon S3 service object, including the name of the bucket to read\. The callback function logs a list of objects \(files\) or a failure message\.

```
// Import required AWS SDK clients and commands for Node.js
const { S3Client, ListObjectsCommand } = require("@aws-sdk/client-s3");

// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"

// Create the parameters for the bucket
const bucketParams = { Bucket: "BUCKET_NAME" };

// Create S3 service object
const s3 = new S3Client({ region: REGION });

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
const s3 = new S3Client({ region: REGION });

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
        // Assign value of pageMarker to bucketParams so that the next iteration will start from the new pageMarker.
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

## Creating a presigned URL<a name="s3-create-presigendurl"></a>

This section demonstrated how to create presigned URLs to get and put objects in Amazon S3 buckets\.

### Configuring Amazon S3 buckets<a name="s3-create-presigendurl-get"></a>

Create a Node\.js module with the file name `s3_presignedURL_v3.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `S3` client service object\. 

Create a function that creates a bucket and an object to upload, creates a presigned url to upload the object, and then uploads the object\. In this example, the object and bucket are automatically deleted to ensure you don't incur any unnecessary expense\.

Create a variable with the parameters needed to call the `PutObjectCommand` command of the Amazon S3 service object\. In the code, enter the *REGION*\. The bucket name, filename, or key, body, and duration to expiration are prepopulated in this example\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

For more information on creating presigned URLs, see [https://docs.aws.amazon.com/AmazonS3/latest/dev/PresignedUrlUploadObject.html](https://docs.aws.amazon.com/AmazonS3/latest/dev/PresignedUrlUploadObject.html)\.

```
// Import the required AWS SDK clients and commands for Node.js
const {
  S3,
  CreateBucketCommand,
  DeleteObjectCommand,
  PutObjectCommand,
  DeleteBucketCommand,
} = require("@aws-sdk/client-s3");
const { S3RequestPresigner } = require("@aws-sdk/s3-request-presigner");
const { createRequest } = require("@aws-sdk/util-create-request");
const { formatUrl } = require("@aws-sdk/util-format-url");
const fetch = require("node-fetch");

// Set the AWS Region
const REGION = "REGION";

// Set parameters
// Create a random names for the Amazon Simple Storage Service (Amazon S3) bucket and key
const clientParams = {
  Bucket: `test-bucket-${Math.ceil(Math.random() * 10 ** 10)}`,
  Key: `test-object-${Math.ceil(Math.random() * 10 ** 10)}`,
  Body: "BODY"
};

// Create Amazon S3 client object
const s3Client = new S3({ region: REGION });

//Create an S3RequestPresigner object
//To avoid redundant construction parameters when instantiating the Amazon S3 presigner,
// spread the configuration of an existing Amazon S3 client and supply it to the
// presigner's constructor.
const signedRequest = new S3RequestPresigner(s3Client.config);

const run = async () => {
  try {
    //Create an S3 bucket
    console.log(`Creating bucket ${clientParams.Bucket}`);
    await s3Client.send(
        new CreateBucketCommand({ Bucket: clientParams.Bucket })
    );
    console.log(`Waiting for "${clientParams.Bucket}" bucket creation...`);
  } catch (err) {
    console.log("Error creating bucket", err);
  }
  try {
    // Create request
    const request = await createRequest(
        s3Client,
        new PutObjectCommand(clientParams)
    );
    // Create and format presigned URL
    const signedUrl = formatUrl(
        await signedRequest.presign(request, {
          // Supply expiration in second
          expiresIn: 60 * 60 * 24
        })
    );
    console.log(
        `\nPutting "${clientParams.Key}" using signedUrl with body "${clientParams.Body}" in v3`
    );
    console.log(signedUrl);
  } catch (err) {
    console.log("Error creating presigned URL", err);
  }
  try {
    // Delete the object
    console.log(`\nDeleting object "${clientParams.Key}" from bucket`);
    await s3Client.send(new DeleteObjectCommand({Bucket:clientParams.Bucket, Key:clientParams.Key}));
  } catch (err) {
    console.log("Error deleting object", err);
  }
  try {
    // Delete the bucket
    console.log(`\nDeleting bucket ${clientParams.Bucket}`);
    await s3Client.send(new DeleteBucketCommand({Bucket:clientParams.Bucket}));
  } catch (err) {
    console.log("Error deleting bucket", err);
  }
};
run();
```

To run the example, type the following at the command line\.

```
node s3_presignedURL_v3.js
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/src/s3_put_presignedURL.ts)\.

### Create a presigned URL to Put Objects<a name="s3-create-presigendurl-put"></a>

Create a Node\.js module with the file name `s3_presignedURL_v3.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `S3` client service object\. 

Create a function that creates a bucket and an object to upload, creates a presigned url to upload the object, and then uploads the object\. In this example, the object and bucket are automatically deleted to ensure you don't incur any unnecessary expense\.

Create a variable with the parameters needed to call the `PutObjectCommand` command of the Amazon S3 service object\. In the code, enter the *REGION*\. The bucket name, filename, or key, body, and duration to expiration are prepopulated in this example\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

For more information on creating presigned URLs, see [https://docs.aws.amazon.com/AmazonS3/latest/dev/PresignedUrlUploadObject.html](https://docs.aws.amazon.com/AmazonS3/latest/dev/PresignedUrlUploadObject.html)\.

```
// Import the required AWS SDK clients and commands for Node.js
const {
  S3,
  CreateBucketCommand,
  DeleteObjectCommand,
  PutObjectCommand,
  DeleteBucketCommand,
} = require("@aws-sdk/client-s3");
const { S3RequestPresigner } = require("@aws-sdk/s3-request-presigner");
const { createRequest } = require("@aws-sdk/util-create-request");
const { formatUrl } = require("@aws-sdk/util-format-url");
const fetch = require("node-fetch");

// Set the AWS Region
const REGION = "REGION";

// Set parameters
// Create a random names for the Amazon Simple Storage Service (Amazon S3) bucket and key
const clientParams = {
  Bucket: `test-bucket-${Math.ceil(Math.random() * 10 ** 10)}`,
  Key: `test-object-${Math.ceil(Math.random() * 10 ** 10)}`,
  Body: "BODY"
};

// Create Amazon S3 client object
const s3Client = new S3({ region: REGION });

//Create an S3RequestPresigner object
//To avoid redundant construction parameters when instantiating the Amazon S3 presigner,
// spread the configuration of an existing Amazon S3 client and supply it to the
// presigner's constructor.
const signedRequest = new S3RequestPresigner(s3Client.config);

const run = async () => {
  try {
    //Create an S3 bucket
    console.log(`Creating bucket ${clientParams.Bucket}`);
    await s3Client.send(
        new CreateBucketCommand({ Bucket: clientParams.Bucket })
    );
    console.log(`Waiting for "${clientParams.Bucket}" bucket creation...`);
  } catch (err) {
    console.log("Error creating bucket", err);
  }
  try {
    // Create request
    const request = await createRequest(
        s3Client,
        new PutObjectCommand(clientParams)
    );
    // Create and format presigned URL
    const signedUrl = formatUrl(
        await signedRequest.presign(request, {
          // Supply expiration in second
          expiresIn: 60 * 60 * 24
        })
    );
    console.log(
        `\nPutting "${clientParams.Key}" using signedUrl with body "${clientParams.Body}" in v3`
    );
    console.log(signedUrl);
  } catch (err) {
    console.log("Error creating presigned URL", err);
  }
  try {
    // Delete the object
    console.log(`\nDeleting object "${clientParams.Key}" from bucket`);
    await s3Client.send(new DeleteObjectCommand({Bucket:clientParams.Bucket, Key:clientParams.Key}));
  } catch (err) {
    console.log("Error deleting object", err);
  }
  try {
    // Delete the bucket
    console.log(`\nDeleting bucket ${clientParams.Bucket}`);
    await s3Client.send(new DeleteBucketCommand({Bucket:clientParams.Bucket}));
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

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/src/s3_put_presignedURL.ts)\.

### Create a presigned URL to get objects from an Amazon S3 bucket<a name="s3-create-presigendurl-get"></a>

Create a Node\.js module with the file name `s3_get_presignedURL_v3.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `S3` client service object\. 

Create a function that creates a bucket and uploads, and object, then creates a presigned url to get the object from the bucket\. In this example, the object and bucket are automatically deleted to ensure you don't incur any unnecessary expense\.

Create a variable with the parameters needed to call the `GetObjectCommand` command of the Amazon S3 service object\. In the code, enter the *REGION*\. The bucket name, filename, or key, body, and duration to expiration are prepopulated in this example\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

For more information on creating presigned URLs, see [https://docs.aws.amazon.com/AmazonS3/latest/dev/PresignedUrlUploadObject.html](https://docs.aws.amazon.com/AmazonS3/latest/dev/PresignedUrlUploadObject.html)\.

```
const {
 S3,
 CreateBucketCommand,
 PutObjectCommand,
 GetObjectCommand,
 DeleteObjectCommand,
 DeleteBucketCommand,
} = require("@aws-sdk/client-s3");
const { S3RequestPresigner } = require("@aws-sdk/s3-request-presigner");
const { createRequest } = require("@aws-sdk/util-create-request");
const { formatUrl } = require("@aws-sdk/util-format-url");
const fetch = require("node-fetch");

// Set parameters
// Create a random names for the Amazon Simple Storage Service (Amazon S3) bucket and key
const params = {
 Bucket: `test-bucket-${Math.ceil(Math.random() * 10 ** 10)}`,
 Key: `test-object-${Math.ceil(Math.random() * 10 ** 10)}`,
 Body: "BODY",
 Region: "REGION"
};

// Create Amazon S3 client object
const s3Client = new S3({ region: params.Region });

// Create an S3RequestPresigner object
const signedRequest = new S3RequestPresigner(s3Client.config);

const run = async () => {
 let signedUrl;
 let response;
 // Create Amazon Simple Storage Service (Amazon S3) bucket
 try {
 console.log(`Creating bucket ${params.Bucket}`);
 const data = await s3Client.send(
 new CreateBucketCommand({ Bucket: params.Bucket })
 );
 console.log(`Waiting for "${params.Bucket}" bucket creation...\n`);
 } catch (err) {
 console.log("Error creating bucket", err);
 }
 // Put object in Amazon S3 bucket
 try {
 console.log(`Putting object "${params.Key}" in bucket`);
 const data = await s3Client.send(
 new PutObjectCommand({ Bucket: params.Bucket, Key: params.Key, Body: params.Body })
 );
 } catch (err) {
 console.log("Error putting object", err);
 }
 // Create presigned URL
 try {
 // Create request
 const request = await createRequest(s3Client, new GetObjectCommand(params));
 // Create and format presigned URL
 const signedUrl = formatUrl(
 await signedRequest.presign(request, {
 // Supply expiration in seconds
 expiresIn: 60 * 60 * 24,
 })
 );
 console.log(
 `\nPutting "${params.Key}" using signedUrl with body "${params.Body}"`
 );
 console.log(signedUrl);
 response = await fetch(signedUrl);
 console.log(
 `\nResponse returned by signed URL: ${await response.text()}\n`
 );
 } catch (err) {
 console.log("Error creating presigned URL", err);
 }
 // Delete object
 try {
 console.log(`\nDeleting object "${params.Key}" from bucket`);
 const data = await s3Client.send(new
 DeleteObjectCommand({ Bucket: params.Bucket, Key: params.Key })
 );
 } catch (err) {
 console.log("Error deleting object", err);
 }
 // Delete bucket
 try {
 console.log(`\nDeleting bucket ${params.Bucket}`);
 const data = await s3Client.send(new
 DeleteBucketCommand({ Bucket: params.Bucket, Key: params.Key })
 );
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

<<<<<<< HEAD
This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/src/s3_get_presignedURL.ts)\.
=======
This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/src/s3_put_presignedURL.ts)\.
>>>>>>> Import S3Client in ListBucketsCommand example
