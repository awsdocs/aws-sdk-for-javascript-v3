--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# S3 Glacier examples using SDK for JavaScript \(v3\)<a name="javascript_glacier_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for JavaScript \(v3\) with S3 Glacier\.

*Actions* are code excerpts that show you how to call individual service functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple functions within the same service\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#actions)

## Actions<a name="actions"></a>

### Create a vault<a name="glacier_CreateVault_javascript_topic"></a>

The following code example shows how to create an Amazon S3 Glacier vault\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glacier#code-examples)\. 
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
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/glacier-example-creating-a-vault.html)\. 
+  For API details, see [CreateVault](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glacier/classes/createvaultcommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript \(v2\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/glacier#code-examples)\. 
  

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
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/glacier-example-creating-a-vault.html)\. 
+  For API details, see [CreateVault](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/glacier-2012-06-01/CreateVault) in *AWS SDK for JavaScript API Reference*\. 

### Upload an archive to a vault<a name="glacier_UploadArchive_javascript_topic"></a>

The following code example shows how to upload an archive to an Amazon S3 Glacier vault\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glacier#code-examples)\. 
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
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/glacier-example-uploadarchive.html)\. 
+  For API details, see [UploadArchive](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glacier/classes/uploadarchivecommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript \(v2\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/glacier#code-examples)\. 
  

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
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/glacier-example-uploadrchive.html)\. 
+  For API details, see [UploadArchive](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/glacier-2012-06-01/UploadArchive) in *AWS SDK for JavaScript API Reference*\. 