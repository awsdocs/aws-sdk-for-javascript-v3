--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Create the AWS resources<a name="serverless-step-functions-example-create-resources"></a>

This topic is part of a tutorial that demonstrates to to invoke Lambda functions using AWS Step Functions\. To start at the beginning of the tutorial, see [Creating AWS serverless workflows using AWS SDK for JavaScript](serverless-step-functions-example.md)\.

This tutorial requires the following resources\.
+ An Amazon DynamoDB table named **Case** with a key named **Id**\.
+  An IAM role named `lambda-support` used to invoke Lamdba functions\. This role has policies that enable it to invoke the Amazon DynamoDB and Amazon Simple Email Service services from a Lambda function\. 
+  An IAM role named `workflow-support` used to invoke the workflow\.
+ An Amazon S3 bucket to host the Lambda functions\.
**Important**  
This Amazon S3 bucket allows READ \(LIST\) public access, which enables anyone to list the objects within the bucket and potentially misuse the information\. If you do not delete this Amazon S3 bucket immediately after completing the tutorial, we highly recommend you comply with the [Security Best Practices in Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/dev/compM.html) in the *Amazon Simple Storage Service Developer Guide*\. 

You can create these resources manually, but we recommend provisioning these resources using the AWS Cloud Development Kit \(AWS CDK\) as described in this tutorial\.

**Note**  
The AWS CDK is a software development framework that enables you to define cloud application resources\. For more information, see the [AWS Cloud Development Kit \(AWS CDK\) Developer Guide](https://docs.aws.amazon.com/cdk/latest/guide/home.html)\.

## Create the AWS resources using the AWS CLI<a name="api-gateway-invoking-lambda-resources-cli"></a>

To run the stack using the AWS CLI:

1. Install and configure the AWS CLI following the instructions in the [AWS CLI User Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)\.

1. Create a file named `setup.yaml` in the root directory of your project folder, and copy the content [ here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/lambda-step-functions/setup.yaml) into it\.

1. Run the following command from the command line, replacing *STACK\_NAME* with a unique name for the stack\.
**Important**  
The stack name must be unique within an AWS Region and AWS account\. You can specify up to 128 characters, and numbers and hyphens are allowed\.

   ```
   aws cloudformation create-stack --stack-name STACK_NAME --template-body file://setup.yaml --capabilities CAPABILITY_IAM
   ```

   For more information on the `create-stack` command parameters, see the [AWS CLI Command Reference guide](https://docs.aws.amazon.com/cli/latest/reference/cloudformation/create-stack.html), and the [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-cli-creating-stack.html)\.

## Create the AWS resources using the AWS Management Console;<a name="api-gateway-invoking-lambda-resources-console"></a>

To create resources for the app in the console, follow the instructions in the [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html)\. Use the template provided create a file named `setup.yaml`, and copy the content [ here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/lambda-step-functions/setup.yaml)\.

**Important**  
The stack name must be unique within an AWS Region and AWS account\. You can specify up to 128 characters, and numbers and hyphens are allowed\.

View a list of the resources in the console by opening the stack on the AWS CloudFormation dashboard, and choosing the **Resources** tab\. You require these for the tutorial\. 