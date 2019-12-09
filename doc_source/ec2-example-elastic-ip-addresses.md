# Using Elastic IP Addresses in Amazon EC2<a name="ec2-example-elastic-ip-addresses"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve descriptions of your Elastic IP addresses\.
+ How to allocate and release an Elastic IP address\.
+ How to associate an Elastic IP address with an Amazon EC2 instance\.

## The Scenario<a name="ec2-example-elastic-ip-addresses-scenario"></a>

An *Elastic IP address* is a static IP address designed for dynamic cloud computing\. An Elastic IP address is associated with your AWS account\. It is a public IP address, which is reachable from the internet\. If your instance does not have a public IP address, you can associate an Elastic IP address with your instance to enable communication with the internet\.

In this example, you use a series of Node\.js modules to perform several Amazon EC2 operations involving Elastic IP addresses\. The Node\.js modules use the SDK for JavaScript to manage Elastic IP addresses by using these methods of the Amazon EC2 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeAddresses-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeAddresses-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#allocateAddress-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#allocateAddress-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#associateAddress-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#associateAddress-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#releaseAddress-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#releaseAddress-property)

For more information about Elastic IP addresses in Amazon EC2, see [Elastic IP Addresses](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) in the *Amazon EC2 User Guide for Linux Instances* or [Elastic IP Addresses](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/elastic-ip-addresses-eip.html) in the *Amazon EC2 User Guide for Windows Instances*\.

## Prerequisite Tasks<a name="ec2-example-elastic-ip-addresses-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.
+ Create an Amazon EC2 instance\. For more information about creating Amazon EC2 instances, see [Amazon EC2 Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Instances.html) in the *Amazon EC2 User Guide for Linux Instances* or [Amazon EC2 Instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/Instances.html) in the *Amazon EC2 User Guide for Windows Instances*\.

## Describing Elastic IP Addresses<a name="ec2-example-elastic-ip-addresses-describing"></a>

Create a Node\.js module with the file name `ec2_describeaddresses.js`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `AWS.EC2` service object\. Create a JSON object to pass as parameters, filtering the addresses returned by those in your VPC\. To retrieve descriptions of all your Elastic IP addresses, omit a filter from the parameters JSON\. Then call the `describeAddresses` method of the Amazon EC2 service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create EC2 service object
var ec2 = new AWS.EC2({apiVersion: '2016-11-15'});

var params = {
  Filters: [
    {Name: 'domain', Values: ['vpc']}
  ]
};

// Retrieve Elastic IP address descriptions
ec2.describeAddresses(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", JSON.stringify(data.Addresses));
  }
});
```

To run the example, type the following at the command line\.

```
node ec2_describeaddresses.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ec2/ec2_describeaddresses.js)\.

## Allocating and Associating an Elastic IP Address with an Amazon EC2 Instance<a name="ec2-example-elastic-ip-addresses-allocating-associating"></a>

Create a Node\.js module with the file name `ec2_allocateaddress.js`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `AWS.EC2` service object\. Create a JSON object for the parameters used to allocate an Elastic IP address, which in this case specifies the `Domain` is a VPC\. Call the `allocateAddress` method of the Amazon EC2 service object\.

If the call succeeds, the `data` parameter to the callback function has an `AllocationId` property that identifies the allocated Elastic IP address\.

Create a JSON object for the parameters used to associate an Elastic IP address to an Amazon EC2 instance, including the `AllocationId` from the newly allocated address and the `InstanceId` of the Amazon EC2 instance\. Then call the `associateAddresses` method of the Amazon EC2 service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create EC2 service object
var ec2 = new AWS.EC2({apiVersion: '2016-11-15'});

var paramsAllocateAddress = {
   Domain: 'vpc'
};

// Allocate the Elastic IP address
ec2.allocateAddress(paramsAllocateAddress, function(err, data) {
   if (err) {
      console.log("Address Not Allocated", err);
   } else {
      console.log("Address allocated:", data.AllocationId);
      var paramsAssociateAddress = {
        AllocationId: data.AllocationId,
        InstanceId: 'INSTANCE_ID'
      };
      // Associate the new Elastic IP address with an EC2 instance
      ec2.associateAddress(paramsAssociateAddress, function(err, data) {
        if (err) {
          console.log("Address Not Associated", err);
        } else {
          console.log("Address associated:", data.AssociationId);
        }
      });
   }
});
```

To run the example, type the following at the command line\.

```
node ec2_allocateaddress.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ec2/ec2_allocateaddress.js)\.

## Releasing an Elastic IP Address<a name="ec2-example-elastic-ip-addresses-releasing"></a>

Create a Node\.js module with the file name `ec2_releaseaddress.js`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `AWS.EC2` service object\. Create a JSON object for the parameters used to release an Elastic IP address, which in this case specifies the `AllocationId` for the Elastic IP address\. Releasing an Elastic IP address also disassociates it from any Amazon EC2 instance\. Call the `releaseAddress` method of the Amazon EC2 service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create EC2 service object
var ec2 = new AWS.EC2({apiVersion: '2016-11-15'});

var paramsReleaseAddress = {
   AllocationId: 'ALLOCATION_ID'
};

// Disassociate the Elastic IP address from EC2 instance
ec2.releaseAddress(paramsReleaseAddress, function(err, data) {
   if (err) {
      console.log("Error", err);
   } else {
      console.log("Address released");
   }
});
```

To run the example, type the following at the command line\.

```
node ec2_releaseaddress.js
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ec2/ec2_releaseaddress.js)\.