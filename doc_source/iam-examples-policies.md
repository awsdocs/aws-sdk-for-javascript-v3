--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Working with IAM policies<a name="iam-examples-policies"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create and delete IAM policies\.
+ How to attach and detach IAM policies from roles\.

## The scenario<a name="iam-examples-policies-scenario"></a>

You grant permissions to a user by creating a *policy*, which is a document that lists the actions that a user can perform and the resources those actions can affect\. Any actions or resources that are not explicitly allowed are denied by default\. Policies can be created and attached to users, groups of users, roles assumed by users, and resources\.

In this example, a series of Node\.js modules are used to manage policies in IAM\. The Node\.js modules use the SDK for JavaScript to create and delete policies as well as attaching and detaching role policies using these methods of the `IAM` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#createPolicy-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#createPolicy-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#GetPolicyCommand-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#GetPolicyCommand-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#ListAttachedRolePoliciesCommand-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#ListAttachedRolePoliciesCommand-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#AttachRolePolicyCommand-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#AttachRolePolicyCommand-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#DetachRolePolicyCommand-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#DetachRolePolicyCommand-property)

For more information about IAM users, see [Overview of access management: Permissions and policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_access-management.html) in the *IAM User Guide*\.

## Prerequisite tasks<a name="iam-examples-policies-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript is a super\-set of JavaScript so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create an IAM role to which you can attach policies\. For more information about creating roles, see [Creating IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create.html) in the *IAM User Guide*\.

## Creating an IAM policy<a name="iam-examples-policies-creating"></a>

Create a Node\.js module with the file name `iam_createpolicy.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access IAM, create an `IAM` client service object\. Create two JSON objects, one containing the policy document to create and the other containing the parameters needed to create the policy, which includes the policy JSON and the name to give the policy\. Be sure to stringify the policy JSON object in the parameters\. Call the `CreatePolicyCommand` method of the `IAM` client service object\.

**Note**  
Replace *REGION* with your AWS Region, *RESOURCE\_ARN* with the Amazon Resource Name \(ARN\) of the resource you want to grant the permissions to, and *DYNAMODB\_POLICY\_NAME* with the name of the DynamoDB policy name\.

```
// Import required AWS SDK clients and commands for Node.js
const { IAMClient, CreatePolicyCommand } = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Create IAM service object
const iam = new IAMClient(REGION);

// Set the parameters
const myManagedPolicy = {
  Version: "2012-10-17",
  Statement: [
    {
      Effect: "Allow",
      Action: "logs:CreateLogGroup",
      Resource: "RESOURCE_ARN", // RESOURCE_ARN
    },
    {
      Effect: "Allow",
      Action: [
        "dynamodb:DeleteItem",
        "dynamodb:GetItem",
        "dynamodb:PutItem",
        "dynamodb:Scan",
        "dynamodb:UpdateItem",
      ],
      Resource: "DYNAMODB_POLICY_NAME", // DYNAMODB_POLICY_NAME; e.g., "myDynamoDBName"
    },
  ],
};
const params = {
  PolicyDocument: JSON.stringify(myManagedPolicy),
  PolicyName: process.argv[4],
};

const run = async () => {
  try {
    const data = await iam.send(new CreatePolicyCommand(params));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node iam_createpolicy.ts // If you prefer JavaScript, enter 'node iam_createpolicy.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_createpolicy.ts)\.

## Getting an IAM policy<a name="iam-examples-policies-getting"></a>

Create a Node\.js module with the file name `iam_getpolicy.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access IAM, create an `IAM` client service object\. Create a JSON object containing the parameters needed retrieve a policy, which is the ARN of the policy to get\. Call the `GetPolicyCommand` method of the `IAM` client service object\. Write the policy description to the console\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region\.

```
// Import required AWS SDK clients and commands for Node.js
const { IAMClient, GetPolicyCommand } = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  PolicyArn: "arn:aws:iam::aws:policy/AWSLambdaExecute",
};

// Create IAM service object
const iam = new IAMClient(REGION);

const run = async () => {
  try {
    const data = await iam.send(new GetPolicyCommand(params));
    console.log("Success", data.Policy.Description);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node iam_getpolicy.ts // If you prefer JavaScript, enter 'node iam_getpolicy.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_getpolicy.ts)\.

## Attaching a managed role policy<a name="iam-examples-policies-attaching-role-policy"></a>

Create a Node\.js module with the file name `iam_attachrolepolicy.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access IAM, create an `IAM` client service object\. Create a JSON object containing the parameters needed to get a list of managed IAM policies attached to a role, which consists of the name of the role\. Provide the role name as a command\-line parameter\. Call the `ListAttachedRolePoliciesCommand` method of the `IAM` client service object, which returns an array of managed policies to the callback function\.

Check the array members to see if the policy to attach to the role is already attached\. If the policy is not attached, call the `AttachRolePolicyCommand` method to attach it\. 

**Note**  
Replace *REGION* with your AWS Region, and *ROLE\_NAME* with the name of the role to attach\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  IAMClient,
  ListAttachedRolePoliciesCommand,
  AttachRolePolicyCommand
} = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const ROLENAME = "ROLE_NAME";
const paramsRoleList = { RoleName: ROLENAME }; //ROLE_NAME

// Create IAM service object
const iam = new IAMClient(REGION);

const run = async () => {
  const iam = new IAMClient(REGION);
  try {
    const data = await iam.send(
      new ListAttachedRolePoliciesCommand(paramsRoleList)
    );
    const myRolePolicies = data.AttachedPolicies;
    myRolePolicies.forEach(function (val, index, array) {
      if (myRolePolicies[index].PolicyName === "AmazonDynamoDBFullAccess") {
        console.log(
          "AmazonDynamoDBFullAccess is already attached to this role."
        );
        process.exit();
      }
    });
    try {
      const params = {
        PolicyArn: "arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess",
        RoleName: ROLENAME,
      };
      const data = await iam.send(new AttachRolePolicyCommand(params));
      console.log("Role attached successfully");
    } catch (err) {
      console.log("Error", err);
    }
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node iam_attachrolepolicy.ts // If you prefer JavaScript, enter 'node iam_attachrolepolicy.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_attachrolepolicy.ts)\.

## Detaching a managed role policy<a name="iam-examples-policies-detaching-role-policy"></a>

Create a Node\.js module with the file name `iam_detachrolepolicy.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access IAM, create an `IAM` client service object\. Create a JSON object containing the parameters needed to get a list of managed IAM policies attached to a role, which consists of the name of the role\. Provide the role name as a command\-line parameter\. Call the `ListAttachedRolePoliciesCommand` method of the `IAM` client service object, which returns an array of managed policies in the callback function\.

Check the array members to see if the policy to detach from the role is attached\. If the policy is attached, call the `DetachRolePolicyCommand` method to detach it\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *ROLE\_NAME* with the name of the role to detach\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  IAMClient,
  ListAttachedRolePoliciesCommand,
  DetachRolePolicyCommand
} = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const paramsRoleList = { RoleName: "ROLE_NAME" }; //ROLE_NAME

// Create IAM service object
const iam = new IAMClient(REGION);

const run = async () => {
  // Load the AWS SDK for Node.js

  // Create IAM service object
  try {
    const data = await iam.send(
      new ListAttachedRolePoliciesCommand(paramsRoleList)
    );
    const myRolePolicies = data.AttachedPolicies;
    myRolePolicies.forEach(function (val, index, array) {
      if (myRolePolicies[index].PolicyName === "AmazonDynamoDBFullAccess") {
        const params = {
          PolicyArn: "arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess",
          paramsRoleList,
        };
        try {
          const results = iam.send(new DetachRolePolicyCommand(paramsRoleList));
          console.log("Policy detached from role successfully");
          process.exit();
        } catch (err) {
          console.log("Unable to detach policy from role", err);
        }
      } else {
      }
    });
  } catch (err) {
    console.log("User " + process.argv[2] + " does not exist.");
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node iam_detachrolepolicy.ts // If you prefer JavaScript, enter 'node iam_detachrolepolicy.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_detachrolepolicy.ts)\.