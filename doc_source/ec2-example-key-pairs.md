--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Working with Amazon EC2 key pairs<a name="ec2-example-key-pairs"></a>

![\[JavaScript code example that applies to Node.ts execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve information about your key pairs\.
+ How to create a key pair to access an Amazon EC2 instance\.
+ How to delete an existing key pair\.

## The scenario<a name="ec2-example-key-pairs-scenario"></a>

Amazon EC2 uses public–key cryptography to encrypt and decrypt login information\. Public–key cryptography uses a public key to encrypt data, then the recipient uses the private key to decrypt the data\. The public and private keys are known as a *key pair*\.

In this example, you use a series of Node\.js modules to perform several Amazon EC2 key pair management operations\. The Node\.js modules use the SDK for JavaScript to manage instances by using these methods of the Amazon EC2 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#createKeyPair-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#createKeyPair-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#deleteKeyPair-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#deleteKeyPair-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeKeyPairs-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeKeyPairs-property)

For more information about the Amazon EC2 key pairs, see [Amazon EC2 key pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key pairs.html) in the *Amazon EC2 User Guide for Linux Instances* or [Amazon EC2 key pairs and Windows Instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-key pairs.html) in the *Amazon EC2 User Guide for Windows Instances*\.

## Prerequisite tasks<a name="ec2-example-key-pairs-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/ec2/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these example can also be run in JavaScript\.
+ Install SDK for JavaScript Amazon EC2 client\. For more information, see [What's new in Version 3](welcome.md#welcome_whats_new_v3)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Describing your key pairs<a name="ec2-example-key-pairs-describing"></a>

Create a Node\.js module with the file name `ec2_describekeypairs.ts`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `EC2` client service object\. Create an empty JSON object to hold the parameters needed by the `DescribeKeyPairsCommand` method to return descriptions for all your key pairs\. You can also provide an array of names of key pairs in the `KeyName` portion of the parameters in the JSON file to the `DescribeKeyPairsCommand` method\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region\.

```
// Import required AWS SDK clients and commands for Node.js
const { EC2Client, DescribeKeyPairsCommand } = require("@aws-sdk/client-ec2");

// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"

// Create EC2 service object
const ec2client = new EC2Client(REGION);

const run = async () => {
  try {
    const data = await ec2client.send(new DescribeKeyPairsCommand({}));
    console.log("Success", JSON.stringify(data.KeyPairs));
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ec2_describekeypairs.ts // If you prefer JavaScript, enter 'node ec2_describekeypairs.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/src/ec2_describekeypairs.ts)\.

## Creating a key pair<a name="ec2-example-key-pairs-creating"></a>

Each key pair requires a name\. Amazon EC2 associates the public key with the name that you specify as the key name\. Create a Node\.js module with the file name `ec2_createkeypair.ts`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\. To access Amazon EC2, create an `EC2` client service object\. Create the JSON parameters to specify the name of the key pair, then pass them to call the `CreateKeyPairCommand` method\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *MY\_KEY\_PAIR* with the name of the key pair\.

```
// Import required AWS SDK clients and commands for Node.js
const { EC2Client, CreateKeyPairCommand } = require("@aws-sdk/client-ec2");

// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { KeyName: "MY_KEY_PAIR" }; //MY_KEY_PAIR

// Create EC2 service object
const ec2client = new EC2Client(REGION);

const run = async () => {
  try {
    const data = await ec2client.send(new CreateKeyPairCommand(params));
    console.log(JSON.stringify(data));
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ec2_createkeypair.ts // If you prefer JavaScript, enter 'node ec2_createkeypair.js'
```

The example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/src/ec2_createkeypair.ts)\.

## Deleting a key pair<a name="ec2-example-key-pairs-deleting"></a>

Create a Node\.js module with the file name `ec2_deletekeypair.ts`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\. To access Amazon EC2, create an `EC2` client service object\. Create the JSON parameters to specify the name of the key pair you want to delete\. Then call the `DeleteKeyPairCommand` method\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *KEY\_PAIR\_NAME* with the name of the key pair you want to delete\.

```
// Import required AWS SDK clients and commands for Node.js
const { EC2Client, DeleteKeyPairCommand } = require("@aws-sdk/client-ec2");
// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { KeyName: "KEY_PAIR_NAME" }; //KEY_PAIR_NAME

// Create EC2 service object
const ec2client = new EC2Client(REGION);

const run = async () => {
  try {
    const data = await ec2client.send(new DeleteKeyPairCommand(params));
    console.log("Key Pair Deleted");
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ec2_deletekeypair.ts // If you prefer JavaScript, enter 'node ec2_deletekeypair.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/src/ec2_deletekeypair.ts)\.