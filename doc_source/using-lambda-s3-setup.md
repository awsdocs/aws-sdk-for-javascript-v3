--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Create an Amazon S3 bucket configured as a static website<a name="using-lambda-s3-setup"></a>

In this task, you create and prepare the Amazon S3 bucket used by the application\.

![\[JavaScript running in a browser that creates an Amazon S3 bucket\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/create-s3-bucket.png)![\[JavaScript running in a browser that creates an Amazon S3 bucket\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[JavaScript running in a browser that creates an Amazon S3 bucket\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

For this application, the first thing you need to create is an Amazon S3 bucket to store all the browser assets\. These include the HTML file, all graphics files, and the CSS file\. The bucket is configured as a static website so that it also serves the application from the bucket's URL\. 

The `slotassets` directory contains the Node\.js script `s3-bucket-setup.ts` that creates the Amazon S3 bucket and sets the website configuration\. 

**To create and configure the Amazon S3 bucket that the tutorial application uses**
+ In the Node\.js script `s3-bucket-setup.ts` replace *BUCKET\_NAME* with a globally unique bucket name\.

  At the command line, enter the following command\.

  `node s3-bucket-setup.ts `

  If the command succeeds, the script displays the URL of the new bucket\. Make a note of this URL because you'll use it later\.

## Setup script<a name="using-lambda-s3-script"></a>

The setup script runs the following code\. It takes the command\-line argument that is passed in and uses it to specify the bucket name and the parameter that makes the bucket publicly readable\. It then sets up the parameters used to enable the bucket to act as a static website host\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import an S3 client
const {
  S3Client,
  CreateBucketCommand,
  PutBucketWebsiteCommand,
  PutBucketPolicyCommand
} = require("@aws-sdk/client-s3");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Create params JSON for S3.createBucket
const bucketName = "BUCKET_NAME"; //BUCKET_NAME
const bucketParams = {
  Bucket: bucketName
};

// Create params JSON for S3.setBucketWebsite
const staticHostParams = {
  Bucket: bucketName,
  WebsiteConfiguration: {
    ErrorDocument: {
      Key: "error.html",
    },
    IndexDocument: {
      Suffix: "index.html",
    },
  },
};

var readOnlyAnonUserPolicy = {
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
const bucketResource = "arn:aws:s3:::" + bucketName + "/*"; //BUCKET_NAME
readOnlyAnonUserPolicy.Statement[0].Resource[0] = bucketResource;

// convert policy JSON into string and assign into params
const bucketPolicyParams = {
  Bucket: bucketName,
  Policy: JSON.stringify(readOnlyAnonUserPolicy)
};

// Instantiate an S3 client
const s3 = new S3Client({ region: REGION });

const run = async () => {
  try {
    // Call S3 to create the bucket
    const response = await s3.send(new CreateBucketCommand(bucketParams));
    console.log("Bucket URL is ", response.Location);
  } catch (err) {
    console.log("Error", err);
  }
  try {
    // Set the new policy on the newly created bucket
    const response = await s3.send(
      new PutBucketWebsiteCommand(staticHostParams)
    );
    // Update the displayed policy for the selected bucket
    console.log("Success", response);
  } catch (err) {
    // Display error message
    console.log("Error", err);
  }
};

run();
```

Choose **next** to continue the tutorial\.

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/lambda/src/s3-bucket-setup.ts)\.