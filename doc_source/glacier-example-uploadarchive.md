--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Uploading an archive to S3 Glacier<a name="glacier-example-uploadarchive"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to upload an archive to Amazon S3 Glacier using the `uploadArchive` method of the S3 Glacier service object\.

The following example uploads a single `Buffer` object as an entire archive using the `UploadArchiveCommand` method of the S3 Glacier service object\.

The example assumes you've already created a vault named `VAULT_NAME`\. The SDK automatically computes the tree hash `checksum` for the data uploaded, however, you can override it by passing your own checksum parameter\.

## Prerequisite tasks<a name="glacier-example-uploadarchive-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/glacier/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript is a super\-set of JavaScript so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Upload the archive<a name="glacier-example-uploadarchive-code"></a>

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *VAULT\_NAME* with the name of the S3 Glacier vault\.

```
// Load the SDK for JavaScript
const { GlacierClient, UploadArchiveCommand } = require("@aws-sdk/client-glacier");

// Set the AWS Region
const REGION = "REGION"; // e.g. 'us-east-1'

// Set the parameters
const vaultname = "VAULT_NAME"; // VAULT_NAME

// Create a new service object and buffer
const buffer = new Buffer.alloc(2.5 * 1024 * 1024); // 2.5MB buffer
const params = { vaultName: vaultname, body: buffer };

// Instantiate an S3 Glacier client
const glacier = new GlacierClient(REGION);

const run = async () => {
  try {
    const data = await glacier.send(new UploadArchiveCommand(params));
    console.log("Archive ID", data.archiveId);
  } catch (err) {
    console.log("Error uploading archive!", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node uploadArchive.ts // If you prefer JavaScript, enter 'node uploadArchive.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/glacier/src/uploadArchive.ts)\.