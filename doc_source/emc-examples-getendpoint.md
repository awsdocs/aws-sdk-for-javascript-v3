--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Getting your account\-specific endpoint for MediaConvert<a name="emc-examples-getendpoint"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve your account\-specific endpoint from MediaConvert\.

## The scenario<a name="emc-example-getendpoint-scenario"></a>

In this example, you use a Node\.js module to call MediaConvert and retrieve your account\-specific endpoint\. You can retrieve your endpoint URL from the service default endpoint and so do not yet need your account\-specific endpoint\. The code uses the SDK for JavaScript to retrieve this endpoint by using this method of the MediaConvert client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#describeEndpoints-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#describeEndpoints-property)

## Prerequisite tasks<a name="emc-example-getendpoint-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/mediaconvert/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create an IAM role that gives MediaConvert access to your input files and the Amazon S3 buckets where your output files are stored\. For details, see [Set up IAM permissions](https://docs.aws.amazon.com/mediaconvert/latest/ug/iam-role.html) in the *AWS Elemental MediaConvert User Guide*\.

## Getting your endpoint URL<a name="emc-example-getendpoint-url"></a>

Create a Node\.js module with the file name `emc_getendpoint.ts`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the empty request parameters for the `DescribeEndpointsCommand` method of the MediaConvert client class\. To call the `DescribeEndpointsCommand` method, create a promise for invoking an MediaConvert client service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *ACCOUNT\_ENDPOINT* with your AWS Region, *AMI\_ID* with the ID of the AMI to run, and *KEY\_PAIR\_NAME* of the key pair to assign to the AMI ID\.

```
// Import required AWS-SDK clients and commands for Node.js
const {
  MediaConvertClient,
  DescribeEndpointsCommand
} = require("@aws-sdk/client-mediaconvert");

//set the parameters
const endpoint = { endpoint: "ACCOUNT_END_POINT" }; //ACCOUNT_END_POINT
const params = { MaxResults: 0 };

//Set the MediaConvert Service Object
const mediaconvert = new MediaConvertClient(endpoint);

const run = async () => {
  try {
    // Load the required SDK for JavaScript modules
    // Create a new service object and set MediaConvert to customer endpoint
    const params = { MaxResults: 0 };
    const data = await mediaconvert.send(new DescribeEndpointsCommand(params));
    console.log("Your MediaConvert endpoint is ", data.Endpoints);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node emc_getendpoint.ts // If you prefer JavaScript, enter 'node emc_getendpoint.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/mediaconvert/src/emc_getendpoint.ts)\.