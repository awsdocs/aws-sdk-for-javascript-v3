--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Managing IAM account aliases<a name="iam-examples-account-aliases"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to manage aliases for your AWS account ID\.

## The scenario<a name="iam-examples-account-aliases-scenario"></a>

If you want the URL for your sign\-in page to contain your company name or other friendly identifier instead of your AWS account ID, you can create an alias for your AWS account ID\. If you create an AWS account alias, your sign\-in page URL changes to incorporate the alias\.

In this example, a series of Node\.js modules are used to create and manage IAM account aliases\. The Node\.js modules use the SDK for JavaScript to manage aliases using these methods of the `IAM` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#createAccountAlias-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#createAccountAlias-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#listAccountAliases-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#listAccountAliases-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#deleteAccountAlias-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#deleteAccountAlias-property)

For more information about IAM account aliases, see [Your AWS account ID and its alias](https://docs.aws.amazon.com/IAM/latest/UserGuide/console_account-alias.html) in the *IAM User Guide*\.

## Prerequisite tasks<a name="iam-examples-account-aliases-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript is a super\-set of JavaScript so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Creating an account alias<a name="iam-examples-account-aliases-creating"></a>

Create a Node\.js module with the file name `iam_createaccountalias.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access IAM, create an `IAM` client service object\. Create a JSON object containing the parameters needed to create an account alias, which includes the alias to create\. Call the `CreateAccountAliasCommand` method of the `IAM` client service object\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *ACCOUNT\_ALIAS* with the alias to create\.

```
// Import required AWS SDK clients and commands for Node.js
const { IAMClient, CreateAccountAliasCommand } = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const accountAlias = { AccountAlias: "ACCOUNT_ALIAS" }; //ACCOUNT_ALIAS

// Create IAM service object
const iam = new IAMClient(REGION);

const run = async () => {
  try {
    const data = await iam.send(new CreateAccountAliasCommand(accountAlias));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node iam_createaccountalias.ts // If you prefer JavaScript, enter 'node iam_createaccountalias.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_createaccountalias.ts)\.

## Listing account aliases<a name="iam-examples-account-aliases-listing"></a>

Create a Node\.js module with the file name `iam_listaccountaliases.ts`\. Be sure to configure the SDK as client previously shown, including downloading the required clients and packages\. To access IAM, create an `IAM` service object\. Create a JSON object containing the parameters needed to list account aliases, which includes the maximum number of items to return\. Call the `ListAccountAliasesCommand` method of the `IAM` client service object\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region\.

```
// Import required AWS SDK clients and commands for Node.js
const { IAMClient, ListAccountAliasesCommand } = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { MaxItems: 5 };

// Create IAM service object
const iam = new IAMClient(REGION);

const run = async () => {
  try {
    const data = await iam.send(new ListAccountAliasesCommand(params));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node iam_listaccountaliases.ts // If you prefer JavaScript, enter 'node iam_listaccountaliases.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_listaccountaliases.ts)\.

## Deleting an account alias<a name="iam-examples-account-aliases-deleting"></a>

Create a Node\.js module with the file name `iam_deleteaccountalias.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access IAM, create an `IAM` service object\. Create a JSON object containing the parameters needed to delete an account alias, which includes the alias you want deleted\. Call the `DeleteAccountAliasCommand` method of the `IAM` service object\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *ALIAS* with the name of the alias you want to delete\.

```
// Import required AWS SDK clients and commands for Node.js
const { IAMClient, DeleteAccountAliasCommand } = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { AccountAlias: "ALIAS" }; // ALIAS

// Create IAM service object
const iam = new IAMClient(REGION);

const run = async () => {
  try {
    const data = await iam.send(new DeleteAccountAliasCommand(params));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node iam_deleteaccountalias.ts // If you prefer JavaScript, enter 'node iam_deleteaccountalias.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_deleteaccountalias.ts)\.