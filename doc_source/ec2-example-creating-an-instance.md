# Creating an Amazon EC2 Instance<a name="ec2-example-creating-an-instance"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create an Amazon EC2 instance from a public Amazon Machine Image \(AMI\)\.
+ How to create and assign tags to the new Amazon EC2 instance\.

## About the Example<a name="ec2-example-creating-an-instance-scenario"></a>

In this example, you use a Node\.js module to create an Amazon EC2 instance and assign both a key pair and tags to it\. The code uses the SDK for JavaScript to create and tag an instance by using these methods of the Amazon EC2 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#runInstances-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#runInstances-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#createTags-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/EC2.html#createTags-property)

## Prerequisite Tasks<a name="ec2-example-creating-an-instance-prerequisites"></a>

To set up and run this example, first complete these tasks\.
+ Install Node\.js\. For more information, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.
+ Create a key pair\. For details, see [Working with Amazon EC2 Key Pairs](ec2-example-key-pairs.md)\. You use the name of the key pair in this example\.

## Creating and Tagging an Instance<a name="ec2-example-creating-an-instance-and-tags"></a>

Create a Node\.js module with the file name `ec2_createinstances.js`\. Be sure to configure the SDK as previously shown\.

Create an object to pass the parameters for the `runInstances` method of the `AWS.EC2` client class, including the name of the key pair to assign and the ID of the AMI to run\. To call the `runInstances` method, create a promise for invoking an Amazon EC2 service object, passing the parameters\. Then handle the response in the promise callback\. 

The code next adds a `Name` tag to a new instance, which the Amazon EC2 console recognizes and displays in the **Name** field of the instance list\. You can add up to 50 tags to an instance, all of which can be added in a single call to the `createTags` method\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Load credentials and set region from JSON file
AWS.config.update({region: 'REGION'});

// Create EC2 service object
var ec2 = new AWS.EC2({apiVersion: '2016-11-15'});

// AMI is amzn-ami-2011.09.1.x86_64-ebs
var instanceParams = {
   ImageId: 'AMI_ID', 
   InstanceType: 't2.micro',
   KeyName: 'KEY_PAIR_NAME',
   MinCount: 1,
   MaxCount: 1
};

// Create a promise on an EC2 service object
var instancePromise = new AWS.EC2({apiVersion: '2016-11-15'}).runInstances(instanceParams).promise();

// Handle promise's fulfilled/rejected states
instancePromise.then(
  function(data) {
    console.log(data);
    var instanceId = data.Instances[0].InstanceId;
    console.log("Created instance", instanceId);
    // Add tags to the instance
    tagParams = {Resources: [instanceId], Tags: [
       {
          Key: 'Name',
          Value: 'SDK Sample'
       }
    ]};
    // Create a promise on an EC2 service object
    var tagPromise = new AWS.EC2({apiVersion: '2016-11-15'}).createTags(tagParams).promise();
    // Handle promise's fulfilled/rejected states
    tagPromise.then(
      function(data) {
        console.log("Instance tagged");
      }).catch(
        function(err) {
        console.error(err, err.stack);
      });
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\.

```
node ec2_createinstances.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ec2/ec2_createinstances.js)\.