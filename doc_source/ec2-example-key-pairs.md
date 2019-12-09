# Working with Amazon EC2 Key Pairs<a name="ec2-example-key-pairs"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve information about your key pairs\.
+ How to create a key pair to access an Amazon EC2 instance\.
+ How to delete an existing key pair\.

## The Scenario<a name="ec2-example-key-pairs-scenario"></a>

Amazon EC2 uses public–key cryptography to encrypt and decrypt login information\. Public–key cryptography uses a public key to encrypt data, then the recipient uses the private key to decrypt the data\. The public and private keys are known as a *key pair*\.

In this example, you use a series of Node\.js modules to perform several Amazon EC2 key pair management operations\. The Node\.js modules use the SDK for JavaScript to manage instances by using these methods of the Amazon EC2 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#createKeyPair-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#createKeyPair-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#deleteKeyPair-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#deleteKeyPair-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeKeyPairs-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeKeyPairs-property)

For more information about the Amazon EC2 key pairs, see [Amazon EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) in the *Amazon EC2 User Guide for Linux Instances* or [Amazon EC2 Key Pairs and Windows Instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-key-pairs.html) in the *Amazon EC2 User Guide for Windows Instances*\.

## Prerequisite Tasks<a name="ec2-example-key-pairs-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Describing Your Key Pairs<a name="ec2-example-key-pairs-describing"></a>

Create a Node\.js module with the file name `ec2_describekeypairs.js`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `AWS.EC2` service object\. Create an empty JSON object to hold the parameters needed by the `describeKeyPairs` method to return descriptions for all your key pairs\. You can also provide an array of names of key pairs in the `KeyName` portion of the parameters in the JSON file to the `describeKeyPairs` method\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create EC2 service object
var ec2 = new AWS.EC2({apiVersion: '2016-11-15'});

// Retrieve key pair descriptions; no params needed
ec2.describeKeyPairs(function(err, data) {
   if (err) {
      console.log("Error", err);
   } else {
      console.log("Success", JSON.stringify(data.KeyPairs));
   }
});
```

To run the example, type the following at the command line\.

```
node ec2_describekeypairs.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ec2/ec2_describekeypairs.js)\.

## Creating a Key Pair<a name="ec2-example-key-pairs-creating"></a>

Each key pair requires a name\. Amazon EC2 associates the public key with the name that you specify as the key name\. Create a Node\.js module with the file name `ec2_createkeypair.js`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `AWS.EC2` service object\. Create the JSON parameters to specify the name of the key pair, then pass them to call the `createKeyPair` method\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create EC2 service object
var ec2 = new AWS.EC2({apiVersion: '2016-11-15'});

var params = {
   KeyName: 'KEY_PAIR_NAME'
};

// Create the key pair
ec2.createKeyPair(params, function(err, data) {
   if (err) {
      console.log("Error", err);
   } else {
      console.log(JSON.stringify(data));
   }
});
```

To run the example, type the following at the command line\.

```
node ec2_createkeypair.js
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ec2/ec2_createkeypair.js)\.

## Deleting a Key Pair<a name="ec2-example-key-pairs-deleting"></a>

Create a Node\.js module with the file name `ec2_deletekeypair.js`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `AWS.EC2` service object\. Create the JSON parameters to specify the name of the key pair you want to delete\. Then call the `deleteKeyPair` method\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create EC2 service object
var ec2 = new AWS.EC2({apiVersion: '2016-11-15'});

var params = {
   KeyName: 'KEY_PAIR_NAME'
};

// Delete the key pair
ec2.deleteKeyPair(params, function(err, data) {
   if (err) {
      console.log("Error", err);
   } else {
      console.log("Key Pair Deleted");
   }
});
```

To run the example, type the following at the command line\.

```
node ec2_deletekeypair.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ec2/ec2_deletekeypair.js)\.