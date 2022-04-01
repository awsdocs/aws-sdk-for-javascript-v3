--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# S3 Glacier examples using SDK for JavaScript V3<a name="javascript_glacier_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for JavaScript V3 with S3 Glacier\.

*Actions* are code excerpts that show you how to call individual S3 Glacier functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple S3 Glacier functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w362aac23b9c27c13)

## Actions<a name="w362aac23b9c27c13"></a>

### Create a vault<a name="glacier_CreateVault_javascript_topic"></a>

The following code example shows how to create an Amazon S3 Glacier vault\.

**SDK for JavaScript V3**  
Create the client\.  

```
const { GlacierClient } = require("@aws-sdk/client-glacier");
// Set the AWS Region.
const REGION = "REGION";
//Set the Redshift Service Object
const glacierClient = new GlacierClient({ region: REGION });
export { glacierClient };
```
Create the vault\.  

```
// Load the SDK for JavaScript
import { CreateVaultCommand } from "@aws-sdk/client-glacier";
import { glacierClient } from "./libs/glacierClient.js";

// Set the parameters
const vaultname = "VAULT_NAME"; // VAULT_NAME
const params = { vaultName: vaultname };

const run = async () => {
  try {
    const data = await glacierClient.send(new CreateVaultCommand(params));
    console.log("Success, vault created!");
    return data; // For unit tests.
  } catch (err) {
    console.log("Error");
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glacier#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/glacier-example-creating-a-vault.html)\. 
+  For API details, see [CreateVault](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glacier/classes/createvaultcommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the SDK for JavaScript
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create a new service object
var glacier = new AWS.Glacier({apiVersion: '2012-06-01'});
// Call Glacier to create the vault
glacier.createVault({vaultName: 'YOUR_VAULT_NAME'}, function(err) {
  if (!err) {
    console.log("Created vault!")
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/glacier#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/glacier-example-creating-a-vault.html)\. 
+  For API details, see [CreateVault](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/glacier-2012-06-01/CreateVault) in *AWS SDK for JavaScript API Reference*\. 

### Upload an archive to a vault<a name="glacier_UploadArchive_javascript_topic"></a>

The following code example shows how to upload an archive to an Amazon S3 Glacier vault\.

**SDK for JavaScript V3**  
Create the client\.  

```
const { GlacierClient } = require("@aws-sdk/client-glacier");
// Set the AWS Region.
const REGION = "REGION";
//Set the Redshift Service Object
const glacierClient = new GlacierClient({ region: REGION });
export { glacierClient };
```
Upload the archive\.  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glacier#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/glacier-example-uploadarchive.html)\. 
+  For API details, see [UploadArchive](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glacier/classes/uploadarchivecommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the SDK for JavaScript
var AWS = require('aws-sdk');
// Set the region
AWS.config.update({region: 'REGION'});

// Create a new service object and buffer
var glacier = new AWS.Glacier({apiVersion: '2012-06-01'});
buffer = Buffer.alloc(2.5 * 1024 * 1024); // 2.5MB buffer

var params = {vaultName: 'YOUR_VAULT_NAME', body: buffer};
// Call Glacier to upload the archive.
glacier.uploadArchive(params, function(err, data) {
  if (err) {
    console.log("Error uploading archive!", err);
  } else {
    console.log("Archive ID", data.archiveId);
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/glacier#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/glacier-example-uploadrchive.html)\. 
+  For API details, see [UploadArchive](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/glacier-2012-06-01/UploadArchive) in *AWS SDK for JavaScript API Reference*\. 