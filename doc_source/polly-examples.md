--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Amazon Polly examples<a name="polly-examples"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ Upload audio recorded using Amazon Polly to Amazon S3

## The scenario<a name="polly-example-synthesize-to-s3-scenario"></a>

In this example, a series of Node\.js modules are used to automatically upload audio recorded using Amazon Polly to Amazon S3 using these methods of the Amazon S3 client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-polly/classes/startspeechsynthesistaskcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-polly/classes/startspeechsynthesistaskcommand.html)

## Prerequisite tasks<a name="polly-example-synthesize-to-s3-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up a project environment to run Node JavaScript examples by following the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create resources for the example using the template [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/polly/general-examples/src/setup.yaml) to create a AWS CDK stack using either the [AWS Web Services Management Console](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html) or the [AWS CLI](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-cli-creating-stack.html)\. For instructions on how to modify the stack, or to delete the stack and its associated resources when you have finished the tutorial, see [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/resources/cdk/javascript_example_code_polly_aws_service)\.
**Note**  
The stack name must be unique within an AWS Region and AWS account\. You can specify up to 128 characters, and numbers and hyphens are allowed\.

  This resulting AWS CloudFormation stack creates the following resources:
  + An AWS Identity and Access Management role with full access permissions to Amazon Polly\.
  + An Amazon Cognito identity pool with the IAM role attached to it\.

## Upload audio recorded using Amazon Polly to Amazon S3<a name="polly-example-synthesize-to-s3-example"></a>

Create a Node\.js module with the file name `polly_synthesize_to_s3.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. In the code, enter the *REGION*, and the *BUCKET\_NAME*\. To access Amazon Polly, create an `Polly` client service object\. Replace *"IDENTITY\_POOL\_ID"* with the `IdentityPoolId` from the **Sample page** of the Amazon Cognito identity pool you created for this example\. This is also passed to each client object\.

**Note**  

![\[Identity pool ID\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Identity pool ID\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Identity pool ID\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

Call the `StartSpeechSynthesisCommand` method of the Amazon Polly client service object synthesize the voice message and upload it to the Amazon S3 bucket\. 

```
const { CognitoIdentityClient } = require("@aws-sdk/client-cognito-identity");
const {
 fromCognitoIdentityPool,
} = require("@aws-sdk/credential-provider-cognito-identity");
const {
 Polly,
 StartSpeechSynthesisTaskCommand,
} = require("@aws-sdk/client-polly");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Create the parameters
var s3Params = {
 OutputFormat: "mp3",
 OutputS3BucketName: "BUCKET_NAME",
 Text: "Hello David, How are you?",
 TextType: "text",
 VoiceId: "Joanna",
 SampleRate: "22050",
};
// Create the Polly service client, assigning your credentials
const polly = new Polly({
 region: REGION,
 credentials: fromCognitoIdentityPool({
 client: new CognitoIdentityClient({ region: REGION }),
 identityPoolId: "IDENTITY_POOL_ID", // IDENTITY_POOL_ID
 }),
});

const run = async () => {
 try {
 const data = await polly.send(
 new StartSpeechSynthesisTaskCommand(s3Params)
 );
 console.log("Audio file added to " + s3Params.OutputS3BucketName);
 } catch (err) {
 console.log("Error putting object", err);
 }
};
run();
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/polly/general-examples/src/polly_synthesize_to_s3.js)\.