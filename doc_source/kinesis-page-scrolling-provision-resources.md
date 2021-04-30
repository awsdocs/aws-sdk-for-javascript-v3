--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Create the AWS resources<a name="kinesis-page-scrolling-provision-resources"></a>

This topic is part of a example that demonstrates how to capture and process browser event data with Amazon Kinesis\. To start at the beginning of the example, see [Capturing Webpage Scroll Progress with Amazon Kinesis](kinesis-examples-capturing-page-scrolling.md)\.

This example requires the following resources\.
+ An Amazon Kinesis stream\. 
+ An Amazon Cognito identity pool with access enabled for unauthenticated identities\. 
+ An AWS Identity and Access Management role whose policy grants permission to submit data to an Amazon Kinesis stream\.

You can create these resources manually, but we recommend provisioning these resources using the AWS Cloud Development Kit \(AWS CDK\) as described in this topic\.

**Note**  
The AWS CDK is a software development framework that enables you to define cloud application resources\. For more information, see the [AWS Cloud Development Kit \(AWS CDK\) Developer Guide](https://docs.aws.amazon.com/cdk/latest/guide/home.html)\.

## Create the AWS resources using the AWS CLI<a name="kinesis-page-scrolling-resources-cli"></a>

To creat the resources using the AWS CLI:

1. Install and configure the AWS CLI following the instructions in the [AWS CLI User Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)\.

1. Create a file named `setup.yaml` in the root directory of your project folder, and copy the content [ here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/kinesis/src/setup.yaml) into it\.

1. Run the following command from the command line, replacing *STACK\_NAME* with a unique name for the stack\.
**Important**  
The stack name must be unique within an AWS Region and AWS account\. You can specify up to 128 characters, and numbers and hyphens are allowed\.

   ```
   aws cloudformation create-stack --stack-name STACK_NAME --template-body file://setup.yaml --capabilities CAPABILITY_IAM
   ```

   For more information on the `create-stack` command parameters, see the [AWS CLI Command Reference guide](https://docs.aws.amazon.com/cli/latest/reference/cloudformation/create-stack.html), and the [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-cli-creating-stack.html)\.

The resources are created, are the values you require as variables for this example are returned in the command window\. These include:
+ An Amazon Kinesis stream\. You need to include the name of the stream the browser script\.
+ An Amazon Cognito identity pool with access enabled for unauthenticated identities\. You need to include the identity pool ID in the code to obtain credentials for the browser script\. For more information about Amazon Cognito identity pools, see [Identity Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/identity-pools.html) in the *Amazon Cognito Developer Guide*\.
+ An IAM role with an attached IAM policy that grants permission to submit data to an Amazon Kinesis stream\. For more information about creating an IAM role, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.
**Note**  
This is the role policy when is attached to the IAM role\. The CDK automatically poplulates the *STREAM\_RESOURCE\_ARN*\.  

  ```
  {
     "Version": "2012-10-17",
     "Statement": [
        {
           "Effect": "Allow",
           "Action": [
              "mobileanalytics:PutEvents",
              "cognito-sync:*"
           ],
           "Resource": [
              "*"
           ]
        },
        {
           "Effect": "Allow",
           "Action": [
              "kinesis:Put*"
           ],
           "Resource": [
              "STREAM_RESOURCE_ARN"
           ]
        }
     ]
  }
  ```
**Note**  
The CDK automatically poplulates the *STREAM\_RESOURCE\_ARN*\. 

## Create the AWS resources using the AWS Management Console;<a name="kinesis-page-scrolling-resources-console"></a>

To create resources for the app in the console, follow the instructions in the [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html)\. Use the template provided create a file named `setup.yaml`, and copy the content [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/kinesis/src/setup.yaml)\.

**Important**  
The stack name must be unique within an AWS Region and AWS account\. You can specify up to 128 characters, and numbers and hyphens are allowed\.

View a list of the resources in the console by opening the stack on the AWS CloudFormation dashboard, and choosing the **Resources** tab\. You require these for the example\. 