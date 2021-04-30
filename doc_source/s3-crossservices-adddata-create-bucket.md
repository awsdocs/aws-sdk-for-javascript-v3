--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Create an Amazon S3 bucket<a name="s3-crossservices-adddata-create-bucket"></a>

This topic is part of a larger tutorial about using the AWS SDK for JavaScript with AWS Lambda functions\. To start at the beginning of the tutorial, see [Creating and using Lambda functions](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/sdk-cross-service-example-submitting-data.html)\. 

In this task, you create an Amazon S3 bucket using the AWS SDK for JavaScript\. Alternatively, you can create it through the AWS Management Console\. For more information, see the [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html)\. 

Make sure to configure the SDK as previously shown, including installing the required clients and packages\. In `DynamoDBAppHelperFiles`, create a Node\.js module with the file name `create_bucket.ts`\. In `create_cognito_id_pool.ts`, load the `S3` client module\.

Create an object for the parameters for creating the bucket, replacing *REGION* with the AWS Region and *BUCKET\_NAME* with the name of the bucket\.

Create an `S3` client service object\.

To create the bucket, enter the following at the command prompt\.

```
ts-node create_bucket.ts
```

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

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/submit-data-app/src/dynamoAppHelperFiles/create-bucket.ts)\.