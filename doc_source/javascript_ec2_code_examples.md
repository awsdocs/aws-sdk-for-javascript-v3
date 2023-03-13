--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Amazon EC2 examples using SDK for JavaScript \(v3\)<a name="javascript_ec2_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for JavaScript \(v3\) with Amazon EC2\.

*Actions* are code excerpts that show you how to call individual service functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple functions within the same service\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Get started**

## Hello Amazon EC2<a name="example_ec2_Hello_section"></a>

The following code examples show how to get started using Amazon Elastic Compute Cloud \(Amazon EC2\)\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { DescribeSecurityGroupsCommand } from "@aws-sdk/client-ec2";

import { client } from "./libs/client.js";

// Call DescribeSecurityGroups and display the result.
export const main = async () => {
  try {
    const { SecurityGroups } = await client.send(
      new DescribeSecurityGroupsCommand({})
    );

    const securityGroupList = SecurityGroups.slice(0, 9)
      .map((sg) => ` • ${sg.GroupId}: ${sg.GroupName}`)
      .join("\n");

    console.log(
      "Hello, Amazon EC2! Let's list up to 10 of your security groups:"
    );
    console.log(securityGroupList);
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [DescribeSecurityGroups](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describesecuritygroupscommand.html) in *AWS SDK for JavaScript API Reference*\. 

**Topics**
+ [Actions](#actions)
+ [Scenarios](#scenarios)

## Actions<a name="actions"></a>

### Allocate an Elastic IP address<a name="ec2_AllocateAddress_javascript_topic"></a>

The following code example shows how to allocate an Elastic IP address for Amazon EC2\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { AllocateAddressCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

export const main = async () => {
  const command = new AllocateAddressCommand({});

  try {
    const { AllocationId, PublicIp } = await client.send(command);
    console.log("A new IP address has been allocated to your account:");
    console.log(`ID: ${AllocationId} Public IP: ${PublicIp}`);
    console.log(
      "You can view your IP addresses in the AWS Management Console for Amazon EC2. Look under Network & Security > Elastic IPs"
    );
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [AllocateAddress](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/allocateaddresscommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Associate an Elastic IP address with an instance<a name="ec2_AssociateAddress_javascript_topic"></a>

The following code example shows how to associate an Elastic IP address with an Amazon EC2 instance\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { AssociateAddressCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

export const main = async () => {
  // You need to allocate an Elastic IP address before associating it with an instance.
  // You can do that with the AllocateAddressCommand.
  const allocationId = "ALLOCATION_ID";
  // You need to create an EC2 instance before an IP address can be associated with it.
  // You can do that with the RunInstancesCommand.
  const instanceId = "INSTANCE_ID";
  const command = new AssociateAddressCommand({
    AllocationId: allocationId,
    InstanceId: instanceId,
  });

  try {
    const { AssociationId } = await client.send(command);
    console.log(
      `Address with allocation ID ${allocationId} is now associated with instance ${instanceId}.`,
      `The association ID is ${AssociationId}.`
    );
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [AssociateAddress](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/associateaddresscommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create a security group<a name="ec2_CreateSecurityGroup_javascript_topic"></a>

The following code example shows how to create an Amazon EC2 security group\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { CreateSecurityGroupCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

export const main = async () => {
  const command = new CreateSecurityGroupCommand({
    // Up to 255 characters in length. Cannot start with sg-.
    GroupName: "SECURITY_GROUP_NAME",
    // Up to 255 characters in length.
    Description: "DESCRIPTION",
  });

  try {
    const { GroupId } = await client.send(command);
    console.log(GroupId);
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [CreateSecurityGroup](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/createsecuritygroupcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create a security key pair<a name="ec2_CreateKeyPair_javascript_topic"></a>

The following code example shows how to create a security key pair for Amazon EC2\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { CreateKeyPairCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

export const main = async () => {
  try {
    // Create a key pair in Amazon EC2.
    const { KeyMaterial, KeyName } = await client.send(
      // A unique name for the key pair. Up to 255 ASCII characters.
      new CreateKeyPairCommand({ KeyName: "KEY_PAIR_NAME" })
    );
    // This logs your private key. Be sure to save it.
    console.log(KeyName);
    console.log(KeyMaterial);
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [CreateKeyPair](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/createkeypaircommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create and run an instance<a name="ec2_RunInstances_javascript_topic"></a>

The following code example shows how to create and run an Amazon EC2 instance\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { RunInstancesCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

// Create a new EC2 instance.
export const main = async () => {
  const command = new RunInstancesCommand({
    // Your key pair name.
    KeyName: "KEY_PAIR_NAME",
    // Your security group.
    SecurityGroupIds: ["SECURITY_GROUP_ID"],
    // An x86_64 compatible image.
    ImageId: "ami-0001a0d1a04bfcc30",
    // An x86_64 compatible free-tier instance type.
    InstanceType: "t1.micro",
    // Ensure only 1 instance launches.
    MinCount: 1,
    MaxCount: 1,
  });

  try {
    const response = await client.send(command);
    console.log(response);
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [RunInstances](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/runinstancescommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Delete a security group<a name="ec2_DeleteSecurityGroup_javascript_topic"></a>

The following code example shows how to delete an Amazon EC2 security group\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { DeleteSecurityGroupCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

export const main = async () => {
  const command = new DeleteSecurityGroupCommand({
    GroupId: "GROUP_ID",
  });

  try {
    await client.send(command);
    console.log("Security group deleted successfully.");
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [DeleteSecurityGroup](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/deletesecuritygroupcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Delete a security key pair<a name="ec2_DeleteKeyPair_javascript_topic"></a>

The following code example shows how to delete an Amazon EC2 security key pair\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { DeleteKeyPairCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

export const main = async () => {
  const command = new DeleteKeyPairCommand({
    KeyName: "KEY_PAIR_NAME",
  });

  try {
    await client.send(command);
    console.log("Successfully deleted key pair.");
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [DeleteKeyPair](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/deletekeypaircommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Describe Regions<a name="ec2_DescribeRegions_javascript_topic"></a>

The following code example shows how to describe Amazon EC2 Regions\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { DescribeRegionsCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

export const main = async () => {
  const command = new DescribeRegionsCommand({
    // By default this command will not show regions that require you to opt-in.
    // When AllRegions true even the regions that require opt-in will be returned.
    AllRegions: true,
    // You can omit the Filters property if you want to get all regions.
    Filters: [
      {
        Name: "region-name",
        // You can specify multiple values for a filter.
        // You can also use '*' as a wildcard. This will return all
        // of the regions that start with `us-east-`.
        Values: ["ap-southeast-4"],
      },
    ],
  });

  try {
    const { Regions } = await client.send(command);
    const regionsList = Regions.map((reg) => ` • ${reg.RegionName}`);
    console.log("Found regions:");
    console.log(regionsList.join("\n"));
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [DescribeRegions](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describeregionscommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Describe instances<a name="ec2_DescribeInstances_javascript_topic"></a>

The following code example shows how to describe Amazon EC2 instances\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { DescribeInstancesCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

// List all of your EC2 instances running with x86_64 architecture that were
// launched this month.
export const main = async () => {
  const d = new Date();
  const year = d.getFullYear();
  const month = `0${d.getMonth() + 1}`.slice(-2);
  const launchTimePattern = `${year}-${month}-*`;
  const command = new DescribeInstancesCommand({
    Filters: [
      { Name: "architecture", Values: ["x86_64"] },
      { Name: "instance-state-name", Values: ["running"] },
      {
        Name: "launch-time",
        Values: [launchTimePattern],
      },
    ],
  });

  try {
    const { Reservations } = await client.send(command);
    const instanceList = Reservations.reduce((prev, current) => {
      return prev.concat(current.Instances);
    }, []);

    console.log(instanceList);
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [DescribeInstances](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describeinstancescommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Disable detailed monitoring<a name="ec2_UnmonitorInstances_javascript_topic"></a>

The following code example shows how to disable detailed monitoring on an Amazon EC2 instance\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { UnmonitorInstancesCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

export const main = async () => {
  const command = new UnmonitorInstancesCommand({
    InstanceIds: ["i-09a3dfe7ae00e853f"],
  });

  try {
    const { InstanceMonitorings } = await client.send(command);
    const instanceMonitoringsList = InstanceMonitorings.map(
      (im) =>
        ` • Detailed monitoring state for ${im.InstanceId} is ${im.Monitoring.State}.`
    );
    console.log("Monitoring status:");
    console.log(instanceMonitoringsList.join("\n"));
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [UnmonitorInstances](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/unmonitorinstancescommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Disassociate an Elastic IP address from an instance<a name="ec2_DisassociateAddress_javascript_topic"></a>

The following code example shows how to disassociate an Elastic IP address from an Amazon EC2 instance\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { DisassociateAddressCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

// Disassociate an Elastic IP address from an instance.
export const main = async () => {
  const command = new DisassociateAddressCommand({
    // You can also use PublicIp, but that is for EC2 classic which is being retired.
    AssociationId: "ASSOCIATION_ID",
  });

  try {
    await client.send(command);
    console.log("Successfully disassociated address");
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [DisassociateAddress](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/disassociateaddresscommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Enable monitoring<a name="ec2_MonitorInstances_javascript_topic"></a>

The following code example shows how to enable monitoring for a running Amazon EC2 instance\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { MonitorInstancesCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

// Turn on detailed monitoring for the selected instance.
// By default, metrics are sent to Amazon CloudWatch every 5 minutes.
// For a cost you can enable detailed monitoring which sends metrics every minute.
export const main = async () => {
  const command = new MonitorInstancesCommand({
    InstanceIds: ["INSTANCE_ID"],
  });

  try {
    const { InstanceMonitorings } = await client.send(command);
    const instancesBeingMonitored = InstanceMonitorings.map(
      (im) =>
        ` • Detailed monitoring state for ${im.InstanceId} is ${im.Monitoring.State}.`
    );
    console.log("Monitoring status:");
    console.log(instancesBeingMonitored.join("\n"));
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [MonitorInstances](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/monitorinstancescommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Get data about Amazon Machine Images<a name="ec2_DescribeImages_javascript_topic"></a>

The following code example shows how to get data about Amazon Machine Images \(AMIs\)\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { paginateDescribeImages } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

// List at least the first i386 image available for EC2 instances.
export const main = async () => {
  // The paginate function is a wrapper around the base command.
  const paginator = paginateDescribeImages(
    // Without limiting the page size, this call can take a long time. pageSize is just sugar for
    // the MaxResults property in the base command.
    { client, pageSize: 25 },
    {
      // There are almost 70,000 images available. Be specific with your filtering
      // to increase efficiency.
      // See https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/interfaces/describeimagescommandinput.html#filters
      Filters: [{ Name: "architecture", Values: ["x86_64"] }],
    }
  );

  try {
    const arm64Images = [];
    for await (const page of paginator) {
      if (page.Images.length) {
        arm64Images.push(...page.Images);
        // Once we have at least 1 result, we can stop.
        if (arm64Images.length >= 1) {
          break;
        }
      }
    }
    console.log(arm64Images);
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [DescribeImages](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describeimagescommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Get data about a security group<a name="ec2_DescribeSecurityGroups_javascript_topic"></a>

The following code example shows how to get data about an Amazon EC2 security group\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { DescribeSecurityGroupsCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

// Log the details of a specific security group.
export const main = async () => {
  const command = new DescribeSecurityGroupsCommand({
    GroupIds: ["SECURITY_GROUP_ID"],
  });

  try {
    const { SecurityGroups } = await client.send(command);
    console.log(JSON.stringify(SecurityGroups, null, 2));
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [DescribeSecurityGroups](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describesecuritygroupscommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Get data about instance types<a name="ec2_DescribeInstanceTypes_javascript_topic"></a>

The following code example shows how to get data about Amazon EC2 instance types\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import {
  paginateDescribeInstanceTypes,
  DescribeInstanceTypesCommand,
} from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

// List at least the first arm64 EC2 instance type available.
export const main = async () => {
  // The paginate function is a wrapper around the underlying command.
  const paginator = paginateDescribeInstanceTypes(
    // Without limiting the page size, this call can take a long time. pageSize is just sugar for
    // the MaxResults property in the underlying command.
    { client, pageSize: 25 },
    {
      Filters: [
        { Name: "processor-info.supported-architecture", Values: ["x86_64"] },
        { Name: "free-tier-eligible", Values: ["true"] },
      ],
    }
  );

  try {
    const instanceTypes = [];

    for await (const page of paginator) {
      if (page.InstanceTypes.length) {
        instanceTypes.push(...page.InstanceTypes);

        // When we have at least 1 result, we can stop.
        if (instanceTypes.length >= 1) {
          break;
        }
      }
    }
    console.log(instanceTypes);
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [DescribeInstanceTypes](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describeinstancetypescommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Get details about Elastic IP addresses<a name="ec2_DescribeAddresses_javascript_topic"></a>

The following code example shows how to get details about Elastic IP addresses\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { DescribeAddressesCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

export const main = async () => {
  const command = new DescribeAddressesCommand({
    // You can omit this property to show all addresses.
    AllocationIds: ["ALLOCATION_ID"],
  });

  try {
    const { Addresses } = await client.send(command);
    const addressList = Addresses.map((address) => ` • ${address.PublicIp}`);
    console.log("Elastic IP addresses:");
    console.log(addressList.join("\n"));
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [DescribeAddresses](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describeaddressescommand.html) in *AWS SDK for JavaScript API Reference*\. 

### List security key pairs<a name="ec2_DescribeKeyPairs_javascript_topic"></a>

The following code example shows how to list Amazon EC2 security key pairs\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { DescribeKeyPairsCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

export const main = async () => {
  const command = new DescribeKeyPairsCommand({});

  try {
    const { KeyPairs } = await client.send(command);
    const keyPairList = KeyPairs.map(
      (kp) => ` • ${kp.KeyPairId}: ${kp.KeyName}`
    ).join("\n");
    console.log("The following key pairs were found in your account:");
    console.log(keyPairList);
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [DescribeKeyPairs](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describekeypairscommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Reboot an instance<a name="ec2_RebootInstances_javascript_topic"></a>

The following code example shows how to reboot an Amazon EC2 instance\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { RebootInstancesCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

export const main = async () => {
  const command = new RebootInstancesCommand({
    InstanceIds: ["INSTANCE_ID"],
  });

  try {
    await client.send(command);
    console.log("Instance rebooted successfully.");
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [RebootInstances](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/rebootinstancescommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Release an Elastic IP address<a name="ec2_ReleaseAddress_javascript_topic"></a>

The following code example shows how to release an Elastic IP address\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { ReleaseAddressCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

export const main = async () => {
  const command = new ReleaseAddressCommand({
    // You can also use PublicIp, but that is for EC2 classic which is being retired.
    AllocationId: "ALLOCATION_ID",
  });

  try {
    await client.send(command);
    console.log("Successfully released address.");
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [ReleaseAddress](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/releaseaddresscommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Set inbound rules for a security group<a name="ec2_AuthorizeSecurityGroupIngress_javascript_topic"></a>

The following code example shows how to set inbound rules for an Amazon EC2 security group\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { AuthorizeSecurityGroupIngressCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

// Grant permissions for a single IP address to ssh into instances
// within the provided security group.
export const main = async () => {
  const command = new AuthorizeSecurityGroupIngressCommand({
    // Replace with a security group ID from the AWS console or
    // the DescribeSecurityGroupsCommand.
    GroupId: "SECURITY_GROUP_ID",
    IpPermissions: [
      {
        IpProtocol: "tcp",
        FromPort: 22,
        ToPort: 22,
        // Replace 0.0.0.0 with the IP address to authorize.
        // For more information on this notation, see
        // https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing#CIDR_notation
        IpRanges: [{ CidrIp: "0.0.0.0/32" }],
      },
    ],
  });

  try {
    const { SecurityGroupRules } = await client.send(command);
    console.log(JSON.stringify(SecurityGroupRules, null, 2));
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [AuthorizeSecurityGroupIngress](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/authorizesecuritygroupingresscommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Start an instance<a name="ec2_StartInstances_javascript_topic"></a>

The following code example shows how to start an Amazon EC2 instance\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { StartInstancesCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

export const main = async () => {
  const command = new StartInstancesCommand({
    // Use DescribeInstancesCommand to find InstanceIds
    InstanceIds: ["INSTANCE_ID"],
  });

  try {
    const { StartingInstances } = await client.send(command);
    const instanceIdList = StartingInstances.map(
      (instance) => ` • ${instance.InstanceId}`
    );
    console.log("Starting instances:");
    console.log(instanceIdList.join("\n"));
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [StartInstances](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/startinstancescommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Stop an instance<a name="ec2_StopInstances_javascript_topic"></a>

The following code example shows how to stop an Amazon EC2 instance\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { StopInstancesCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

export const main = async () => {
  const command = new StopInstancesCommand({
    // Use DescribeInstancesCommand to find InstanceIds
    InstanceIds: ["INSTANCE_ID"],
  });

  try {
    const { StoppingInstances } = await client.send(command);
    const instanceIdList = StoppingInstances.map(
      (instance) => ` • ${instance.InstanceId}`
    );
    console.log("Stopping instances:");
    console.log(instanceIdList.join("\n"));
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [StopInstances](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/stopinstancescommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Terminate an instance<a name="ec2_TerminateInstances_javascript_topic"></a>

The following code example shows how to terminate an Amazon EC2 instance\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
  

```
import { TerminateInstancesCommand } from "@aws-sdk/client-ec2";

import { client } from "../libs/client.js";

export const main = async () => {
  const command = new TerminateInstancesCommand({
    InstanceIds: ["INSTANCE_ID"],
  });

  try {
    const { TerminatingInstances } = await client.send(command);
    const instanceList = TerminatingInstances.map(
      (instance) => ` • ${instance.InstanceId}`
    );
    console.log("Terminating instances:");
    console.log(instanceList.join("\n"));
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [TerminateInstances](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/terminateinstancescommand.html) in *AWS SDK for JavaScript API Reference*\. 

## Scenarios<a name="scenarios"></a>

### Get started with instances<a name="ec2_Scenario_GetStartedInstances_javascript_topic"></a>

The following code example shows how to:
+ Create a key pair that is used to secure SSH communication between your computer and an EC2 instance\.
+ Create a security group that acts as a virtual firewall for your EC2 instances to control incoming and outgoing traffic\.
+ Find an Amazon Machine Image \(AMI\) and a compatible instance type\.
+ Create an instance that is created from the instance type and AMI you select, and is configured to use the security group and key pair created in this example\.
+ Stop and restart the instance\.
+ Create an Elastic IP address and associate it as a consistent IP address for your instance\.
+ Connect to your instance with SSH, using both its public IP address and your Elastic IP address\.
+ Clean up all of the resources created by this example\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/ec2#code-examples)\. 
Run an interactive scenario at a command prompt\.  

```
import { mkdtempSync, writeFileSync, rmSync } from "fs";
import { tmpdir } from "os";
import { join } from "path";
import { createInterface } from "readline";
import { get } from "http";

import {
  AllocateAddressCommand,
  AssociateAddressCommand,
  AuthorizeSecurityGroupIngressCommand,
  CreateKeyPairCommand,
  CreateSecurityGroupCommand,
  DeleteKeyPairCommand,
  DeleteSecurityGroupCommand,
  DescribeInstancesCommand,
  DescribeKeyPairsCommand,
  DescribeSecurityGroupsCommand,
  DisassociateAddressCommand,
  EC2Client,
  paginateDescribeImages,
  paginateDescribeInstanceTypes,
  ReleaseAddressCommand,
  RunInstancesCommand,
  StartInstancesCommand,
  StopInstancesCommand,
  TerminateInstancesCommand,
  waitUntilInstanceStatusOk,
  waitUntilInstanceStopped,
  waitUntilInstanceTerminated,
} from "@aws-sdk/client-ec2";
import { paginateGetParametersByPath, SSMClient } from "@aws-sdk/client-ssm";

import { promptToSelect, promptToContinue } from "libs/utils/util-io.js";
import { wrapText } from "libs/utils/util-string.js";

const ec2Client = new EC2Client();
const ssmClient = new SSMClient();

const tmpDirectory = mkdtempSync(join(tmpdir(), "ec2-scenario-tmp"));

const createKeyPair = async (keyPairName) => {
  // Create a key pair in Amazon EC2.
  const { KeyMaterial, KeyPairId } = await ec2Client.send(
    // A unique name for the key pair. Up to 255 ASCII characters.
    new CreateKeyPairCommand({ KeyName: keyPairName })
  );

  // Save the private key in a temporary location.
  writeFileSync(`${tmpDirectory}/${keyPairName}.pem`, KeyMaterial, {
    mode: 0o400,
  });

  return KeyPairId;
};

const describeKeyPair = async (keyPairName) => {
  const command = new DescribeKeyPairsCommand({
    KeyNames: [keyPairName],
  });
  const { KeyPairs } = await ec2Client.send(command);
  return KeyPairs[0];
};

const createSecurityGroup = async (securityGroupName) => {
  const command = new CreateSecurityGroupCommand({
    GroupName: securityGroupName,
    Description: "A security group for the Amazon EC2 example.",
  });
  const { GroupId } = await ec2Client.send(command);
  return GroupId;
};

const allocateIpAddress = async () => {
  const command = new AllocateAddressCommand({});
  const { PublicIp, AllocationId } = await ec2Client.send(command);
  return { PublicIp, AllocationId };
};

const getLocalIpAddress = async () => {
  return new Promise((res, rej) => {
    get("http://checkip.amazonaws.com", (response) => {
      let data = "";
      response.on("data", (chunk) => (data += chunk));
      response.on("end", () => res(data.trim()));
    }).on("error", (err) => {
      rej(err);
    });
  });
};

const authorizeSecurityGroupIngress = async (securityGroupId) => {
  const ipAddress = await getLocalIpAddress();
  const command = new AuthorizeSecurityGroupIngressCommand({
    GroupId: securityGroupId,
    IpPermissions: [
      {
        IpProtocol: "tcp",
        FromPort: 22,
        ToPort: 22,
        IpRanges: [{ CidrIp: `${ipAddress}/32` }],
      },
    ],
  });

  await ec2Client.send(command);
  return ipAddress;
};

const describeSecurityGroup = async (securityGroupName) => {
  const command = new DescribeSecurityGroupsCommand({
    GroupNames: [securityGroupName],
  });
  const { SecurityGroups } = await ec2Client.send(command);

  return SecurityGroups[0];
};

const getAmznLinux2AMIs = async () => {
  const AMIs = [];
  for await (const page of paginateGetParametersByPath(
    {
      client: ssmClient,
    },
    { Path: "/aws/service/ami-amazon-linux-latest" }
  )) {
    page.Parameters.forEach((param) => {
      if (param.Name.includes("amzn2")) {
        AMIs.push(param.Value);
      }
    });
  }

  const imageDetails = [];

  for await (const page of paginateDescribeImages(
    { client: ec2Client },
    { ImageIds: AMIs }
  )) {
    imageDetails.push(...page.Images);
  }

  const options = imageDetails.map(
    (image) => `${image.ImageId} - ${image.Description}`
  );
  const [selectedIndex] = await promptToSelect(options);

  return imageDetails[selectedIndex];
};

const getCompatibleInstanceTypes = async (imageDetails) => {
  const paginator = paginateDescribeInstanceTypes(
    { client: ec2Client, pageSize: 25 },
    {
      Filters: [
        {
          Name: "processor-info.supported-architecture",
          Values: [imageDetails.Architecture],
        },
        { Name: "instance-type", Values: ["*.micro", "*.small"] },
      ],
    }
  );

  const instanceTypes = [];

  for await (const page of paginator) {
    if (page.InstanceTypes.length) {
      instanceTypes.push(...page.InstanceTypes);
    }
  }

  const instanceTypeList = instanceTypes.map(
    (type) => `${type.InstanceType} - Memory:${type.MemoryInfo.SizeInMiB}`
  );

  const [selectedIndex] = await promptToSelect(
    instanceTypeList,
    "Select an instance type."
  );
  return instanceTypes[selectedIndex];
};

const runInstance = async ({
  keyPairName,
  securityGroupId,
  imageId,
  instanceType,
}) => {
  const command = new RunInstancesCommand({
    KeyName: keyPairName,
    SecurityGroupIds: [securityGroupId],
    ImageId: imageId,
    InstanceType: instanceType,
    MinCount: 1,
    MaxCount: 1,
  });

  const { Instances } = await ec2Client.send(command);
  await waitUntilInstanceStatusOk(
    { client: ec2Client },
    { InstanceIds: [Instances[0].InstanceId] }
  );
  return Instances[0].InstanceId;
};

const describeInstance = async (instanceId) => {
  const command = new DescribeInstancesCommand({
    InstanceIds: [instanceId],
  });

  const { Reservations } = await ec2Client.send(command);
  return Reservations[0].Instances[0];
};

const displaySSHConnectionInfo = ({ publicIp, keyPairName }) => {
  return `ssh -i ${tmpDirectory}/${keyPairName}.pem ec2-user@${publicIp}`;
};

const stopInstance = async (instanceId) => {
  const command = new StopInstancesCommand({ InstanceIds: [instanceId] });
  await ec2Client.send(command);
  await waitUntilInstanceStopped(
    { client: ec2Client },
    { InstanceIds: [instanceId] }
  );
};

const startInstance = async (instanceId) => {
  const startCommand = new StartInstancesCommand({ InstanceIds: [instanceId] });
  await ec2Client.send(startCommand);
  await waitUntilInstanceStatusOk(
    { client: ec2Client },
    { InstanceIds: [instanceId] }
  );
  return await describeInstance(instanceId);
};

const associateAddress = async ({ allocationId, instanceId }) => {
  const command = new AssociateAddressCommand({
    AllocationId: allocationId,
    InstanceId: instanceId,
  });

  const { AssociationId } = await ec2Client.send(command);
  return AssociationId;
};

const disassociateAddress = async (associationId) => {
  const command = new DisassociateAddressCommand({
    AssociationId: associationId,
  });
  await ec2Client.send(command);
};

const releaseAddress = async (allocationId) => {
  const command = new ReleaseAddressCommand({
    AllocationId: allocationId,
  });

  try {
    await ec2Client.send(command);
    console.log(`🧹 Address with allocation ID ${allocationId} released.\n`);
  } catch (err) {
    console.log(err);
  }
};

const restartInstance = async (instanceId) => {
  console.log("Stopping instance.");
  await stopInstance(instanceId);
  console.log("Instance stopped.");
  console.log("Starting instance.");
  const { PublicIpAddress } = await startInstance(instanceId);
  return PublicIpAddress;
};

const terminateInstance = async (instanceId) => {
  const command = new TerminateInstancesCommand({
    InstanceIds: [instanceId],
  });

  try {
    await ec2Client.send(command);
    await waitUntilInstanceTerminated(
      { client: ec2Client },
      { InstanceIds: [instanceId] }
    );
    console.log(`🧹 Instance with ID ${instanceId} terminated.\n`);
  } catch (err) {
    console.error(err);
  }
};

const deleteSecurityGroup = async (securityGroupId) => {
  const command = new DeleteSecurityGroupCommand({
    GroupId: securityGroupId,
  });

  try {
    await ec2Client.send(command);
    console.log(`🧹 Security group ${securityGroupId} deleted.\n`);
  } catch (err) {
    console.error(err);
  }
};

const deleteKeyPair = async (keyPairName) => {
  const command = new DeleteKeyPairCommand({
    KeyName: keyPairName,
  });

  try {
    await ec2Client.send(command);
    console.log(`🧹 Key pair ${keyPairName} deleted.\n`);
  } catch (err) {
    console.error(err);
  }
};

const deleteTemporaryDirectory = () => {
  try {
    rmSync(tmpDirectory, { recursive: true });
    console.log(`🧹 Temporary directory ${tmpDirectory} deleted.\n`);
  } catch (err) {
    console.error(err);
  }
};

export const main = async () => {
  const keyPairName = "ec2-scenario-key-pair";
  const securityGroupName = "ec2-scenario-security-group";

  let securityGroupId, ipAllocationId, publicIp, instanceId, associationId;

  console.log(wrapText("Welcome to the Amazon EC2 basic usage scenario."));

  try {
    // Prerequisites
    console.log(
      "Before you launch an instance, you'll need a few things:",
      "\n - A Key Pair",
      "\n - A Security Group",
      "\n - An IP Address",
      "\n - An AMI",
      "\n - A compatible instance type",
      "\n\n I'll go ahead and take care of the first three, but I'll need your help for the rest."
    );

    await promptToContinue();

    await createKeyPair(keyPairName);
    securityGroupId = await createSecurityGroup(securityGroupName);
    const { PublicIp, AllocationId } = await allocateIpAddress();
    ipAllocationId = AllocationId;
    publicIp = PublicIp;
    const ipAddress = await authorizeSecurityGroupIngress(securityGroupId);

    const { KeyName } = await describeKeyPair(keyPairName);
    const { GroupName } = await describeSecurityGroup(securityGroupName);
    console.log(`✅ created the key pair ${KeyName}.\n`);
    console.log(
      `✅ created the security group ${GroupName}`,
      `and allowed SSH access from ${ipAddress} (your IP).\n`
    );
    console.log(`✅ allocated ${publicIp} to be used for your EC2 instance.\n`);

    await promptToContinue();

    // Creating the instance
    console.log(wrapText("Create the instance."));
    console.log(
      "You get to choose which image you want. Select an amazon-linux-2 image from the following:"
    );
    const imageDetails = await getAmznLinux2AMIs();
    const instanceTypeDetails = await getCompatibleInstanceTypes(imageDetails);
    console.log("Creating your instance. This can take a few seconds.");
    instanceId = await runInstance({
      keyPairName,
      securityGroupId,
      imageId: imageDetails.ImageId,
      instanceType: instanceTypeDetails.InstanceType,
    });
    const instanceDetails = await describeInstance(instanceId);
    console.log(`✅ instance ${instanceId}.\n`);
    console.log(instanceDetails);
    console.log(
      `\nYou should now be able to SSH into your instance from another terminal:`,
      `\n${displaySSHConnectionInfo({
        publicIp: instanceDetails.PublicIpAddress,
        keyPairName,
      })}`
    );

    await promptToContinue();

    // Understanding the IP address.
    console.log(wrapText("Understanding the IP address."));
    console.log(
      "When you stop and start an instance, the IP address will change. I'll restart your",
      "instance for you. Notice how the IP address changes."
    );
    const ipAddressAfterRestart = await restartInstance(instanceId);
    console.log(
      `\n Instance started. The IP address changed from ${instanceDetails.PublicIpAddress} to ${ipAddressAfterRestart}`,
      `\n${displaySSHConnectionInfo({
        publicIp: ipAddressAfterRestart,
        keyPairName,
      })}`
    );
    await promptToContinue();
    console.log(
      `If you want to the IP address to be static, you can associate an allocated`,
      `IP address to your instance. I allocated ${publicIp} for you earlier, and now I'll associate it to your instance.`
    );
    associationId = await associateAddress({
      allocationId: ipAllocationId,
      instanceId,
    });
    console.log(
      "Done. Now you should be able to SSH using the new IP.\n",
      `${displaySSHConnectionInfo({ publicIp, keyPairName })}`
    );
    await promptToContinue();
    console.log(
      "I'll restart the server again so you can see the IP address remains the same."
    );
    const ipAddressAfterAssociated = await restartInstance(instanceId);
    console.log(
      `Done. Here's your SSH info. Notice the IP address hasn't changed.`,
      `\n${displaySSHConnectionInfo({
        publicIp: ipAddressAfterAssociated,
        keyPairName,
      })}`
    );
    await promptToContinue();
  } catch (err) {
    console.error(err);
  } finally {
    // Clean up.
    console.log(wrapText("Clean up."));
    console.log("Now I'll clean up all of the stuff I created.");
    await promptToContinue();
    console.log("Cleaning up. Some of these steps can take a bit of time.");
    await disassociateAddress(associationId);
    await terminateInstance(instanceId);
    await releaseAddress(ipAllocationId);
    await deleteSecurityGroup(securityGroupId);
    deleteTemporaryDirectory();
    await deleteKeyPair(keyPairName);
    console.log(
      "Done cleaning up. Thanks for staying until the end!",
      "If you have any feedback please use the feedback button in the docs",
      "or create an issue on GitHub."
    );
  }
};
```
+ For API details, see the following topics in *AWS SDK for JavaScript API Reference*\.
  + [AllocateAddress](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/allocateaddresscommand.html)
  + [AssociateAddress](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/associateaddresscommand.html)
  + [AuthorizeSecurityGroupIngress](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/authorizesecuritygroupingresscommand.html)
  + [CreateKeyPair](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/createkeypaircommand.html)
  + [CreateSecurityGroup](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/createsecuritygroupcommand.html)
  + [DeleteKeyPair](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/deletekeypaircommand.html)
  + [DeleteSecurityGroup](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/deletesecuritygroupcommand.html)
  + [DescribeImages](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describeimagescommand.html)
  + [DescribeInstanceTypes](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describeinstancetypescommand.html)
  + [DescribeInstances](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describeinstancescommand.html)
  + [DescribeKeyPairs](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describekeypairscommand.html)
  + [DescribeSecurityGroups](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describesecuritygroupscommand.html)
  + [DisassociateAddress](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/disassociateaddresscommand.html)
  + [ReleaseAddress](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/releaseaddresscommand.html)
  + [RunInstances](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/runinstancescommand.html)
  + [StartInstances](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/startinstancescommand.html)
  + [StopInstances](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/stopinstancescommand.html)
  + [TerminateInstances](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/terminateinstancescommand.html)
  + [UnmonitorInstances](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/unmonitorinstancescommand.html)