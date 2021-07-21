--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

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
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describeaddressescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describeaddressescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/allocateaddresscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/allocateaddresscommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/associateaddresscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/associateaddresscommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/releaseaddresscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/releaseaddresscommand.html)

For more information about Elastic IP addresses in Amazon EC2, see [Elastic IP Addresses](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) in the *Amazon EC2 User Guide for Linux Instances* or [Elastic IP Addresses](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/elastic-ip-addresses-eip.html) in the *Amazon EC2 User Guide for Windows Instances*\.

## Prerequisite tasks<a name="ec2-example-elastic-ip-addresses-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create an Amazon EC2 instance\. For more information about creating Amazon EC2 instances, see [Amazon EC2 Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Instances.html) in the *Amazon EC2 User Guide for Linux Instances* or [Amazon EC2 Instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/Instances.html) in the *Amazon EC2 User Guide for Windows Instances*\.

**Important**  
These examples use ECMAScript6 \(ES6\)\. This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.  
However, if you prefer to use CommonJS sytax, please refer to [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)

## Describing elastic IP addresses<a name="ec2-example-elastic-ip-addresses-describing"></a>

Create a Node\.js module with the file name `ec2_describeaddresses.js`\. Be sure to configure the SDK as previously shown\. Create a JSON object to pass as parameters, filtering the addresses returned by those in your VPC\. To retrieve descriptions of all your Elastic IP addresses, omit a filter from the parameters JSON\. Then call the `DescribeAddressesCommand` method of the Amazon EC2 service object\.

```
// Import required AWS SDK clients and commands for Node.js
import { EC2Client, DescribeAddressesCommand } from "@aws-sdk/client-ec2";
import { ec2Client } from "./libs/ec2Client";
// Set the parameters
const params = {
  Filters: [{ Name: "domain", Values: ["vpc"] }],
};

const run = async () => {
  try {
    const data = await ec2Client.send(new DescribeAddressesCommand(params));
    console.log(JSON.stringify(data.Addresses));
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ec2_describeaddresses.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/ec2_describeaddresses.js)\.

## Allocating and associating an elastic IP address with an Amazon EC2 instance<a name="ec2-example-elastic-ip-addresses-allocating-associating"></a>

Create a Node\.js module with the file name `ec2_allocateaddress.js`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `EC2` client service object\. Create a JSON object for the parameters used to allocate an Elastic IP address, which in this case specifies the `Domain` is a VPC\. Call the `AllocateAddressCommand` method of the Amazon EC2 service object\.

If the call succeeds, the `data` parameter to the callback function has an `AllocationId` property that identifies the allocated Elastic IP address\.

Create a JSON object for the parameters used to associate an Elastic IP address to an Amazon EC2 instance, including the `AllocationId` from the newly allocated address and the `InstanceId` of the Amazon EC2 instance\. Then call the `AssociateAddressesCommand` method of the Amazon EC2 service object\.

**Note**  
Replace *INSTANCE\_ID* with the ID of the Amazon EC2 instance\.

```
// Import required AWS SDK clients and commands for Node.js
import {
  AllocateAddressCommand,
  AssociateAddressCommand,
} from "@aws-sdk/client-ec2";
import { ec2Client } from "./libs/ec2Client";

// Set the parameters
const paramsAllocateAddress = { Domain: "vpc" };

const run = async () => {
  try {
    const data = await ec2Client.send(
      new AllocateAddressCommand(paramsAllocateAddress)
    );
    console.log("Address allocated:", data.AllocationId);
    return data;
    var paramsAssociateAddress = {
      AllocationId: data.AllocationId,
      InstanceId: "INSTANCE_ID", //INSTANCE_ID
    };
  } catch (err) {
    console.log("Address Not Allocated", err);
  }
  try {
    const results = await ec2Client.send(
      new AssociateAddressCommand(paramsAssociateAddress)
    );
    console.log("Address associated:", results.AssociationId);
    return results;
  } catch (err) {
    console.log("Address Not Associated", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ec2_allocateaddress.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/ec2_allocateaddress.js)\.

## Releasing an elastic IP address<a name="ec2-example-elastic-ip-addresses-releasing"></a>

Create a Node\.js module with the file name `ec2_releaseaddress.js`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\. To access Amazon EC2, create an `EC2` client service object\. Create a JSON object for the parameters used to release an Elastic IP address, which in this case specifies the `AllocationId` for the Elastic IP address\. Releasing an Elastic IP address also disassociates it from any Amazon EC2 instance\. Call the `ReleaseAddressCommand` method of the Amazon EC2 service object\.

**Note**  
Replace *ALLOCATION\_ID* with the alloaction ID for the Elastic IP address\.

```
// Import required AWS SDK clients and commands for Node.js
import { ReleaseAddressCommand } from "@aws-sdk/client-ec2";
import { ec2Client } from "./libs/ec2Client";

// Set the parameters
const paramsReleaseAddress = { AllocationId: "ALLOCATION_ID" }; //ALLOCATION_ID

const run = async () => {
  try {
    const data = await ec2Client.send(new ReleaseAddressCommand({}));
    console.log("Address released");
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ec2_releaseaddress.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/ec2_releaseaddress.js)\.