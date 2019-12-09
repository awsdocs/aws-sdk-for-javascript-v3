# Using Regions and Availability Zones with Amazon EC2<a name="ec2-example-regions-availability-zones"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve descriptions for AWS Regions and Availability Zones\.

## The Scenario<a name="ec2-example-regions-availability-zones-scenario"></a>

Amazon EC2 is hosted in multiple locations worldwide\. These locations are composed of Regions and Availability Zones\. Each Region is a separate geographic area\. Each Region has multiple, isolated locations known as *Availability Zones*\. Amazon EC2 provides the ability to place instances and data in multiple locations\. 

In this example, you use a series of Node\.js modules to retrieve details about Regions and Availability Zones\. The Node\.js modules use the SDK for JavaScript to manage instances by using the following methods of the Amazon EC2 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeAvailabilityZones-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeAvailabilityZones-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeRegions-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeRegions-property)

For more information about Regions and Availability Zones, see [Regions and Availability Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html) in the *Amazon EC2 User Guide for Linux Instances* or [Regions and Availability Zones](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/using-regions-availability-zones.html) in the *Amazon EC2 User Guide for Windows Instances*\.

## Prerequisite Tasks<a name="ec2-example-regions-availability-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Describing Regions and Availability Zones<a name="ec2-example-regions-availability-describing"></a>

Create a Node\.js module with the file name `ec2_describeregionsandzones.js`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `AWS.EC2` service object\. Create an empty JSON object to pass as parameters, which returns all available descriptions\. Then call the `describeRegions` and `describeAvailabilityZones` methods\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create EC2 service object
var ec2 = new AWS.EC2({apiVersion: '2016-11-15'});

var params = {};

// Retrieves all regions/endpoints that work with EC2
ec2.describeRegions(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Regions: ", data.Regions);
  }
});

// Retrieves availability zones only for region of the ec2 service object
ec2.describeAvailabilityZones(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Availability Zones: ", data.AvailabilityZones);
  }
});
```

To run the example, type the following at the command line\.

```
node ec2_describeregionsandzones.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ec2/ec2_describeregionsandzones.js)\.