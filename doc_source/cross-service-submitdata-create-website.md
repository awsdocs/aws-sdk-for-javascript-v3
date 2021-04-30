--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Hosting the app on Amazon S3<a name="cross-service-submitdata-create-website"></a>

This topic is part of a larger tutorial about using the AWS SDK for JavaScript with AWS Lambda functions\. To start at the beginning of the tutorial, see [Creating and using Lambda functions](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/sdk-cross-service-example-submitting-data.html)\. 

In this task, you upload the app files to an Amazon S3 bucket, and convert that bucket into a static web host\. 

To host the app in Amazon S3 bucket you created, you first need to upload your `index.html`, `error.html`, and `main.js` files to the bucket\. Then you convert the bucket into a static web host\. Finally, you need to apply the required permissions policy to the bucket\.

To upload the files, in `DynamoDBApp`, create a Node\.js module with the file name `upload_files_to_s3.ts`\.

Make sure to configure the SDK as previously shown, including installing the required clients and packages\. In `upload_files_to_s3.ts`, load the required client modules — `CognitoIdentityClient`, `fromCognitoIdentityPool`, `DynamoDB`, and `SNSClient` — and commands\. Then create an `S3` client service object, and initialize the `Cognito` credentials provider to provide the required credentials\. Then create a `Cognito` client service object, and initialize the `Cognito` credentials provider to provide the required credentials\. Declare variables to the bucket, and each of the files to upload\. Create an `S3Client` client object\.

To upload the files, enter the following at the command prompt\.

```
ts-node upload_files_to_s3.ts
```

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  S3Client,
  PutBucketWebsiteCommand,
  PutObjectCommand
} = require("@aws-sdk/client-s3");
import path from "path";
import fs from "fs";

// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"
// Set the bucket parameters
const bucketParams = { Bucket: "BUCKET_NAME" };
const uploadParams1 = { Bucket: bucketParams.Bucket, Key: "index.html" };
const uploadParams2 = { Bucket: bucketParams.Bucket, Key: "error.html" };
const uploadParams3 = { Bucket: bucketParams.Bucket, Key: "main.js" };

var file1 = "index.html";
var file2 = "error.html";
var file3 = "main.js";

// Instantiate S3client and S3 client objects
const s3Client = new S3Client({});

//Attempt to create the bucket
const run = async () => {
  try {
    const fileStream1 = fs.createReadStream(file1);
    fileStream1.on("error", function (err) {
      console.log("File Error", err);
    });
    uploadParams1.Body = fileStream1;
    var path = require("path");
    uploadParams1.Key = path.basename(file1);
    // call S3 to retrieve upload file to specified bucket
    try {
      const data = await s3Client.send(new PutObjectCommand(uploadParams1));
      console.log("Success", data);
    } catch (err) {
      console.log("Error", err);
    }
    const fileStream2 = fs.createReadStream(file2);
    fileStream2.on("error", function (err) {
      console.log("File Error", err);
    });
    uploadParams2.Body = fileStream2;
    var path = require("path");
    uploadParams2.Key = path.basename(file2);
    // call S3 to retrieve upload file to specified bucket
    try {
      const data = await s3Client.send(new PutObjectCommand(uploadParams2));
      console.log("Success", data);
    } catch (err) {
      console.log("Error", err);
    }
    const fileStream3 = fs.createReadStream(file3);
    fileStream3.on("error", function (err) {
      console.log("File Error", err);
    });
    uploadParams3.Body = fileStream3;
    var path = require("path");
    uploadParams3.Key = path.basename(file3);
    // call S3 to retrieve upload file to specified bucket
    try {
      const data = await s3Client.send(new PutObjectCommand(uploadParams3));
      console.log("Success", data);
    } catch (err) {
      console.log("Error", err);
    }
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/submit-data-app/src/dynamoAppHelperFiles/upload-files-to-s3.ts)\.

To convert the `S3` bucket to a static web host, in `DynamoDBAppHelperFiles`, create a Node\.js module with the file name `convert-bucket-to-website.ts`\.

Make sure to configure the SDK as previously shown, including installing the required clients and packages\. In `convert-bucket-to-website.ts`, load the `SNSClient` client module\. Create a parameters object to define the parameters of the static host\. Declare variables to the bucket, and each of the files to upload\. Create an `S3Client` client object\.

To convert the Amazon S3 bucket , enter the following at the command prompt\.

```
ts-node convert-bucket-to-website.ts
```

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
const {
  S3Client,
  CreateBucketCommand,
  PutBucketWebsiteCommand,
  PutBucketPolicyCommand,
} = require("@aws-sdk/client-s3");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Create params JSON for S3.createBucket
const bucketName = "BUCKET_NAME"; //BUCKET_NAME

const readOnlyAnonUserPolicy = {
  Version: "2012-10-17",
  Statement: [
    {
      Sid: "AddPerm",
      Effect: "Allow",
      Principal: "*",
      Action: ["s3:GetObject"],
      Resource: [""],
    },
  ],
};

// create selected bucket resource string for bucket policy
var bucketResource = "arn:aws:s3:::" + bucketName + "/*"; //BUCKET_NAME
readOnlyAnonUserPolicy.Statement[0].Resource[0] = bucketResource;

// convert policy JSON into string and assign into params
var bucketPolicyParams = {
  Bucket: bucketName,
  Policy: JSON.stringify(readOnlyAnonUserPolicy),
};

// Instantiate an S3 client
const s3 = new S3Client({ region: REGION });

const run = async () => {
  try {
    const response = await s3.send(
      new PutBucketPolicyCommand(bucketPolicyParams)
    );
    console.log("Success, permissions added to bucket", response);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/submit-data-app/src/dynamoAppHelperFiles/convert-bucket-to-website.ts)\.