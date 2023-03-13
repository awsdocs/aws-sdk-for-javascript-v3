--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Getting your region\-specific endpoint for MediaConvert<a name="emc-examples-getendpoint"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to retrieve your region\-specific endpoint from MediaConvert\.

## The scenario<a name="emc-example-getendpoint-scenario"></a>

In this example, you use a Node\.js module to call MediaConvert and retrieve your region\-specific endpoint\. You can retrieve your endpoint URL from the service default endpoint and so do not yet need your region\-specific endpoint\. The code uses the SDK for JavaScript to retrieve this endpoint by using this method of the MediaConvert client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-mediaconvert/classes/describeendpointscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-mediaconvert/classes/describeendpointscommand.html)

## Prerequisite tasks<a name="emc-example-getendpoint-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/mediaconvert/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create an IAM role that gives MediaConvert access to your input files and the Amazon S3 buckets where your output files are stored\. For details, see [Set up IAM permissions](https://docs.aws.amazon.com/mediaconvert/latest/ug/iam-role.html) in the *AWS Elemental MediaConvert User Guide*\.

**Important**  
This example uses ECMAScript6 \(ES6\)\. This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.  
However, if you prefer to use CommonJS syntax, please refer to [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Getting your endpoint URL<a name="emc-example-getendpoint-url"></a>

Create a `libs` directory, and create a Node\.js module with the file name `emcClientGet.js`\. Copy and paste the code below into it, which creates the MediaConvert client object\. Replace *REGION* with your AWS Region\. 

```
import { MediaConvertClient } from "@aws-sdk/client-mediaconvert";
// Set the AWS Region.
const REGION = "REGION";
//Set the MediaConvert Service Object
const emcClientGet = new MediaConvertClient({region: REGION});
export { emcClientGet };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/mediaconvert/src/libs/emcClientGet.js)\.

Create a Node\.js module with the file name `emc_getendpoint.js`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the empty request parameters for the `DescribeEndpointsCommand` method of the MediaConvert client class\. Then call the `DescribeEndpointsCommand` method\. 

```
// Import required AWS-SDK clients and commands for Node.js
import { DescribeEndpointsCommand } from  "@aws-sdk/client-mediaconvert";
import { emcClientGet } from  "./libs/emcClientGet.js";

//set the parameters.
const params = { MaxResults: 0 };

const run = async () => {
  try {
    // Create a new service object and set MediaConvert to customer endpoint
    const data = await emcClientGet.send(new DescribeEndpointsCommand(params));
    console.log("Your MediaConvert endpoint is ", data.Endpoints);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node emc_getendpoint.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/mediaconvert/src/emc_getendpoint.js)\.