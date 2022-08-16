--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Managing Amazon EC2 instances<a name="ec2-example-managing-instances"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve basic information about your Amazon EC2 instances\.
+ How to start and stop detailed monitoring of an Amazon EC2 instance\.
+ How to start and stop an Amazon EC2 instance\.
+ How to reboot an Amazon EC2 instance\.

## The scenario<a name="ec2-example-managing-instances-scenario"></a>

In this example, you use a series of Node\.js modules to perform several basic instance management operations\. The Node\.js modules use the SDK for JavaScript to manage instances by using these Amazon EC2 client class methods:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describeinstancescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/describeinstancescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/monitorinstancescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/monitorinstancescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/unmonitorinstancescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/unmonitorinstancescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/startinstancescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/startinstancescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/stopinstancescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/stopinstancescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/rebootinstancescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ec2/classes/rebootinstancescommand.html)

For more information about the lifecycle of Amazon EC2 instances, see [Instance lifecycle](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-lifecycle.html) in the *Amazon EC2 User Guide for Linux Instances*\.

## Prerequisite tasks<a name="ec2-example-managing-instances-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create an Amazon EC2 instance\. For more information about creating Amazon EC2 instances, see [Amazon EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Instances.html) in the *Amazon EC2 User Guide for Linux Instances* or [Amazon EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/Instances.html) in the *Amazon EC2 User Guide for Windows Instances*\.

**Important**  
These examples use ECMAScript6 \(ES6\)\. This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.  
However, if you prefer to use CommonJS syntax, please refer to [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)

## Describing your instances<a name="ec2-example-managing-instances-describing"></a>

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

Create a Node\.js module with the file name `ec2_describeinstances.js`\. Be sure to configure the SDK as previously shown\. Call the `DescribeInstancesCommand` method of the Amazon EC2 service object to retrieve a detailed description of your instances\. 

```
// Import required AWS SDK clients and commands for Node.js
import { DescribeInstancesCommand } from "@aws-sdk/client-ec2";
import { ec2Client } from "./libs/ec2Client";
const run = async () => {
  try {
    const data = await ec2Client.send(new DescribeInstancesCommand({}));
    console.log("Success", JSON.stringify(data));
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ec2_describeinstances.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/ec2_describeinstances.js)\.

## Managing instance monitoring<a name="ec2-example-managing-instances-monitoring"></a>

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

Create a Node\.js module with the file name `ec2_monitorinstances.js`\. Be sure to configure the SDK as previously shown\. Add the instance IDs of the instances for which you want to control monitoring\.

Based on the value of a command\-line argument \(`ON` or `OFF`\), call either the `MonitorInstancesCommand` method of the Amazon EC2 service object to begin detailed monitoring of the specified instances or call the `UnmonitorInstancesCommand` method\. 

**Note**  
Replace *INSTANCE\_ID* with the IDs of the instances for which you want to control monitoring, and *STATE* with either `ON` or `OFF`\.

```
// Import required AWS SDK clients and commands for Node.js
import {
  MonitorInstancesCommand,
  UnmonitorInstancesCommand,
} from "@aws-sdk/client-ec2";
import { ec2Client } from "./libs/ec2Client";

// Set the parameters
const params = { InstanceIds: ["INSTANCE_ID"] }; // Array of INSTANCE_IDs
const state = "STATE"; // STATE; i.e., 'ON' or 'OFF'

const run = async () => {
  if (process.argv[4].toUpperCase() === "ON") {
    try {
      const data = await ec2Client.send(new MonitorInstancesCommand(params));
      console.log("Success", data.InstanceMonitorings);
      return data;
    } catch (err) {
      console.log("Error", err);
    }
  } else if (process.argv[4].toUpperCase() === "OFF") {
    try {
      const data = await ec2Client.send(new UnmonitorInstancesCommand(params));
      console.log("Success", data.InstanceMonitorings);
      return data;
    } catch (err) {
      console.log("Error", err);
    }
  }
};
run();
```

To run the example, enter the following at the command prompt, specifying `ON` to begin detailed monitoring or `OFF` to discontinue monitoring\.

```
node ec2_monitorinstances.js ON
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/ec2_monitorinstances.js)\.

## Starting and stopping instances<a name="ec2-example-managing-instances-starting-stopping"></a>

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

Create a Node\.js module with the file name `ec2_startstopinstances.js`\. Be sure to configure the SDK as previously shown\. Add the instance IDs of the instances you want to start or stop\.

Based on the value of a command\-line argument \(`START` or `STOP`\), call either the `StartInstancesCommand` method of the Amazon EC2 service object to start the specified instances, or the `StopInstancesCommand` method to stop them\. 

**Note**  
Replace *INSTANCE\_ID* with the instance IDs of the instances you want to start or stop, and *STATE* with `START` or `STOP`\.

```
// Import required AWS SDK clients and commands for Node.js.
import {
  StartInstancesCommand,
  StopInstancesCommand,
} from "@aws-sdk/client-ec2";
import { ec2Client } from "./libs/ec2Client";

// Set the parameters
const params = { InstanceIds: ["INSTANCE_ID"] }; // Array of INSTANCE_IDs
const command = "STATE"; // STATE i.e. "START" or "STOP"

const run = async () => {
  if (command.toUpperCase() === "START") {
    try {
      const data = await ec2Client.send(new StartInstancesCommand(params));
      console.log("Success", data.StartingInstances);
      return data;
    } catch (err) {
      console.log("Error2", err);
    }
  } else if (process.argv[2].toUpperCase() === "STOP") {
    try {
      const data = await ec2Client.send(new StopInstancesCommand(params));
      console.log("Success", data.StoppingInstances);
      return data;
    } catch (err) {
      console.log("Error", err);
    }
  }
};
run();
```

To run the example, enter the following at the command prompt specifying `START` to start the instances or `STOP` to stop them\.

```
node ec2_startstopinstances.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/ec2_startstopinstances.js)\.

## Rebooting instances<a name="ec2-example-managing-instances-rebooting"></a>

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

Create a Node\.js module with the file name `ec2_rebootinstances.js`\. Be sure to configure the SDK as previously shown\. Add the instance IDs of the instances you want to reboot\. Call the `RebootInstancesCommand` method of the `EC2` client service object to reboot the specified instances\. 

**Note**  
Replace *INSTANCE\_ID* with the IDs of the instance you want to reboot\.

```
// Import required AWS SDK clients and commands for Node.js
import { RebootInstancesCommand } from "@aws-sdk/client-ec2";
import { ec2Client } from "./libs/ec2Client.js";

// Set the parameters
const params = { InstanceIds: ["INSTANCE_ID"] }; // Array of INSTANCE_IDs

const run = async () => {
  try {
    const data = await ec2Client.send(new RebootInstancesCommand(params));
    console.log("Success", data.InstanceMonitorings);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ec2_rebootinstances.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/ec2_rebootinstances.js)\.