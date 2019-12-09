# Working with Security Groups in Amazon EC2<a name="ec2-example-security-groups"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve information about your security groups\.
+ How to create a security group to access an Amazon EC2 instance\.
+ How to delete an existing security group\.

## The Scenario<a name="ec2-example-security-groups-scenario"></a>

An Amazon EC2 security group acts as a virtual firewall that controls the traffic for one or more instances\. You add rules to each security group to allow traffic to or from its associated instances\. You can modify the rules for a security group at any time; the new rules are automatically applied to all instances that are associated with the security group\.

In this example, you use a series of Node\.js modules to perform several Amazon EC2 operations involving security groups\. The Node\.js modules use the SDK for JavaScript to manage instances by using the following methods of the Amazon EC2 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeSecurityGroups-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeSecurityGroups-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#authorizeSecurityGroupIngress-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#authorizeSecurityGroupIngress-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#createSecurityGroup-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#createSecurityGroup-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeVpcs-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeVpcs-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#deleteSecurityGroup-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#deleteSecurityGroup-property)

For more information about the Amazon EC2 security groups, see [Amazon EC2 Amazon Security Groups for Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html) in the *Amazon EC2 User Guide for Linux Instances* or [Amazon EC2 Security Groups for Windows Instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/using-network-security.html) in the *Amazon EC2 User Guide for Windows Instances*\.

## Prerequisite Tasks<a name="ec2-example-security-groups-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Describing Your Security Groups<a name="ec2-example-security-groups-describing"></a>

Create a Node\.js module with the file name `ec2_describesecuritygroups.js`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `AWS.EC2` service object\. Create a JSON object to pass as parameters, including the group IDs for the security groups you want to describe\. Then call the `describeSecurityGroups` method of the Amazon EC2 service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create EC2 service object
var ec2 = new AWS.EC2({apiVersion: '2016-11-15'});

var params = {
  GroupIds: ['SECURITY_GROUP_ID']
};

// Retrieve security group descriptions
ec2.describeSecurityGroups(params, function(err, data) {
   if (err) {
      console.log("Error", err);
   } else {
      console.log("Success", JSON.stringify(data.SecurityGroups));
   }
});
```

To run the example, type the following at the command line\.

```
node ec2_describesecuritygroups.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ec2/ec2_describesecuritygroups.js)\.

## Creating a Security Group and Rules<a name="ec2-example-security-groups-creating"></a>

Create a Node\.js module with the file name `ec2_createsecuritygroup.js`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `AWS.EC2` service object\. Create a JSON object for the parameters that specify the name of the security group, a description, and the ID for the VPC\. Pass the parameters to the `createSecurityGroup` method\.

After you successfully create the security group, you can define rules for allowing inbound traffic\. Create a JSON object for parameters that specify the IP protocol and inbound ports on which the Amazon EC2 instance will receive traffic\. Pass the parameters to the `authorizeSecurityGroupIngress` method\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Load credentials and set region from JSON file
AWS.config.update({region: 'REGION'});

// Create EC2 service object
var ec2 = new AWS.EC2({apiVersion: '2016-11-15'});

// Variable to hold a ID of a VPC
var vpc = null;

// Retrieve the ID of a VPC
ec2.describeVpcs(function(err, data) {
   if (err) {
     console.log("Cannot retrieve a VPC", err);
   } else {
     vpc = data.Vpcs[0].VpcId;
     var paramsSecurityGroup = {
        Description: 'DESCRIPTION',
        GroupName: 'SECURITY_GROUP_NAME',
        VpcId: vpc
     };
     // Create the instance
     ec2.createSecurityGroup(paramsSecurityGroup, function(err, data) {
        if (err) {
           console.log("Error", err);
        } else {
           var SecurityGroupId = data.GroupId;
           console.log("Success", SecurityGroupId);
           var paramsIngress = {
             GroupId: 'SECURITY_GROUP_ID',
             IpPermissions:[
                {
                   IpProtocol: "tcp",
                   FromPort: 80,
                   ToPort: 80,
                   IpRanges: [{"CidrIp":"0.0.0.0/0"}]
               },
               {
                   IpProtocol: "tcp",
                   FromPort: 22,
                   ToPort: 22,
                   IpRanges: [{"CidrIp":"0.0.0.0/0"}]
               }
             ]
           };
           ec2.authorizeSecurityGroupIngress(paramsIngress, function(err, data) {
             if (err) {
               console.log("Error", err);
             } else {
               console.log("Ingress Successfully Set", data);
             }
          });
        }
     });
   }
});
```

To run the example, type the following at the command line\.

```
node ec2_createsecuritygroup.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ec2/ec2_createsecuritygroup.js)\.

## Deleting a Security Group<a name="ec2-example-security-groups-deleting"></a>

Create a Node\.js module with the file name `ec2_deletesecuritygroup.js`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `AWS.EC2` service object\. Create the JSON parameters to specify the name of the security group to delete\. Then call the `deleteSecurityGroup` method\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create EC2 service object
var ec2 = new AWS.EC2({apiVersion: '2016-11-15'});

var params = {
   GroupId: 'SECURITY_GROUP_ID'
};

// Delete the security group
ec2.deleteSecurityGroup(params, function(err, data) {
   if (err) {
      console.log("Error", err);
   } else {
      console.log("Security Group Deleted");
   }
});
```

To run the example, type the following at the command line\.

```
node ec2_deletesecuritygroup.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ec2/ec2_deletesecuritygroup.js)\.