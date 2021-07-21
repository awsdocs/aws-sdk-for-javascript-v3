--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Create the AWS resources<a name="transcribe-app-provision-resources"></a>

This topic is part of a tutorial about building an app that transcribes and displays voice messages for authenticated users\. To start at the beginning of the tutorial, see [Build a transcription app with authenticated users](transcribe-app.md)\. 

This topic desribes how to provison AWS resources for this app using the AWS Cloud Development Kit \(CDK\)\.

**Note**  
The AWS CDK is a software development framework that enables you to define cloud application resources\. For more information, see the [AWS Cloud Development Kit \(CDK\) Developer Guide](https://docs.aws.amazon.com/cdk/latest/guide/home.html)\.

To create resources for the app, use the template [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/transcription-app/setup.yaml) to create a AWS CDK stack using either the [AWS Web Services Management Console](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html) or the [AWS CLI](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-cli-creating-stack.html)\. For instructions on how to modify the stack, or to delete the stack and its associated resources when you have finished the tutorial, see [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/resources/cdk/javascript_example_code_transcribe_demo/)\.

**Note**  
The stack name must be unique within an AWS Region and AWS account\. You can specify up to 128 characters, and numbers and hyphens are allowed\.

The resulting stack automatically provisions the following resources\.
+  An Amazon Cognito identity pool with an authenticated user role\.
+ An IAM policy with permissions for the Amazon S3 and Amazon Transcribe is attached to the authenticated user role\.
+  An Amazon Cognito user pool that enables users to sign up and sign in to the app\.
+ An Amazon S3 bucket to host the application files\.
+ An Amazon S3 bucket to to store the transcriptions\.
**Important**  
This Amazon S3 bucket allows READ \(LIST\) public access, which enables anyone to list the objects within the bucket and potentially misuse the information\. If you do not delete this Amazon S3 bucket immediately after completing the tutorial, we highly recommend you comply with the [Security Best Practices in Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/compM.html) in the *Amazon Simple Storage Service Developer Guide*\. 