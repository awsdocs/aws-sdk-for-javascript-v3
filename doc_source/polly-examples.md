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
+ Create an AWS Identity and Access Management \(IAM\) Unauthenticated Amazon Cognito user role polly:SynthesizeSpeech permissions, and an Amazon Cognito identity pool with the IAM role attached to it\. The [Create the AWS resources using the AWS CloudFormation](#polly-example-synthesize-to-s3-create-resources)section below describes how to create these resources\.

**Note**  
This example uses Amazon Cognito, but if you are not using Amazon Cognito then your AWS user must have following IAM permissions policy  

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Action": [
        "mobileanalytics:PutEvents",
        "cognito-sync:*"
      ],
      "Resource": "*",
      "Effect": "Allow"
    },
    {
      "Action": "polly:SynthesizeSpeech",
      "Resource": "*",
      "Effect": "Allow"
    }
  ]
}
```

## Create the AWS resources using the AWS CloudFormation<a name="polly-example-synthesize-to-s3-create-resources"></a>

AWS CloudFormation enables you to create and provision AWS infrastructure deployments predictably and repeatedly\. For more information about AWS CloudFormation, see the [AWS CloudFormation developer guide\.](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)\.

To create the AWS CloudFormation stack:

1. Install and configure the AWS CLI following the instructions in the [AWS CLI User Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)\.

1. Create a file named `setup.yaml` in the root directory of your project folder, and copy the content [ here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/polly/general-examples/src/setup.yaml) into it\. 
**Note**  
The AWS CloudFormation template was generated using the AWS CDK available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/resources/cdk/javascript_example_code_polly_aws_service/)\. For more information about the AWS CDK, see the [AWS Cloud Development Kit \(CDK\) Developer Guide](https://docs.aws.amazon.com/cdk/latest/guide/)\.

1. Run the following command from the command line, replacing *STACK\_NAME* with a unique name for the stack\.
**Important**  
The stack name must be unique within an AWS Region and AWS account\. You can specify up to 128 characters, and numbers and hyphens are allowed\.

   ```
   aws cloudformation create-stack --stack-name STACK_NAME --template-body file://setup.yaml --capabilities CAPABILITY_IAM
   ```

   For more information on the `create-stack` command parameters, see the [AWS CLI Command Reference guide](https://docs.aws.amazon.com/cli/latest/reference/cloudformation/create-stack.html), and the [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-cli-creating-stack.html)\.

1. Navigate to the AWS CloudFormation management console, choose **Stacks**, choose the stack name, and choose the **Resources** tab to view a list of the created resources\.  
![\[AWS CloudFormation resources\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[AWS CloudFormation resources\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[AWS CloudFormation resources\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

## Upload audio recorded using Amazon Polly to Amazon S3<a name="polly-example-synthesize-to-s3-example"></a>

Create a Node\.js module with the file name `polly_synthesize_to_s3.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. In the code, enter the *REGION*, and the *BUCKET\_NAME*\. To access Amazon Polly, create an `Polly` client service object\. Replace *"IDENTITY\_POOL\_ID"* with the `IdentityPoolId` from the **Sample page** of the Amazon Cognito identity pool you created for this example\. This is also passed to each client object\.

Call the `StartSpeechSynthesisCommand` method of the Amazon Polly client service object synthesize the voice message and upload it to the Amazon S3 bucket\. 

```
const {
  Polly,
  StartSpeechSynthesisTaskCommand,
} = require("@aws-sdk/client-polly");
const { pollyClient } = require("./libs/pollyClient.js");

// Create the parameters
var params = {
  OutputFormat: "mp3",
  OutputS3BucketName: "videoanalyzerbucket",
  Text: "Hello David, How are you?",
  TextType: "text",
  VoiceId: "Joanna",
  SampleRate: "22050",
};

const run = async () => {
  try {
    const data = await pollyClient.send(
      new StartSpeechSynthesisTaskCommand(params)
    );
    console.log("Success, audio file added to " + params.OutputS3BucketName);
  } catch (err) {
    console.log("Error putting object", err);
  }
};
run();
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/polly/general-examples/src/polly_synthesize_to_s3.js)\.