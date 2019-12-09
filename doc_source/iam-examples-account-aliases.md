# Managing IAM Account Aliases<a name="iam-examples-account-aliases"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to manage aliases for your AWS account ID\.

## The Scenario<a name="iam-examples-account-aliases-scenario"></a>

If you want the URL for your sign\-in page to contain your company name or other friendly identifier instead of your AWS account ID, you can create an alias for your AWS account ID\. If you create an AWS account alias, your sign\-in page URL changes to incorporate the alias\.

In this example, a series of Node\.js modules are used to create and manage IAM account aliases\. The Node\.js modules use the SDK for JavaScript to manage aliases using these methods of the `AWS.IAM` client class:
+ [createAccountAlias](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#createAccountAlias-property)
+ [listAccountAliases](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#listAccountAliases-property)
+ [deleteAccountAlias](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#deleteAccountAlias-property)

For more information about IAM account aliases, see [Your AWS Account ID and Its Alias](https://docs.aws.amazon.com/IAM/latest/UserGuide/console_account-alias.html) in the *IAM User Guide*\.

## Prerequisite Tasks<a name="iam-examples-account-aliases-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Creating an Account Alias<a name="iam-examples-account-aliases-creating"></a>

Create a Node\.js module with the file name `iam_createaccountalias.js`\. Be sure to configure the SDK as previously shown\. To access IAM, create an `AWS.IAM` service object\. Create a JSON object containing the parameters needed to create an account alias, which includes the alias you want to create\. Call the `createAccountAlias` method of the `AWS.IAM` service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

iam.createAccountAlias({AccountAlias: process.argv[2]}, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node iam_createaccountalias.js ALIAS
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/iam/iam_createaccountalias.js)\.

## Listing Account Aliases<a name="iam-examples-account-aliases-listing"></a>

Create a Node\.js module with the file name `iam_listaccountaliases.js`\. Be sure to configure the SDK as previously shown\. To access IAM, create an `AWS.IAM` service object\. Create a JSON object containing the parameters needed to list account aliases, which includes the maximum number of items to return\. Call the `listAccountAliases` method of the `AWS.IAM` service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

iam.listAccountAliases({MaxItems: 10}, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node iam_listaccountaliases.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/iam/iam_listaccountaliases.js)\.

## Deleting an Account Alias<a name="iam-examples-account-aliases-deleting"></a>

Create a Node\.js module with the file name `iam_deleteaccountalias.js`\. Be sure to configure the SDK as previously shown\. To access IAM, create an `AWS.IAM` service object\. Create a JSON object containing the parameters needed to delete an account alias, which includes the alias you want deleted\. Call the `deleteAccountAlias` method of the `AWS.IAM` service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

iam.deleteAccountAlias({AccountAlias: process.argv[2]}, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node iam_deleteaccountalias.js ALIAS
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/iam/iam_deleteaccountalias.js)\.