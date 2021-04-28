--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Managing IAM access keys<a name="iam-examples-managing-access-keys"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to manage the access keys of your users\.

## The scenario<a name="iam-examples-managing-access-keys-scenario"></a>

Users need their own access keys to make programmatic calls to AWS from the SDK for JavaScript\. To fill this need, you can create, modify, view, or rotate access keys \(access key IDs and secret access keys\) for IAM users\. By default, when you create an access key, its status is `Active`, which means the user can use the access key for API calls\. 

In this example, a series of Node\.js modules are used manage access keys in IAM\. The Node\.js modules use the SDK for JavaScript to manage IAM access keys using these methods of the `IAM` client class:
+ [CreateAccessKeyCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/createaccesskeycommand.html)
+ [ListAccessKeysCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/listaccesskeyscommand.html)
+ [GetAccessKeyLastUsedCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/getaccesskeylastusedcommand.html)
+ [UpdateAccessKeyCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/updateaccesskeycommand.html)
+ [DeleteAccessKeyCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/deleteaccesskeycommand.html)

For more information about IAM access keys, see [Access keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the *IAM User Guide*\.

## Prerequisite tasks<a name="iam-examples-managing-access-keys-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypeScript, so for consistency these examples are presented in TypeScript\. TypeScript is a super\-set of JavaScript so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Creating access keys for a user<a name="iam-examples-managing-access-keys-creating"></a>

Create a Node\.js module with the file name `iam_createaccesskeys.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access IAM, create an `IAM` client service object\. Create a JSON object containing the parameters needed to create new access keys, which includes IAM user's name\. Call the `CreateAccessKeyCommand` method of the `IAM` client service object\.

**Note**  
Replace *REGION* with your AWS Region, and *IAM\_USER\_NAME* with the IAM user name\.

```
// Import required AWS SDK clients and commands for Node.js
const { IAMClient, CreateAccessKeyCommand } = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const userName = "IAM_USER_NAME"; //IAM_USER_NAME

// Create IAM service object
const iam = new IAMClient({ region: REGION });

const run = async () => {
  try {
    const data = await iam.send(new CreateAccessKeyCommand(userName));
    console.log("Success", data.AccessKey);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\. Be sure to pipe the returned data to a text file in order not to lose the secret key, which can only be provided once\.

```
ts-node iam_createaccesskeys.ts > newuserkeysV3.txt // If you prefer JavaScript, enter 'node iam_createaccesskeys.js > newuserkeysV3.txt'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_createaccesskeys.ts)\.

## Listing a user's access keys<a name="iam-examples-managing-access-keys-listing"></a>

Create a Node\.js module with the file name `iam_listaccesskeys.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access IAM, create an `IAM` client service object\. Create a JSON object containing the parameters needed to retrieve the user's access keys, which includes IAM user's name and optionally the maximum number of access key pairs listed\. Call the `ListAccessKeysCommand` method of the `IAM` client service object\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *IAM\_USER\_NAME* with the IAM user name\.

```
// Import required AWS SDK clients and commands for Node.js
const { IAMClient, ListAccessKeysCommand } = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  MaxItems: 5,
  UserName: "IAM_USER_NAME", //IAM_USER_NAME
};

// Create IAM service object
const iam = new IAMClient({ region: REGION });

const run = async () => {
  try {
    const data = await iam.send(new ListAccessKeysCommand(params));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node iam_listaccesskeys.ts // If you prefer JavaScript, enter 'node iam_listaccesskeys.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_listaccesskeys.ts)\.

## Getting the last use for access keys<a name="iam-examples-managing-access-keys-last-used"></a>

Create a Node\.js module with the file name `iam_accesskeylastused.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access IAM, create an `IAM` service object\. Create a JSON object containing the parameters needed to create new access keys, which is the access key ID for which the last use information\. Call the `GetAccessKeyLastUsedCommand` method of the `IAM` service object\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *ACCESS\_KEY\_ID* with the access key ID for which the the last use information\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  IAMClient,
  GetAccessKeyLastUsedCommand
} = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { AccessKeyId: "ACCESS_KEY_ID" }; //ACCESS_KEY_ID

// Create IAM service object
const iam = new IAMClient({ region: REGION });

const run = async () => {
  try {
    const data = await iam.send(new GetAccessKeyLastUsedCommand(params));
    console.log("Success", data.AccessKeyLastUsed);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node iam_accesskeylastused.ts // If you prefer JavaScript, enter 'node iam_accesskeylastused.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_accesskeylastused.ts)\.

## Updating access key status<a name="iam-examples-managing-access-keys-updating"></a>

Create a Node\.js module with the file name `iam_updateaccesskey.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access IAM, create an `IAM` client service object\. Create a JSON object containing the parameters needed to update the status of an access keys, which includes the access key ID and the updated status\. The status can be `Active` or `Inactive`\. Call the `updateAccessKey` method of the `IAM` client service object\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *ACCESS\_KEY\_ID* the access key ID and the updated status, and *USER\_NAME* with the name of the user\.

```
// Import required AWS SDK clients and commands for Node.js
const { IAMClient, UpdateAccessKeyCommand } = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  AccessKeyId: "ACCESS_KEY_ID", //ACCESS_KEY_ID
  Status: "Active",
  UserName: "USER_NAME", //USER_NAME
};

// Create IAM service object
const iam = new IAMClient({ region: REGION });

const run = async () => {
  try {
    const data = await iam.send(new UpdateAccessKeyCommand(params));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node iam_updateaccesskey.ts // If you prefer JavaScript, enter 'node iam_updateaccesskey.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_updateaccesskey.ts)\.

## Deleting access keys<a name="iam-examples-managing-access-keys-deleting"></a>

Create a Node\.js module with the file name `iam_deleteaccesskey.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access IAM, create an `IAM` client service object\. Create a JSON object containing the parameters needed to delete access keys, which includes the access key ID and the name of the user\. Call the `DeleteAccessKeyCommand` method of the `IAM` client service object\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *ACCESS\_KEY\_ID* with your access key ID, and *USER\_NAME* with the user name\.

```
// Import required AWS SDK clients and commands for Node.js
const { IAMClient, DeleteAccessKeyCommand } = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  AccessKeyId: "ACCESS_KEY_ID", // ACCESS_KEY_ID
  UserName: "USER_NAME", // USER_NAME
};

// Create IAM service object
const iam = new IAMClient({ region: REGION });

const run = async () => {
  try {
    const data = await iam.send(new DeleteAccessKeyCommand(params));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node iam_deleteaccesskey.ts // If you prefer JavaScript, enter 'node iam_deleteaccesskey.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_deleteaccesskey.ts)\.