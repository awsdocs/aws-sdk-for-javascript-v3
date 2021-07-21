--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Uploading an archive to S3 Glacier<a name="glacier-example-uploadarchive"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to upload an archive to Amazon S3 Glacier using the `uploadArchive` method of the S3 Glacier service object\.

The following example uploads a single `Buffer` object as an entire archive using the `UploadArchiveCommand` method of the S3 Glacier service object\.

The example assumes you've already created a vault named `VAULT_NAME`\. The SDK automatically computes the tree hash `checksum` for the data uploaded, however, you can override it by passing your own checksum parameter\.

## Prerequisite tasks<a name="glacier-example-uploadarchive-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/glacier/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Upload the archive<a name="glacier-example-uploadarchive-code"></a>

Create a `libs` directory, and create a Node\.js module with the file name `glacierClient.js`\. Copy and paste the code below into it, which creates the S3 Glacier client object\. Replace *REGION* with your AWS Region\.

```
import  { GlacierClient }  from  "@aws-sdk/client-glacier";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create Glacier service object.
const glacierClient = new GlacierClient({ region: REGION });
export { glacierClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/libs/glacierClient.js)\.

Create a Node\.js module with the file name `uploadArchive.js`\. Copy and paste the code below into it\.

**Note**  
Replace *VAULT\_NAME* with the name of the S3 Glacier vault\.

```
// Load the SDK for JavaScript
import { UploadArchiveCommand } from "@aws-sdk/client-glacier";
import { glacierClient } from "./libs/glacierClient.js";

// Set the parameters
const vaultname = "VAULT_NAME"; // VAULT_NAME

// Create a new service object and buffer
const buffer = new Buffer.alloc(2.5 * 1024 * 1024); // 2.5MB buffer
const params = { vaultName: vaultname, body: buffer };

const run = async () => {
  try {
    const data = await glacierClient.send(new UploadArchiveCommand(params));
    console.log("Archive ID", data.archiveId);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error uploading archive!", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node uploadArchive.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/glacier/src/uploadArchive.js)\.