--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Managing IAM users<a name="iam-examples-managing-users"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve a list of IAM users\.
+ How to create and delete users\.
+ How to update a user name\.

## The scenario<a name="iam-examples-managing-users-scenario"></a>

In this example, a series of Node\.js modules are used to create and manage users in IAM\. The Node\.js modules use the SDK for JavaScript to create, delete, and update users using these methods of the `IAM` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/createusercommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/createusercommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/listuserscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/listuserscommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/updateusercommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/updateusercommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/getusercommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/getusercommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/deleteusercommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/deleteusercommand.html)

For more information about IAM users, see [IAM users](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users.html) in the *IAM User Guide*\.

## Prerequisite tasks<a name="iam-examples-managing-users-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Creating a user<a name="iam-examples-managing-users-creating-users"></a>

Create a `libs` directory, and create a Node\.js module with the file name `iamClient.js`\. Copy and paste the code below into it, which creates the IAM client object\. Replace *REGION* with your AWS Region\.

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/libs/iamClient.js)\.

Create a Node\.js module with the file name `iam_createuser.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. Create a JSON object containing the parameters needed, which consists of the user name you want to use for the new user as a command\-line parameter\.

Call the `GetUserCommand` method of the `IAM` client service object to see if the user name already exists\. If the user name does not currently exist, call the `CreateUserCommand` method to create it\. If the name already exists, write a message to that effect to the console\.

**Note**  
Replace *USER\_NAME* with the user name to create\.

```
// Import required AWS SDK clients and commands for Node.js
import { iamClient } from "./libs/iamClient.js";
import { GetUserCommand, CreateUserCommand } from "@aws-sdk/client-iam";

// Set the parameters
const params = { UserName: "USER_NAME" }; //USER_NAME

const run = async () => {
  try {
    const data = await iamClient.send(new GetUserCommand(params));
    console.log(
      "User " + process.argv[3] + " already exists",
      data.User.UserId
    );
    return data;
  } catch (err) {
    try {
      const results = await iamClient.send(new CreateUserCommand(params));
      console.log("Success", results);
      return results;
    } catch (err) {
      console.log("Error", err);
    }
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node iam_createuser.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_createuser.js)\.

## Listing users in Your Account<a name="iam-examples-managing-users-listing-users"></a>

Create a `libs` directory, and create a Node\.js module with the file name `iamClient.js`\. Copy and paste the code below into it, which creates the IAM client object\. Replace *REGION* with your AWS Region\.

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/libs/iamClient.js)\.

Create a Node\.js module with the file name `iam_listusers.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. Create a JSON object containing the parameters needed to list your users, limiting the number returned by setting the `MaxItems` parameter to 10\. Call the `ListUsersCommand` method of the `IAM` client service object\. Write the first user's name and creation date to the console\.

```
// Import required AWS SDK clients and commands for Node.js
import { iamClient } from "./libs/iamClient.js";
import { ListUsersCommand } from "@aws-sdk/client-iam";

// Set the parameters
const params = { MaxItems: 10 };

const run = async () => {
  try {
    const data = await iamClient.send(new ListUsersCommand(params));
    return data;
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
node iam_listusers.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_listusers.js)\.

## Updating a user's name<a name="iam-examples-managing-users-updating-users"></a>

Create a `libs` directory, and create a Node\.js module with the file name `iamClient.js`\. Copy and paste the code below into it, which creates the IAM client object\. Replace *REGION* with your AWS Region\.

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/libs/iamClient.js)\.

Create a Node\.js module with the file name `iam_updateuser.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. Create a JSON object containing the parameters needed to list your users, specifying both the current and new user names as command\-line parameters\. Call the `UpdateUserCommand` method of the `IAM` client service object\.

**Note**  
Replace *ORIGNAL\_USER\_NAME* with the user name to update, and *NEW\_USER\_NAME* with the new user name\.

```
// Import required AWS SDK clients and commands for Node.js
import { iamClient } from "./libs/iamClient.js";
import { UpdateUserCommand } from "@aws-sdk/client-iam";

// Set the parameters
const params = {
  UserName: "ORIGINAL_USER_NAME", //ORIGINAL_USER_NAME
  NewUserName: "NEW_USER_NAME", //NEW_USER_NAME
};

const run = async () => {
  try {
    const data = await iamClient.send(new UpdateUserCommand(params));
    console.log("Success, username updated");
    return data;
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

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_updateuser.js)\.

## Deleting a user<a name="iam-examples-managing-users-deleting-users"></a>

Create a `libs` directory, and create a Node\.js module with the file name `iamClient.js`\. Copy and paste the code below into it, which creates the IAM client object\. Replace *REGION* with your AWS Region\.

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/libs/iamClient.js)\.

Create a Node\.js module with the file name `iam_deleteuser.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. Create a JSON object containing the parameters needed, which consists of the user name to delete as a command\-line parameter\.

Call the `GetUserCommand` method of the `IAM` client service object to see if the user name already exists\. If the user name does not currently exist, write a message to that effect to the console\. If the user exists, call the `DeleteUserCommand` method to delete it\.

**Note**  
Replace *USER\_NAME* with the name of the user to delete\.

```
// Import required AWS SDK clients and commands for Node.js
import { iamClient } from "./libs/iamClient.js";
import { DeleteUserCommand, GetUserCommand } from "@aws-sdk/client-iam";

// Set the parameters
const params = { UserName: "USER_NAME" }; //USER_NAME

const run = async () => {
  try {
    const data = await iamClient.send(new GetUserCommand(params));
    return data;
    try {
      const results = await iamClient.send(new DeleteUserCommand(params));
      console.log("Success", results);
      return results;
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
node iam_deleteuser.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_deleteuser.js)\.