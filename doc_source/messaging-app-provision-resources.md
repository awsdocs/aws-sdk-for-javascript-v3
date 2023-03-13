--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Create the AWS resources<a name="messaging-app-provision-resources"></a>

This topic is part of a tutorial that create an AWS application that sends and retrieves messages by using the AWS SDK for JavaScript and Amazon Simple Queue Service \(Amazon SQS\)\. To start at the beginning of the tutorial, see [Creating an example messaging application](messaging-app.md)\.

This tutorial requires the following resources\.
+ An unautenticated IAM role with permissions for Amazon SQS\.
+ A FIFO Amazon SQS Queue named **Message\.fifo** \- for information about creating a queue, see [Creating an Amazon SQS queue](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-configure-create-queue.html)\.

You can create this resources manually, but we recommend provisioning these resources using the AWS CloudFormation \(AWS CloudFormation\) as described in this tutorial\.

**Note**  
The AWS CloudFormation is a software development framework that enables you to define cloud application resources\. For more information, see the [AWS CloudFormation Developer Guide](https://docs.aws.amazon.com/cfn/latest/guide/home.html)\.

## Create the AWS resources using the AWS CloudFormation<a name="messagining-app-cli"></a>

AWS CloudFormation enables you to create and provision AWS infrastructure deployments predictably and repeatedly\. For more information about AWS CloudFormation, see the [AWS CloudFormation developer guide\.](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)\.

To create the AWS CloudFormation stack using the AWS CLI:

1. Install and configure the AWS CLI following the instructions in the [AWS CLI User Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)\.

1. Create a file named `setup.yaml` in the root directory of your project folder, and copy the content [ here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/message-app/setup.yaml) into it\.
**Note**  
The AWS CloudFormation template was generated using the AWS CDK available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/resources/cdk/messaging-app-unauthenticated-role-sqs-fifo-queue)\. For more information about the AWS CDK, see the [AWS Cloud Development Kit \(AWS CDK\) Developer Guide](https://docs.aws.amazon.com/cdk/latest/guide/)\.

1. Run the following command from the command line, replacing *STACK\_NAME* with a unique name for the stack\.
**Important**  
The stack name must be unique within an AWS Region and AWS account\. You can specify up to 128 characters, and numbers and hyphens are allowed\.

   ```
   aws cloudformation create-stack --stack-name STACK_NAME --template-body file://setup.yaml --capabilities CAPABILITY_IAM
   ```

   For more information on the `create-stack` command parameters, see the [AWS CLI Command Reference guide](https://docs.aws.amazon.com/cli/latest/reference/cloudformation/create-stack.html), and the [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-cli-creating-stack.html)\.

   To view the resources created, open AWS CloudFormation in the AWS management console, choose the stack, and select the **Resources** tab\.