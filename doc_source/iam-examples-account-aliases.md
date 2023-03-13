--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Managing IAM account aliases<a name="iam-examples-account-aliases"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to manage aliases for your AWS account ID\.

## The scenario<a name="iam-examples-account-aliases-scenario"></a>

If you want the URL for your sign\-in page to contain your company name or other friendly identifier instead of your AWS account ID, you can create an alias for your AWS account ID\. If you create an AWS account alias, your sign\-in page URL changes to incorporate the alias\.

In this example, a series of Node\.js modules are used to create and manage IAM account aliases\. The Node\.js modules use the SDK for JavaScript to manage aliases using these methods of the `IAM` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/createaccountaliascommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/createaccountaliascommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/listaccountaliasescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/listaccountaliasescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/deleteaccountaliascommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/deleteaccountaliascommand.html)

For more information about IAM account aliases, see [Your AWS account ID and its alias](https://docs.aws.amazon.com/IAM/latest/UserGuide/console_account-alias.html) in the *IAM User Guide*\.

## Prerequisite tasks<a name="iam-examples-account-aliases-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Creating an account alias<a name="iam-examples-account-aliases-creating"></a>

Create a `libs` directory, and create a Node\.js module with the file name `iamClient.js`\. Copy and paste the code below into it, which creates the IAM client object\. Replace *REGION* with your AWS Region\.

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/libs/iamClient.js)\.

Create a Node\.js module with the file name `iam_createaccountalias.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. Create a JSON object containing the parameters needed to create an account alias, which includes the alias to create\. Call the `CreateAccountAliasCommand` method of the `IAM` client service object\.

**Note**  
Replace *ACCOUNT\_ALIAS* with the alias to create\.

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { CreateAccountAliasCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = { AccountAlias: "ACCOUNT_ALIAS" }; //ACCOUNT_ALIAS

export const run = async () => {
  try {
    const data = await iamClient.send(new CreateAccountAliasCommand(params));
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node iam_createaccountalias.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_createaccountalias.js)\.

## Listing account aliases<a name="iam-examples-account-aliases-listing"></a>

Create a `libs` directory, and create a Node\.js module with the file name `iamClient.js`\. Copy and paste the code below into it, which creates the IAM client object\. Replace *REGION* with your AWS Region\.

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/libs/iamClient.js)\.

Create a Node\.js module with the file name `iam_listaccountaliases.js`\. Be sure to configure the SDK as client previously shown, including downloading the required clients and packages\. Create a JSON object containing the parameters needed to list account aliases, which includes the maximum number of items to return\. Call the `ListAccountAliasesCommand` method of the `IAM` client service object\.

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { ListAccountAliasesCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = { MaxItems: 5 };

export const run = async () => {
  try {
    const data = await iamClient.send(new ListAccountAliasesCommand(params));
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node iam_listaccountaliases.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_listaccountaliases.js)\.

## Deleting an account alias<a name="iam-examples-account-aliases-deleting"></a>

Create a `libs` directory, and create a Node\.js module with the file name `iamClient.js`\. Copy and paste the code below into it, which creates the IAM client object\. Replace *REGION* with your AWS Region\.

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/libs/iamClient.js)\.

Create a Node\.js module with the file name `iam_deleteaccountalias.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. Create a JSON object containing the parameters needed to delete an account alias, which includes the alias you want deleted\. Call the `DeleteAccountAliasCommand` method of the `IAM` service object\.

**Note**  
Replace *ALIAS* with the name of the alias you want to delete\.

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { DeleteAccountAliasCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = { AccountAlias: "ALIAS" }; // ALIAS

export const run = async () => {
  try {
    const data = await iamClient.send(new DeleteAccountAliasCommand(params));
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node iam_deleteaccountalias.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_deleteaccountalias.js)\.