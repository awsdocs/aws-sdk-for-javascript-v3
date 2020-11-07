--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Managing IAM users<a name="iam-examples-managing-users"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve a list of IAM users\.
+ How to create and delete users\.
+ How to update a user name\.

## The scenario<a name="iam-examples-managing-users-scenario"></a>

In this example, a series of Node\.js modules are used to create and manage users in IAM\. The Node\.js modules use the SDK for JavaScript to create, delete, and update users using these methods of the `IAM` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#createUser-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#createUser-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#listUsers-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#listUsers-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#updateUser-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#updateUser-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#getUser-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#getUser-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#deleteUser-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#deleteUser-property)

For more information about IAM users, see [IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html) in the *IAM User Guide*\.

## Prerequisite tasks<a name="iam-examples-managing-users-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Creating a user<a name="iam-examples-managing-users-creating-users"></a>

Create a Node\.js module with the file name `iam_createuser.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access IAM, create an `IAM` service object\. Create a JSON object containing the parameters needed, which consists of the user name you want to use for the new user as a command\-line parameter\.

Call the `getUser` method of the `IAM` client service object to see if the user name already exists\. If the user name does not currently exist, call the `CreateUserCommand` method to create it\. If the name already exists, write a message to that effect to the console\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *USER\_NAME* with the user name to create\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  IAMClient,
  GetUserCommand,
  CreateUserCommand
} = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { UserName: "USER_NAME" }; //USER_NAME

// Create IAM service object
const iam = new IAMClient(REGION);

const run = async () => {
  try {
    const data = await iam.send(new GetUserCommand(params));
    console.log(
      "User " + process.argv[3] + " already exists",
      data.User.UserId
    );
  } catch (err) {
    try {
      const results = await iam.send(new CreateUserCommand(params));
      console.log("Success", results);
    } catch (err) {
      console.log("Error", err);
    }
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node iam_createuser.ts // If you prefer JavaScript, enter 'node iam_createuser.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/iam_createuser.ts)\.

## Listing users in Your Account<a name="iam-examples-managing-users-listing-users"></a>

Create a Node\.js module with the file name `iam_listusers.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access IAM, create an `IAM` client service object\. Create a JSON object containing the parameters needed to list your users, limiting the number returned by setting the `MaxItems` parameter to 10\. Call the `ListUsersCommand` method of the `IAM` client service object\. Write the first user's name and creation date to the console\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region\.

```
// Import required AWS SDK clients and commands for Node.js
const { IAMClient, ListUsersCommand } = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { MaxItems: 10 };

// Create IAM service object
const iam = new IAMClient(REGION);

const run = async () => {
  try {
    const data = await iam.send(new ListUsersCommand(params));
    const users = data.Users || [];
    users.forEach(function (user) {
      console.log("User " + user.UserName + " created", user.CreateDate);
    });
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node iam_listusers.ts // If you prefer JavaScript, enter 'node iam_listusers.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/iam_listusers.ts)\.

## Updating a user's name<a name="iam-examples-managing-users-updating-users"></a>

Create a Node\.js module with the file name `iam_updateuser.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access IAM, create an `IAM` client service object\. Create a JSON object containing the parameters needed to list your users, specifying both the current and new user names as command\-line parameters\. Call the `UpdateUserCommand` method of the `IAM` client service object\.

**Note**  
Replace *REGION* with your AWS Region, *ORIGNAL\_USER\_NAME* with the user name to update, and *NEW\_USER\_NAME* with the new user name\.

```
// Import required AWS SDK clients and commands for Node.js
const { IAMClient, UpdateUserCommand } = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  UserName: "ORIGINAL_USER_NAME", //ORIGINAL_USER_NAME
  NewUserName: "NEW_USER_NAME", //NEW_USER_NAME
};

// Create IAM service object
const iam = new IAMClient(REGION);

const run = async () => {
  try {
    const data = await iam.send(new UpdateUserCommand(params));
    console.log("Success, username updated");
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt, specifying the user's current name followed by the new user name\.

```
node iam_updateuser.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/iam_updateuser.ts)\.

## Deleting a user<a name="iam-examples-managing-users-deleting-users"></a>

Create a Node\.js module with the file name `iam_deleteuser.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access IAM, create an `IAM` client service object\. Create a JSON object containing the parameters needed, which consists of the user name to delete as a command\-line parameter\.

Call the `GetUserCommand` method of the `IAM` client service object to see if the user name already exists\. If the user name does not currently exist, write a message to that effect to the console\. If the user exists, call the `DeleteUserCommand` method to delete it\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *USER\_NAME* with the name of the user to delete\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  IAMClient,
  DeleteUserCommand,
  GetUserCommand,
} = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { UserName: "USER_NAME" }; //USER_NAME

// Create IAM service object
const iam = new IAMClient(REGION);

const run = async () => {
  try {
    const data = await iam.send(new GetUserCommand(params));
    try {
      const results = await iam.send(new DeleteUserCommand(params));
      console.log("Success", results);
    } catch (err) {
      console.log("Error", err);
    }
  } catch (err) {
    console.log("User " + process.argv[2] + " does not exist.");
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node iam_deleteuser.ts // If you prefer JavaScript, enter 'node iam_deleteuser.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/iam_deleteuser.ts)\.