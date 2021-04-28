--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Creating a S3 Glacier vault<a name="glacier-example-creating-a-vault"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create a vault using the `CreateVaultCommand` method of the Amazon S3 Glacier service object\.

## Prerequisite tasks<a name="glacier-example-createvault-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/glacier/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypeScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these examples can also be run in JavaScript\. For more information, see [this article](https://aws.amazon.com/blogs/developer/first-class-typescript-support-in-modular-aws-sdk-for-javascript/) in the AWS Developer Blog\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Create the vault<a name="glacier-example-createvault-code"></a>

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *VAULT\_NAME* with the name of the Glacier vault\.

```
// Load the SDK for JavaScript
const { GlacierClient, CreateVaultCommand } = require("@aws-sdk/client-glacier");

// Set the AWS Region
const REGION = "REGION"; // e.g. 'us-east-1'

// Set the parameters
const vaultname = "VAULT_NAME"; // VAULT_NAME
const params = { vaultName: vaultname };

// Instantiate an S3 Glacier client
const glacier = new GlacierClient({ region: REGION });

const run = async () => {
  try {
    const data = await glacier.send(new CreateVaultCommand(params));
    console.log("Success, vault created!");
  } catch (err) {
    console.log("Error");
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node createVault.ts // If you prefer JavaScript, enter 'node createVault.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/glacier/src/createVault.ts)\.