# Working with IAM Policies<a name="iam-examples-policies"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create and delete IAM policies\.
+ How to attach and detach IAM policies from roles\.

## The Scenario<a name="iam-examples-policies-scenario"></a>

You grant permissions to a user by creating a *policy*, which is a document that lists the actions that a user can perform and the resources those actions can affect\. Any actions or resources that are not explicitly allowed are denied by default\. Policies can be created and attached to users, groups of users, roles assumed by users, and resources\.

In this example, a series of Node\.js modules are used to manage policies in IAM\. The Node\.js modules use the SDK for JavaScript to create and delete policies as well as attaching and detaching role policies using these methods of the `AWS.IAM` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#createPolicy-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#createPolicy-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#getPolicy-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#getPolicy-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#listAttachedRolePolicies-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#listAttachedRolePolicies-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#attachRolePolicy-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#attachRolePolicy-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#detachRolePolicy-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#detachRolePolicy-property)

For more information about IAM users, see [Overview of Access Management: Permissions and Policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction_access-management.html) in the *IAM User Guide*\.

## Prerequisite Tasks<a name="iam-examples-policies-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.
+ Create an IAM role to which you can attach policies\. For more information about creating roles, see [Creating IAM Roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create.html) in the *IAM User Guide*\.

## Creating an IAM Policy<a name="iam-examples-policies-creating"></a>

Create a Node\.js module with the file name `iam_createpolicy.js`\. Be sure to configure the SDK as previously shown\. To access IAM, create an `AWS.IAM` service object\. Create two JSON objects, one containing the policy document you want to create and the other containing the parameters needed to create the policy, which includes the policy JSON and the name you want to give the policy\. Be sure to stringify the policy JSON object in the parameters\. Call the `createPolicy` method of the `AWS.IAM` service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var myManagedPolicy = {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "logs:CreateLogGroup",
            "Resource": "RESOURCE_ARN"
        },
        {
            "Effect": "Allow",
            "Action": [
                "dynamodb:DeleteItem",
                "dynamodb:GetItem",
                "dynamodb:PutItem",
                "dynamodb:Scan",
                "dynamodb:UpdateItem"
            ],
            "Resource": "RESOURCE_ARN"
        }
    ]
};

var params = {
  PolicyDocument: JSON.stringify(myManagedPolicy),
  PolicyName: 'myDynamoDBPolicy',
};

iam.createPolicy(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node iam_createpolicy.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/iam/iam_createpolicy.js)\.

## Getting an IAM Policy<a name="iam-examples-policies-getting"></a>

Create a Node\.js module with the file name `iam_getpolicy.js`\. Be sure to configure the SDK as previously shown\. To access IAM, create an `AWS.IAM` service object\. Create a JSON object containing the parameters needed retrieve a policy, which is the ARN of the policy you want to get\. Call the `getPolicy` method of the `AWS.IAM` service object\. Write the policy description to the console\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var params = {
  PolicyArn: 'arn:aws:iam::aws:policy/AWSLambdaExecute'
};

iam.getPolicy(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.Policy.Description);
  }
});
```

To run the example, type the following at the command line\.

```
node iam_getpolicy.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/iam/iam_getpolicy.js)\.

## Attaching a Managed Role Policy<a name="iam-examples-policies-attaching-role-policy"></a>

Create a Node\.js module with the file name `iam_attachrolepolicy.js`\. Be sure to configure the SDK as previously shown\. To access IAM, create an `AWS.IAM` service object\. Create a JSON object containing the parameters needed to get a list of managed IAM policies attached to a role, which consists of the name of the role\. Provide the role name as a command\-line parameter\. Call the `listAttachedRolePolicies` method of the `AWS.IAM` service object, which returns an array of managed policies to the callback function\.

Check the array members to see if the policy you want to attach to the role is already attached\. If the policy is not attached, call the `attachRolePolicy` method to attach it\. 

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var paramsRoleList = {
  RoleName: process.argv[2]
};

iam.listAttachedRolePolicies(paramsRoleList, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    var myRolePolicies = data.AttachedPolicies;
    myRolePolicies.forEach(function (val, index, array) {
      if (myRolePolicies[index].PolicyName === 'AmazonDynamoDBFullAccess') {
        console.log("AmazonDynamoDBFullAccess is already attached to this role.")
        process.exit();
      }
    });
    var params = {
      PolicyArn: 'arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess',
      RoleName: process.argv[2]
    };
    iam.attachRolePolicy(params, function(err, data) {
      if (err) {
        console.log("Unable to attach policy to role", err);
      } else {
        console.log("Role attached successfully");
      }
    });
  }
});
```

To run the example, type the following at the command line\.

```
node iam_attachrolepolicy.js IAM_ROLE_NAME
```

## Detaching a Managed Role Policy<a name="iam-examples-policies-detaching-role-policy"></a>

Create a Node\.js module with the file name `iam_detachrolepolicy.js`\. Be sure to configure the SDK as previously shown\. To access IAM, create an `AWS.IAM` service object\. Create a JSON object containing the parameters needed to get a list of managed IAM policies attached to a role, which consists of the name of the role\. Provide the role name as a command\-line parameter\. Call the `listAttachedRolePolicies` method of the `AWS.IAM` service object, which returns an array of managed policies in the callback function\.

Check the array members to see if the policy you want to detach from the role is attached\. If the policy is attached, call the `detachRolePolicy` method to detach it\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var paramsRoleList = {
  RoleName: process.argv[2]
};

iam.listAttachedRolePolicies(paramsRoleList, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    var myRolePolicies = data.AttachedPolicies;
    myRolePolicies.forEach(function (val, index, array) {
      if (myRolePolicies[index].PolicyName === 'AmazonDynamoDBFullAccess') {
        var params = {
          PolicyArn: 'arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess',
          RoleName: process.argv[2]
        };
        iam.detachRolePolicy(params, function(err, data) {
          if (err) {
            console.log("Unable to detach policy from role", err);
          } else {
            console.log("Policy detached from role successfully");
            process.exit();
          }
        });
      }
    });
  }
});
```

To run the example, type the following at the command line\.

```
node iam_detachrolepolicy.js IAM_ROLE_NAME
```