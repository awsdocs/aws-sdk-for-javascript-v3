# Managing IAM Access Keys<a name="iam-examples-managing-access-keys"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to manage the access keys of your users\.

## The Scenario<a name="iam-examples-managing-access-keys-scenario"></a>

Users need their own access keys to make programmatic calls to AWS from the SDK for JavaScript\. To fill this need, you can create, modify, view, or rotate access keys \(access key IDs and secret access keys\) for IAM users\. By default, when you create an access key, its status is `Active`, which means the user can use the access key for API calls\. 

In this example, a series of Node\.js modules are used manage access keys in IAM\. The Node\.js modules use the SDK for JavaScript to manage IAM access keys using these methods of the `AWS.IAM` client class:
+ [createAccessKey](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#createAccessKey-property)
+ [listAccessKeys](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#listAccessKeys-property)
+ [getAccessKeyLastUsed](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#getAccessKeyLastUsed-property)
+ [updateAccessKey](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#updateAccessKey-property)
+ [deleteAccessKey](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#deleteAccessKey-property)

For more information about IAM access keys, see [Access Keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html) in the *IAM User Guide*\.

## Prerequisite Tasks<a name="iam-examples-managing-access-keys-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Creating Access Keys for a User<a name="iam-examples-managing-access-keys-creating"></a>

Create a Node\.js module with the file name `iam_createaccesskeys.js`\. Be sure to configure the SDK as previously shown\. To access IAM, create an `AWS.IAM` service object\. Create a JSON object containing the parameters needed to create new access keys, which includes IAM user's name\. Call the `createAccessKey` method of the `AWS.IAM` service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

iam.createAccessKey({UserName: 'IAM_USER_NAME'}, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.AccessKey);
  }
});
```

To run the example, type the following at the command line\. Be sure to pipe the returned data to a text file in order not to lose the secret key, which can only be provided once\.

```
node iam_createaccesskeys.js > newuserkeys.txt
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/iam/iam_createaccesskeys.js)\.

## Listing a User's Access Keys<a name="iam-examples-managing-access-keys-listing"></a>

Create a Node\.js module with the file name `iam_listaccesskeys.js`\. Be sure to configure the SDK as previously shown\. To access IAM, create an `AWS.IAM` service object\. Create a JSON object containing the parameters needed to retrieve the user's access keys, which includes IAM user's name and optionally the maximum number of access key pairs you want listed\. Call the `listAccessKeys` method of the `AWS.IAM` service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var params = {
  MaxItems: 5,
  UserName: 'IAM_USER_NAME'
};

iam.listAccessKeys(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node iam_listaccesskeys.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/iam/iam_listaccesskeys.js)\.

## Getting the Last Use for Access Keys<a name="iam-examples-managing-access-keys-last-used"></a>

Create a Node\.js module with the file name `iam_accesskeylastused.js`\. Be sure to configure the SDK as previously shown\. To access IAM, create an `AWS.IAM` service object\. Create a JSON object containing the parameters needed to create new access keys, which is the access key ID for which you want the last use information\. Call the `getAccessKeyLastUsed` method of the `AWS.IAM` service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

iam.getAccessKeyLastUsed({AccessKeyId: 'ACCESS_KEY_ID'}, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.AccessKeyLastUsed);
  }
});
```

To run the example, type the following at the command line\.

```
node iam_accesskeylastused.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/iam/iam_accesskeylastused.js)\.

## Updating Access Key Status<a name="iam-examples-managing-access-keys-updating"></a>

Create a Node\.js module with the file name `iam_updateaccesskey.js`\. Be sure to configure the SDK as previously shown\. To access IAM, create an `AWS.IAM` service object\. Create a JSON object containing the parameters needed to update the status of an access keys, which includes the access key ID and the updated status\. The status can be `Active` or `Inactive`\. Call the `updateAccessKey` method of the `AWS.IAM` service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var params = {
  AccessKeyId: 'ACCESS_KEY_ID',
  Status: 'Active',
  UserName: 'USER_NAME'
};

iam.updateAccessKey(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node iam_updateaccesskey.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/iam/iam_updateaccesskey.js)\.

## Deleting Access Keys<a name="iam-examples-managing-access-keys-deleting"></a>

Create a Node\.js module with the file name `iam_deleteaccesskey.js`\. Be sure to configure the SDK as previously shown\. To access IAM, create an `AWS.IAM` service object\. Create a JSON object containing the parameters needed to delete access keys, which includes the access key ID and the name of the user\. Call the `deleteAccessKey` method of the `AWS.IAM` service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var params = {
  AccessKeyId: 'ACCESS_KEY_ID',
  UserName: 'USER_NAME'
};

iam.deleteAccessKey(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node iam_deleteaccesskey.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/iam/iam_deleteaccesskey.js)\.