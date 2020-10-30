--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Using elastic IP addresses in Amazon EC2<a name="ec2-example-elastic-ip-addresses"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve descriptions of your Elastic IP addresses\.
+ How to allocate and release an Elastic IP address\.
+ How to associate an Elastic IP address with an Amazon EC2 instance\.

## The scenario<a name="ec2-example-elastic-ip-addresses-scenario"></a>

An *Elastic IP address* is a static IP address designed for dynamic cloud computing\. An Elastic IP address is associated with your AWS account\. It is a public IP address, which is reachable from the internet\. If your instance does not have a public IP address, you can associate an Elastic IP address with your instance to enable communication with the internet\.

In this example, you use a series of Node\.js modules to perform several Amazon EC2 operations involving Elastic IP addresses\. The Node\.js modules use the SDK for JavaScript to manage Elastic IP addresses by using these methods of the Amazon EC2 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeAddresses-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeAddresses-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#allocateAddress-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#allocateAddress-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#associateAddress-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#associateAddress-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#releaseAddress-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#releaseAddress-property)

For more information about Elastic IP addresses in Amazon EC2, see [Elastic IP Addresses](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) in the *Amazon EC2 User Guide for Linux Instances* or [Elastic IP Addresses](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/elastic-ip-addresses-eip.html) in the *Amazon EC2 User Guide for Windows Instances*\.

## Prerequisite tasks<a name="ec2-example-elastic-ip-addresses-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/ec2/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create an Amazon EC2 instance\. For more information about creating Amazon EC2 instances, see [Amazon EC2 Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Instances.html) in the *Amazon EC2 User Guide for Linux Instances* or [Amazon EC2 Instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/Instances.html) in the *Amazon EC2 User Guide for Windows Instances*\.

## Describing elastic IP addresses<a name="ec2-example-elastic-ip-addresses-describing"></a>

Create a Node\.js module with the file name `ec2_describeaddresses.ts`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `EC2` service object\. Create a JSON object to pass as parameters, filtering the addresses returned by those in your VPC\. To retrieve descriptions of all your Elastic IP addresses, omit a filter from the parameters JSON\. Then call the `DescribeAddressesCommand` method of the Amazon EC2 service object\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region\.

```
// Import required AWS SDK clients and commands for Node.js
const { EC2Client, DescribeAddressesCommand } = require("@aws-sdk/client-ec2");

// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  Filters: [{ Name: "domain", Values: ["vpc"] }],
};

// Create EC2 service object
const ec2client = new EC2Client(REGION);

const run = async () => {
  try {
    const data = await ec2client.send(new DescribeAddressesCommand(params));
    console.log(JSON.stringify(data.Addresses));
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ec2_describeaddresses.ts // If you prefer JavaScript, enter 'node ec2_describeaddresses.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/src/ec2_describeaddresses.ts)\.

## Allocating and associating an elastic IP address with an Amazon EC2 instance<a name="ec2-example-elastic-ip-addresses-allocating-associating"></a>

Create a Node\.js module with the file name `ec2_allocateaddress.ts`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `EC2` client service object\. Create a JSON object for the parameters used to allocate an Elastic IP address, which in this case specifies the `Domain` is a VPC\. Call the `AllocateAddressCommand` method of the Amazon EC2 service object\.

If the call succeeds, the `data` parameter to the callback function has an `AllocationId` property that identifies the allocated Elastic IP address\.

Create a JSON object for the parameters used to associate an Elastic IP address to an Amazon EC2 instance, including the `AllocationId` from the newly allocated address and the `InstanceId` of the Amazon EC2 instance\. Then call the `AssociateAddressesCommand` method of the Amazon EC2 service object\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *INSTANCE\_ID* with the ID of the Amazon EC2 instance\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  EC2Client,
  AllocateAddressCommand,
  AssociateAddressCommand
} = require("@aws-sdk/client-ec2");

// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const paramsAllocateAddress = { Domain: "vpc" };

// Create EC2 service object
const ec2client = new EC2Client(REGION);

const run = async () => {
  try {
    const data = await ec2client.send(new AllocateAddressCommand(paramsAllocateAddress));
    console.log("Address allocated:", data.AllocationId);
    var paramsAssociateAddress = {
      AllocationId: data.AllocationId,
      InstanceId: "INSTANCE_ID", //INSTANCE_ID
    };
  } catch (err) {
    console.log("Address Not Allocated", err);
  }
  try {
    const results = await ec2client.send(new AssociateAddressCommand(paramsAssociateAddress));
    console.log("Address associated:", results.AssociationId);
  } catch (err) {
    console.log("Address Not Associated", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ec2_allocateaddress.ts // If you prefer JavaScript, enter 'node ec2_allocateaddress.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/src/ec2_allocateaddress.ts)\.

## Releasing an elastic IP address<a name="ec2-example-elastic-ip-addresses-releasing"></a>

Create a Node\.js module with the file name `ec2_releaseaddress.ts`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\. To access Amazon EC2, create an `EC2` client service object\. Create a JSON object for the parameters used to release an Elastic IP address, which in this case specifies the `AllocationId` for the Elastic IP address\. Releasing an Elastic IP address also disassociates it from any Amazon EC2 instance\. Call the `ReleaseAddressCommand` method of the Amazon EC2 service object\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *ALLOCATION\_ID* with the alloaction ID for the Elastic IP address\.

```
// Import required AWS SDK clients and commands for Node.js
const { EC2Client, ReleaseAddressCommand } = require("@aws-sdk/client-ec2");

// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"

// Create EC2 service object
const ec2client = new EC2Client(REGION);

// Set the parameters
const paramsReleaseAddress = { AllocationId: "ALLOCATION_ID" }; //ALLOCATION_ID

const run = async () => {
  try {
    const data = await ec2client.send(new ReleaseAddressCommand({}));
    console.log("Address released");
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ec2_releaseaddress.ts // If you prefer JavaScript, enter 'node ec2_releaseaddress.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/src/ec2_releaseaddress.ts)\.