--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Create the AWS resources<a name="serverless-step-functions-example-create-resources"></a>

This topic is part of a tutorial that demonstrates to to invoke Lambda functions using AWS Step Functions\. To start at the beginning of the tutorial, see [Creating AWS serverless workflows using AWS SDK for JavaScript](serverless-step-functions-example.md)\.

This tutorial requires the following resources\.
+ An Amazon DynamoDB table named **Case** with a key named **Id**\.
+  An IAM role named `lambda-support` used to invoke Lambda functions\. This role has policies that enable it to invoke the Amazon DynamoDB and Amazon Simple Email Service services from a Lambda function\. 
+  An IAM role named `workflow-support` used to invoke the workflow\.
+ An Amazon S3 bucket to host the Lambda functions\.

You can create these resources manually, but we recommend provisioning these resources using the AWS Cloud Development Kit \(AWS CDK\) \(AWS CDK\) as described in this tutorial\.

## Create the AWS resources using the AWS CloudFormation<a name="api-gateway-invoking-lambda-resources-cli"></a>

AWS CloudFormation enables you to create and provision AWS infrastructure deployments predictably and repeatedly\. For more information about AWS CloudFormation, see the [AWS CloudFormation developer guide\.](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)\.

To create the AWS CloudFormation stack:

1. Install and configure the AWS CLI following the instructions in the [AWS CLI User Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)\.

1. Create a file named `setup.yaml` in the root directory of your project folder, and copy the content [ here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/cross-services/lambda-step-functions/setup.yaml) into it\. 
**Note**  
The AWS CloudFormation template was generated using the AWS CDK available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/resources/cdk/lambda_api_step_functions)\. For more information about the AWS CDK, see the [AWS Cloud Development Kit \(AWS CDK\) Developer Guide](https://docs.aws.amazon.com/cdk/latest/guide/)\.

1. Run the following command from the command line, replacing *STACK\_NAME* with a unique name for the stack\.
**Important**  
The stack name must be unique within an AWS Region and AWS account\. You can specify up to 128 characters, and numbers and hyphens are allowed\.

   ```
   aws cloudformation create-stack --stack-name STACK_NAME --template-body file://setup.yaml --capabilities CAPABILITY_IAM
   ```

   For more information on the `create-stack` command parameters, see the [AWS CLI Command Reference guide](https://docs.aws.amazon.com/cli/latest/reference/cloudformation/create-stack.html), and the [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-cli-creating-stack.html)\.

## Create the AWS resources using the Amazon Web Services Management Console;<a name="api-gateway-invoking-lambda-resources-console"></a>

To create resources for the app in the console, follow the instructions in the [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html)\. Use the template provided create a file named `setup.yaml`, and copy the content [ here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/cross-services/lambda-step-functions/setup.yaml)\.

**Important**  
The stack name must be unique within an AWS Region and AWS account\. You can specify up to 128 characters, and numbers and hyphens are allowed\.

View a list of the resources in the console by opening the stack on the AWS CloudFormation dashboard, and choosing the **Resources** tab\. You require these for the tutorial\. 