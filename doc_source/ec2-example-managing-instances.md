# Managing Amazon EC2 Instances<a name="ec2-example-managing-instances"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve basic information about your Amazon EC2 instances\.
+ How to start and stop detailed monitoring of an Amazon EC2 instance\.
+ How to start and stop an Amazon EC2 instance\.
+ How to reboot an Amazon EC2 instance\.

## The Scenario<a name="ec2-example-managing-instances-scenario"></a>

In this example, you use a series of Node\.js modules to perform several basic instance management operations\. The Node\.js modules use the SDK for JavaScript to manage instances by using these Amazon EC2 client class methods:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeInstances-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#describeInstances-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#monitorInstances-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#monitorInstances-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#unmonitorInstances-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#unmonitorInstances-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#startInstances-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#startInstances-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#stopInstances-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#stopInstances-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#rebootInstances-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#rebootInstances-property)

For more information about the lifecycle of Amazon EC2 instances, see [Instance Lifecycle](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-lifecycle.html) in the *Amazon EC2 User Guide for Linux Instances*\.

## Prerequisite Tasks<a name="ec2-example-managing-instances-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.
+ Create an Amazon EC2 instance\. For more information about creating Amazon EC2 instances, see [Amazon EC2 Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Instances.html) in the *Amazon EC2 User Guide for Linux Instances* or [Amazon EC2 Instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/Instances.html) in the *Amazon EC2 User Guide for Windows Instances*\.

## Describing Your Instances<a name="ec2-example-managing-instances-describing"></a>

Create a Node\.js module with the file name `ec2_describeinstances.js`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `AWS.EC2` service object\. Call the `describeInstances` method of the Amazon EC2 service object to retrieve a detailed description of your instances\. 

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create EC2 service object
var ec2 = new AWS.EC2({apiVersion: '2016-11-15'});

var params = {
  DryRun: false
};

// Call EC2 to retrieve policy for selected bucket
ec2.describeInstances(params, function(err, data) {
  if (err) {
    console.log("Error", err.stack);
  } else {
    console.log("Success", JSON.stringify(data));
  }
});
```

To run the example, type the following at the command line\.

```
node ec2_describeinstances.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ec2/ec2_describeinstances.js)\.

## Managing Instance Monitoring<a name="ec2-example-managing-instances-monitoring"></a>

Create a Node\.js module with the file name `ec2_monitorinstances.js`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `AWS.EC2` service object\. Add the instance IDs of the instances for which you want to control monitoring\.

Based on the value of a command\-line argument \(`ON` or `OFF`\), call either the `monitorInstances` method of the Amazon EC2 service object to begin detailed monitoring of the specified instances or call the `unmonitorInstances` method\. Use the `DryRun` parameter to test whether you have permission to change instance monitoring before you attempt to change the monitoring of these instances\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create EC2 service object
var ec2 = new AWS.EC2({apiVersion: '2016-11-15'});

var params = {
  InstanceIds: ['INSTANCE_ID'],
  DryRun: true
};

if (process.argv[2].toUpperCase() === "ON") {
  // Call EC2 to start monitoring the selected instances
  ec2.monitorInstances(params, function(err, data) {
    if (err && err.code === 'DryRunOperation') {
      params.DryRun = false;
      ec2.monitorInstances(params, function(err, data) {
          if (err) {
            console.log("Error", err);
          } else if (data) {
            console.log("Success", data.InstanceMonitorings);
          }
      });
    } else {
      console.log("You don't have permission to change instance monitoring.");
    }
  });
} else if (process.argv[2].toUpperCase() === "OFF") {
  // Call EC2 to stop monitoring the selected instances
  ec2.unmonitorInstances(params, function(err, data) {
    if (err && err.code === 'DryRunOperation') {
      params.DryRun = false;
      ec2.unmonitorInstances(params, function(err, data) {
          if (err) {
            console.log("Error", err);
          } else if (data) {
            console.log("Success", data.InstanceMonitorings);
          }
      });
    } else {
      console.log("You don't have permission to change instance monitoring.");
    }
  });
}
```

To run the example, type the following at the command line, specifying `ON` to begin detailed monitoring or `OFF` to discontinue monitoring\.

```
node ec2_monitorinstances.js ON
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ec2/ec2_monitorinstances.js)\.

## Starting and Stopping Instances<a name="ec2-example-managing-instances-starting-stopping"></a>

Create a Node\.js module with the file name `ec2_startstopinstances.js`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an `AWS.EC2` service object\. Add the instance IDs of the instances you want to start or stop\.

Based on the value of a command\-line argument \(`START` or `STOP`\), call either the `startInstances` method of the Amazon EC2 service object to start the specified instances, or the `stopInstances` method to stop them\. Use the `DryRun` parameter to test whether you have permission before actually attempting to start or stop the selected instances\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create EC2 service object
var ec2 = new AWS.EC2({apiVersion: '2016-11-15'});

var params = {
  InstanceIds: [process.argv[3]],
  DryRun: true
};

if (process.argv[2].toUpperCase() === "START") {
  // Call EC2 to start the selected instances
  ec2.startInstances(params, function(err, data) {
    if (err && err.code === 'DryRunOperation') {
      params.DryRun = false;
      ec2.startInstances(params, function(err, data) {
          if (err) {
            console.log("Error", err);
          } else if (data) {
            console.log("Success", data.StartingInstances);
          }
      });
    } else {
      console.log("You don't have permission to start instances.");
    }
  });
} else if (process.argv[2].toUpperCase() === "STOP") {
  // Call EC2 to stop the selected instances
  ec2.stopInstances(params, function(err, data) {
    if (err && err.code === 'DryRunOperation') {
      params.DryRun = false;
      ec2.stopInstances(params, function(err, data) {
          if (err) {
            console.log("Error", err);
          } else if (data) {
            console.log("Success", data.StoppingInstances);
          }
      });
    } else {
      console.log("You don't have permission to stop instances");
    }
  });
}
```

To run the example, type the following at the command line specifying `START` to start the instances or `STOP` to stop them\.

```
node ec2_startstopinstances.js START INSTANCE_ID
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ec2/ec2_startstopinstances.js)\.

## Rebooting Instances<a name="ec2-example-managing-instances-rebooting"></a>

Create a Node\.js module with the file name `ec2_rebootinstances.js`\. Be sure to configure the SDK as previously shown\. To access Amazon EC2, create an Amazon EC2 service object\. Add the instance IDs of the instances you want to reboot\. Call the `rebootInstances` method of the `AWS.EC2` service object to reboot the specified instances\. Use the `DryRun` parameter to test whether you have permission to reboot these instances before actually attempting to reboot them\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create EC2 service object
var ec2 = new AWS.EC2({apiVersion: '2016-11-15'});

var params = {
  InstanceIds: ['INSTANCE_ID'],
  DryRun: true
};

// Call EC2 to reboot instances
ec2.rebootInstances(params, function(err, data) {
  if (err && err.code === 'DryRunOperation') {
    params.DryRun = false;
    ec2.rebootInstances(params, function(err, data) {
        if (err) {
          console.log("Error", err);
        } else if (data) {
          console.log("Success", data);
        }
    });
  } else {
    console.log("You don't have permission to reboot instances.");
  }
});
```

To run the example, type the following at the command line\.

```
node ec2_rebootinstances.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ec2/ec2_rebootinstances.js)\.