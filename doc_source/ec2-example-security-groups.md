--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Working with security groups in Amazon EC2<a name="ec2-example-security-groups"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve information about your security groups\.
+ How to create a security group to access an Amazon EC2 instance\.
+ How to delete an existing security group\.

## The scenario<a name="ec2-example-security-groups-scenario"></a>

An Amazon EC2 security group acts as a virtual firewall that controls the traffic for one or more instances\. You add rules to each security group to allow traffic to or from its associated instances\. You can modify the rules for a security group at any time; the new rules are automatically applied to all instances that are associated with the security group\.

In this example, you use a series of Node\.js modules to perform several Amazon EC2 operations involving security groups\. The Node\.js modules use the SDK for JavaScript to manage instances by using the following methods of the Amazon EC2 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describesecuritygroupscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describesecuritygroupscommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/authorizesecuritygroupingresscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/authorizesecuritygroupingresscommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/createsecuritygroupcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/createsecuritygroupcommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describevpcscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describevpcscommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/deletesecuritygroupcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/deletesecuritygroupcommand.html)

For more information about the Amazon EC2 security groups, see [Amazon EC2 Amazon securitiy groups for Linux nstances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html) in the *Amazon EC2 User Guide for Linux Instances* or [Amazon EC2 Security groups for Windows instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/using-network-security.html) in the *Amazon EC2 User Guide for Windows Instances*\.

## Prerequisite tasks<a name="ec2-example-security-groups-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples use ECMAScript6 \(ES6\)\. This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.  
However, if you prefer to use CommonJS sytax, please refer to [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)

## Describing your security groups<a name="ec2-example-security-groups-describing"></a>

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

Create a Node\.js module with the file name `ec2_describesecuritygroups.js`\. Be sure to configure the SDK as previously shown\. Create a JSON object to pass as parameters, including the group IDs for the security groups you want to describe\. Then call the `DescribeSecurityGroupsCommand` method of the Amazon EC2 service object\.

**Note**  
Replace *SECURITY\_GROUP\_ID* with the group IDs for the security groups you want to describe\.

```
// Import required AWS SDK clients and commands for Node.js
import { DescribeSecurityGroupsCommand } from "@aws-sdk/client-ec2";
import { ec2Client } from "./libs/ec2Client";

// Set the parameters
const params = { GroupIds: ["SECURITY_GROUP_ID"] }; //SECURITY_GROUP_ID

const run = async () => {
  try {
    const data = await ec2Client.send(
      new DescribeSecurityGroupsCommand(params)
    );
    console.log("Success", JSON.stringify(data.SecurityGroups));
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ec2_describesecuritygroups.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/ec2_describesecuritygroups.js)\.

## Creating a security group and rules<a name="ec2-example-security-groups-creating"></a>

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

Create a Node\.js module with the file name `ec2_createsecuritygroup.js`\. Be sure to configure the SDK as previously shown\. Create a JSON object for the parameters that specify the name of the security group, a description, and the ID for the VPC\. Pass the parameters to the `CreateSecurityGroupCommand` method\.

`curityGroupCommand` method\.

After you successfully create the security group, you can define rules for allowing inbound traffic\. Create a JSON object for parameters that specify the IP protocol and inbound ports on which the Amazon EC2 instance will receive traffic\. Pass the parameters to the `AuthorizeSecurityGroupIngressCommand` method\.

**Note**  
Replace *KEY\_PAIR\_NAME* with the name of the key pair, *DESCRIPTION* with a description of the security group, *SECURITY\_GROUP\_NAME* with the name of the security group, and *SECURITY\_GROUP\_ID* with the ID of the security group\.

```
// Import required AWS SDK clients and commands for Node.js
import {
  DescribeVpcsCommand,
  CreateSecurityGroupCommand,
  AuthorizeSecurityGroupIngressCommand,
} from "@aws-sdk/client-ec2";
import { ec2Client } from "./libs/ec2Client";

// Set the parameters
const params = { KeyName: "KEY_PAIR_NAME" }; //KEY_PAIR_NAME

// Variable to hold a ID of a VPC
const vpc = null;

const run = async () => {
  try {
    const data = await ec2Client.send(new DescribeVpcsCommand(params));
    return data;
    const vpc = data.Vpcs[0].VpcId;
    const paramsSecurityGroup = {
      Description: "DESCRIPTION", //DESCRIPTION
      GroupName: "SECURITY_GROUP_NAME", // SECURITY_GROUP_NAME
      VpcId: vpc,
    };
  } catch (err) {
    console.log("Error", err);
  }
  try {
    const data = await ec2Client.send(new CreateSecurityGroupCommand(params));
    const SecurityGroupId = data.GroupId;
    console.log("Success", SecurityGroupId);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
  try {
    const paramsIngress = {
      GroupId: "SECURITY_GROUP_ID", //SECURITY_GROUP_ID
      IpPermissions: [
        {
          IpProtocol: "tcp",
          FromPort: 80,
          ToPort: 80,
          IpRanges: [{ CidrIp: "0.0.0.0/0" }],
        },
        {
          IpProtocol: "tcp",
          FromPort: 22,
          ToPort: 22,
          IpRanges: [{ CidrIp: "0.0.0.0/0" }],
        },
      ],
    };
    const data = await ec2Client.send(
      new AuthorizeSecurityGroupIngressCommand(paramsIngress)
    );
    console.log("Ingress Successfully Set", data);
    return data;
  } catch (err) {
    console.log("Cannot retrieve a VPC", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ec2_createsecuritygroup.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/ec2_createsecuritygroup.js)\.

## Deleting a security group<a name="ec2-example-security-groups-deleting"></a>

Create a Node\.js module with the file name `ec2_deletesecuritygroup.js`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `EC2` client service object\. Create the JSON parameters to specify the name of the security group to delete\. Then call the `DeleteSecurityGroupCommand` method\.

**Note**  
Replace *SECURITY\_GROUP\_ID* with the security group ID\.

```
// Import required AWS SDK clients and commands for Node.js
import { DeleteSecurityGroupCommand } from "@aws-sdk/client-ec2";
import { ec2Client } from "./libs/ec2Client";
// Set the parameters
const params = { GroupId: "SECURITY_GROUP_ID" }; //SECURITY_GROUP_ID

const run = async () => {
  try {
    const data = await ec2Client.send(new DeleteSecurityGroupCommand(params));
    console.log("Security Group Deleted");
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ec2_deletesecuritygroup.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/ec2_deletesecuritygroup.js)\.