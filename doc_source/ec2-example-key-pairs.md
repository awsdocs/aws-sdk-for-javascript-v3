--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Working with Amazon EC2 key pairs<a name="ec2-example-key-pairs"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve information about your key pairs\.
+ How to create a key pair to access an Amazon EC2 instance\.
+ How to delete an existing key pair\.

## The scenario<a name="ec2-example-key-pairs-scenario"></a>

Amazon EC2 uses public–key cryptography to encrypt and decrypt login information\. Public–key cryptography uses a public key to encrypt data, then the recipient uses the private key to decrypt the data\. The public and private keys are known as a *key pair*\.

In this example, you use a series of Node\.js modules to perform several Amazon EC2 key pair management operations\. The Node\.js modules use the SDK for JavaScript to manage instances by using these methods of the Amazon EC2 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/createkeypaircommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/createkeypaircommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/deletekeypaircommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/deletekeypaircommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describekeypairscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describekeypairscommand.html)

For more information about the Amazon EC2 key pairs, see [Amazon EC2 key pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key pairs.html) in the *Amazon EC2 User Guide for Linux Instances* or [Amazon EC2 key pairs and Windows Instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/ec2-key pairs.html) in the *Amazon EC2 User Guide for Windows Instances*\.

## Prerequisite tasks<a name="ec2-example-key-pairs-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Install SDK for JavaScript Amazon EC2 client\. For more information, see [What's new in Version 3](welcome.md#welcome_whats_new_v3)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples use ECMAScript6 \(ES6\)\. This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.  
However, if you prefer to use CommonJS sytax, please refer to [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)

## Describing your key pairs<a name="ec2-example-key-pairs-describing"></a>

Create a `libs` directory, and create a Node\.js module with the file name `ec2Client.js`\. Copy and paste the code below into it, which creates the Amazon EC2 client object\. Replace *REGION* with your AWS Region\.

```
const  { EC2Client } = require( "@aws-sdk/client-ec2");
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create anAmazon EC2 service client object.
const ec2Client = new EC2Client({ region: REGION });
module.exports = { ec2Client };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/libs/ec2Client.js)\.

Create a Node\.js module with the file name `ec2_describekeypairs.js`\. Be sure to configure the SDK as previously shown\. Create an empty JSON object to hold the parameters needed by the `DescribeKeyPairsCommand` method to return descriptions for all your key pairs\. You can also provide an array of names of key pairs in the `KeyName` portion of the parameters in the JSON file to the `DescribeKeyPairsCommand` method\.

```
// Import required AWS SDK clients and commands for Node.js
import { DescribeKeyPairsCommand } from "@aws-sdk/client-ec2";
import { ec2Client } from "./libs/ec2Client";
const run = async () => {
  try {
    const data = await ec2Client.send(new DescribeKeyPairsCommand({}));
    console.log("Success", JSON.stringify(data.KeyPairs));
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ec2_describekeypairs.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/ec2_describekeypairs.js)\.

## Creating a key pair<a name="ec2-example-key-pairs-creating"></a>

Create a `libs` directory, and create a Node\.js module with the file name `ec2Client.js`\. Copy and paste the code below into it, which creates the Amazon EC2 client object\. Replace *REGION* with your AWS Region\.

```
const  { EC2Client } = require( "@aws-sdk/client-ec2");
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create anAmazon EC2 service client object.
const ec2Client = new EC2Client({ region: REGION });
module.exports = { ec2Client };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/libs/ec2Client.js)\.

Each key pair requires a name\. Amazon EC2 associates the public key with the name that you specify as the key name\. Create a Node\.js module with the file name `ec2_createkeypair.js`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\. Create the JSON parameters to specify the name of the key pair, then pass them to call the `CreateKeyPairCommand` method\.

**Note**  
Replace *MY\_KEY\_PAIR* with the name of the key pair\.

```
// Import required AWS SDK clients and commands for Node.js
import { CreateKeyPairCommand } from "@aws-sdk/client-ec2";
import { ec2Client } from "./libs/ec2Client";

// Set the parameters
const params = { KeyName: "MY_KEY_PAIR" }; //MY_KEY_PAIR

const run = async () => {
  try {
    const data = await ec2Client.send(new CreateKeyPairCommand(params));
    console.log(JSON.stringify(data));
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ec2_createkeypair.js 
```

The example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/ec2_createkeypair.js)\.

## Deleting a key pair<a name="ec2-example-key-pairs-deleting"></a>

Create a `libs` directory, and create a Node\.js module with the file name `ec2Client.js`\. Copy and paste the code below into it, which creates the Amazon EC2 client object\. Replace *REGION* with your AWS Region\.

```
const  { EC2Client } = require( "@aws-sdk/client-ec2");
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create anAmazon EC2 service client object.
const ec2Client = new EC2Client({ region: REGION });
module.exports = { ec2Client };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/libs/ec2Client.js)\.

Create a Node\.js module with the file name `ec2_deletekeypair.js`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\. To access Amazon EC2, create an `EC2` client service object\. Create the JSON parameters to specify the name of the key pair you want to delete\. Then call the `DeleteKeyPairCommand` method\.

**Note**  
Replace *KEY\_PAIR\_NAME* with the name of the key pair you want to delete\.

```
// Import required AWS SDK clients and commands for Node.js
import { DeleteKeyPairCommand } from "@aws-sdk/client-ec2";
import { ec2Client } from "./libs/ec2Client";

// Set the parameters
const params = { KeyName: "KEY_PAIR_NAME" }; //KEY_PAIR_NAME

const run = async () => {
  try {
    const data = await ec2Client.send(new DeleteKeyPairCommand(params));
    console.log("Key Pair Deleted");
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ec2_deletekeypair.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/ec2_deletekeypair.js)\.