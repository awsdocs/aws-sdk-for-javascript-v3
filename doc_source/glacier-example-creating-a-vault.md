--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Creating a S3 Glacier vault<a name="glacier-example-creating-a-vault"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create a vault using the `CreateVaultCommand` method of the Amazon S3 Glacier service object\.

## Prerequisite tasks<a name="glacier-example-createvault-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glacier/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Create the vault<a name="glacier-example-createvault-code"></a>

Create a `libs` directory, and create a Node\.js module with the file name `glacierClient.js`\. Copy and paste the code below into it, which creates the S3 Glacier client object\. Replace *REGION* with your AWS Region\.

```
import  { GlacierClient }  from  "@aws-sdk/client-glacier";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create Glacier service object.
const glacierClient = new GlacierClient({ region: REGION });
export { glacierClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/glacier/src/libs/glacierClient.js)\.

Create a Node\.js module with the file name `createVault.ts`\. Copy and paste the code below into it\.

**Note**  
Replace *VAULT\_NAME* with the name of the S3 Glacier vault\.

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

To run the example, enter the following at the command prompt\.

```
node createVault.ts 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/glacier/src/createVault.ts)\.