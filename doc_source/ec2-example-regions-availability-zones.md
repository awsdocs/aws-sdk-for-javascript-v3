--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Using Regions and Availability Zones with Amazon EC2<a name="ec2-example-regions-availability-zones"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve descriptions for AWS Regions and Availability Zones\.

## The scenario<a name="ec2-example-regions-availability-zones-scenario"></a>

Amazon EC2 is hosted in multiple locations worldwide\. These locations are composed of Regions and Availability Zones\. Each Region is a separate geographic area\. Each Region has multiple, isolated locations known as *Availability Zones*\. Amazon EC2 provides the ability to place instances and data in multiple locations\. 

In this example, you use a series of Node\.js modules to retrieve details about Regions and Availability Zones\. The Node\.js modules use the SDK for JavaScript to manage instances by using the following methods of the Amazon EC2 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describekeypairscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describekeypairscommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describeregionscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describeregionscommand.html)

For more information about Regions and Availability Zones, see [Regions and Availability Zones](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html) in the *Amazon EC2 User Guide for Linux Instances* or [Regions and Availability Zones](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/using-regions-availability-zones.html) in the *Amazon EC2 User Guide for Windows Instances*\.

## Prerequisite tasks<a name="ec2-example-regions-availability-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples use ECMAScript6 \(ES6\)\. This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.  
However, if you prefer to use CommonJS sytax, please refer to [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)

## Describing Regions and Availability Zones<a name="ec2-example-regions-availability-describing"></a>

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

Create a Node\.js module with the file name `ec2_describeregionsandzones.js`\. Be sure to configure the SDK as previously shown\. Create an empty JSON object to pass as parameters, which returns all available descriptions\. Then call the `DescribeRegionsCommand` and `DescribeAvailabilityZonesCommand` methods\.

```
// Import required AWS SDK clients and commands for Node.js
import { DescribeRegionsCommand } from "@aws-sdk/client-ec2";
import { ec2Client } from "./libs/ec2Client";

const run = async () => {
  try {
    const data = await ec2Client.send(new DescribeRegionsCommand({}));
    console.log("Availability Zones: ", data.Regions);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ec2_describeregionsandzones.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/ec2_describeregionsandzones.js)\.