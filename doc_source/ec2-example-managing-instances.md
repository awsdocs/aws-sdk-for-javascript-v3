--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

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
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeInstances-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeInstances-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#monitorInstances-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#monitorInstances-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#unmonitorInstances-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#unmonitorInstances-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#startInstances-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#startInstances-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#stopInstances-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#stopInstances-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#rebootInstances-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#rebootInstances-property)

For more information about the lifecycle of Amazon EC2 instances, see [Instance lifecycle](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-lifecycle.html) in the *Amazon EC2 User Guide for Linux Instances*\.

## Prerequisite tasks<a name="ec2-example-managing-instances-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/ec2/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create an Amazon EC2 instance\. For more information about creating Amazon EC2 instances, see [Amazon EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Instances.html) in the *Amazon EC2 User Guide for Linux Instances* or [Amazon EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/Instances.html) in the *Amazon EC2 User Guide for Windows Instances*\.

## Describing your instances<a name="ec2-example-managing-instances-describing"></a>

Create a Node\.js module with the file name `ec2_describeinstances.ts`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `EC2` client service object\. Call the `DescribeInstancesCommand` method of the Amazon EC2 service object to retrieve a detailed description of your instances\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region\.

```
// Import required AWS SDK clients and commands for Node.js
const { EC2Client, DescribeInstancesCommand } = require("@aws-sdk/client-ec2");

// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"

// Create EC2 service object
const ec2client = new EC2Client(REGION);

const run = async () => {
  try {
    const data = await ec2client.send(new DescribeInstancesCommand({}));
    console.log("Success", JSON.stringify(data));
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ec2_describeinstances.ts // If you prefer JavaScript, enter 'node ec2_describeinstances.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/src/ec2_describeinstances.ts)\.

## Managing instance monitoring<a name="ec2-example-managing-instances-monitoring"></a>

Create a Node\.js module with the file name `ec2_monitorinstances.ts`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `EC2` client service object\. Add the instance IDs of the instances for which you want to control monitoring\.

Based on the value of a command\-line argument \(`ON` or `OFF`\), call either the `MonitorInstancesCommand` method of the Amazon EC2 service object to begin detailed monitoring of the specified instances or call the `UnmonitorInstancesCommand` method\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, the *INSTANCE\_ID* with the IDs of the instances for which you want to control monitoring, and *STATE* with either `ON` or `OFF`\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  EC2Client,
  MonitorInstancesCommand,
  UnmonitorInstancesCommand,
} = require("@aws-sdk/client-ec2");

// Set the AWS region
const REGION = "region"; //e.g. "us-east-1"

// Create EC2 service object
const ec2client = new EC2Client(REGION);

// Set the parameters
const params = { InstanceIds: "INSTANCE_ID" }; // INSTANCE_ID
const state = "STATE"; // STATE; i.e., 'ON' or 'OFF'

const run = async () => {
  if (process.argv[4].toUpperCase() === "ON") {
    try {
      const data = await ec2client.send(new MonitorInstancesCommand(params));
      console.log("Success", data.InstanceMonitorings);
    } catch (err) {
      console.log("Error", err);
    }
  } else if (process.argv[4].toUpperCase() === "OFF") {
    try {
      const data = await ec2client.send(new UnmonitorInstancesCommand(params));
      console.log("Success", data.InstanceMonitorings);
    } catch (err) {
      console.log("Error", err);
    }
  }
};
run();
```

To run the example, enter the following at the command prompt, specifying `ON` to begin detailed monitoring or `OFF` to discontinue monitoring\.

```
ts-node ec2_monitorinstances.ts ON
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/src/ec2_monitorinstances.ts)\.

## Starting and stopping instances<a name="ec2-example-managing-instances-starting-stopping"></a>

Create a Node\.js module with the file name `ec2_startstopinstances.ts`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `EC2` client service object\. Add the instance IDs of the instances you want to start or stop\.

Based on the value of a command\-line argument \(`START` or `STOP`\), call either the `StartInstancesCommand` method of the Amazon EC2 service object to start the specified instances, or the `StopInstancesCommand` method to stop them\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *INSTANCE\_ID* with the instance IDs of the instances you want to start or stop, and *STATE* with `START` or `STOP`\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  EC2Client,
  StartInstancesCommand,
  StopInstancesCommand
} = require("@aws-sdk/client-ec2");

// Set the AWS region
const REGION = "REGION"; //e.g. "us-east-1"

// Create EC2 service object
const ec2client = new EC2Client(REGION);

// Set the parameters
const params = { InstanceIds: "INSTANCE_ID" }; //INSTANCE_ID
const command = "STATE"; // STATE i.e. "START" or "STOP"

const run = async () => {
  if (command.toUpperCase() === "START") {
    try {
      const data = await ec2client.send(new StartInstancesCommand(params));
      console.log("Success", data.StartingInstances);
    } catch (err) {
      console.log("Error2", err);
    }
  } else if (process.argv[2].toUpperCase() === "STOP") {
    try {
      const data = await ec2client.send(new StopInstancesCommand(params));
      console.log("Success", data.StoppingInstances);
    } catch (err) {
      console.log("Error", err);
    }
  }
};
run();
```

To run the example, enter the following at the command prompt specifying `START` to start the instances or `STOP` to stop them\.

```
ts-node ec2_startstopinstances.ts
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/src/ec2_startstopinstances.ts)\.

## Rebooting instances<a name="ec2-example-managing-instances-rebooting"></a>

Create a Node\.js module with the file name `ec2_rebootinstances.ts`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an Amazon EC2 service object\. Add the instance IDs of the instances you want to reboot\. Call the `RebootInstancesCommand` method of the `EC2` client service object to reboot the specified instances\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *INSTANCE\_ID* with the IDs of the instance you want to reboot\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  EC2Client,
  RebootInstancesCommandInput,
} = require("@aws-sdk/client-ec2");

// Set the AWS region
const REGION = "region"; //e.g. "us-east-1"

// Create EC2 service object
const ec2client = new EC2Client(REGION);

// Set the parameters
const params = { InstanceIds: "INSTANCE_ID" }; //INSTANCE_ID

const run = async () => {
  try {
    const data = await ec2client.send(new RebootInstancesCommandInput(params));
    console.log("Success", data.InstanceMonitorings);
  } catch (err) {
    console.log("Error", err);
  }
};
```

To run the example, enter the following at the command prompt\.

```
ts-node ec2_rebootinstances.ts // If you prefer JavaScript, enter 'node ec2_rebootinstances.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ec2/src/src/ec2_rebootinstances.ts)\.