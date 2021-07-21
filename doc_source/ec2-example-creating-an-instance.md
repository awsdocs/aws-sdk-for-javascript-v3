--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Creating an Amazon EC2 instance<a name="ec2-example-creating-an-instance"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create an Amazon EC2 instance from a public Amazon Machine Image \(AMI\)\.
+ How to create and assign tags to the new Amazon EC2 instance\.

## About the example<a name="ec2-example-creating-an-instance-scenario"></a>

In this example, you use a Node\.js module to create an Amazon EC2 instance and assign both a key pair and tags to it\. The code uses the SDK for JavaScript to create and tag an instance by using these methods of the Amazon EC2 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/runinstancescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/runinstancescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/createtagscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/createtagscommand.html)

## Prerequisite tasks<a name="ec2-example-creating-an-instance-prerequisites"></a>

To set up and run this example, first complete these tasks\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create a key pair\. For details, see [Working with Amazon EC2 key pairs](ec2-example-key-pairs.md)\. You use the name of the key pair in this example\.

**Important**  
These examples use ECMAScript6 \(ES6\)\. This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.  
However, if you prefer to use CommonJS sytax, please refer to [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)

## Creating and tagging an instance<a name="ec2-example-creating-an-instance-and-tags"></a>

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

Create a Node\.js module with the file name `ec2_createinstances.js`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the parameters for the `RunInstancesCommand` method of the `EC2` client class, including the name of the key pair to assign and the ID of the AMI to run\. To call the `RunInstancesCommand` method, create an asynchronous function for invoking an Amazon EC2 client service object, passing the parameters\. 

The code next adds a `Name` tag to a new instance, which the Amazon EC2 console recognizes and displays in the **Name** field of the instance list\. You can add up to 50 tags to an instance, all of which can be added in a single call to the `CreateTagsCommand` method\.

**Note**  
Replace *AMI\_ID* with the ID of the Amazon Machine Image \(AMI\) to run, and *KEY\_PAIR\_NAME* of the key pair to assign to the AMI ID\.

```
// Import required AWS SDK clients and commands for Node.js
const {
    CreateTagsCommand,
    RunInstancesCommand
} = require("@aws-sdk/client-ec2");
import { ec2Client } from "./libs/ec2Client";

// Set the parameters
const instanceParams = {
    ImageId: "AMI_ID", //AMI_ID
    InstanceType: "t2.micro",
    KeyName: "KEY_PAIR_NAME", //KEY_PAIR_NAME
    MinCount: 1,
    MaxCount: 1,
};

const run = async () => {
    try {
        const data = await ec2Client.send(new RunInstancesCommand(instanceParams));
        console.log(data.Instances[0].InstanceId);
        const instanceId = data.Instances[0].InstanceId;
        console.log("Created instance", instanceId);
        // Add tags to the instance
        const tagParams = {
            Resources: [instanceId],
            Tags: [
                {
                    Key: "Name",
                    Value: "SDK Sample",
                },
            ],
        };
        try {
            const data = await ec2Client.send(new CreateTagsCommand(tagParams));
            console.log("Instance tagged");
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
node ec2_createinstances.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/ec2_createinstances.js)\.