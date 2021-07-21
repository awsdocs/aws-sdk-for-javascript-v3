--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Create the AWS resources<a name="lambda-create-table-provision-resources"></a>

This topic is part of a tutorial that demonstrates how to create, deploy, and run a Lambda function using the AWS SDK for JavaScript\. To start at the beginning of the tutorial, see [Creating and using Lambda functions](lambda-create-table-example.md)\.

This tutorial requires the following resources\.
+  An Amazon Cognito identity pool with an unauthenticated user role\.
+ An IAM policy with permissions for the DynamoDB and Lambda is attached to the unauthenticated user role\.
+ An Amazon S3 bucket to host the browser HTML and script pages, and the Lambda function\.

You can create these resources manually, but we recommend provisioning these resources using the AWS Cloud Development Kit \(CDK\) \(AWS CDK\) as described in this tutorial\.

## Create the AWS resources using the AWS CloudFormation<a name="lambda-create-table-resources-cli"></a>

AWS CloudFormation enables you to create and provision AWS infrastructure deployments predictably and repeatedly\. For more information about AWS CloudFormation, see the [AWS CloudFormation developer guide\.](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)\.

To create the AWS CloudFormation stack:

1. Install and configure the AWS CLI following the instructions in the [AWS CLI User Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)\.

1. Create a file named `describe-stack-resources.js` in the root directory of your project folder\.

1. Create a file named `setup.yaml` in the root directory of your project folder, and copy the content [ here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/lambda/lambda_create_function/setup.yaml) into it\.
**Note**  
The AWS CloudFormation template was generated using the AWS CDK available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/resources/cdk/javascript_example_lambda_aws_service)\. For more information about the AWS CDK, see the [AWS Cloud Development Kit \(CDK\) Developer Guide](https://docs.aws.amazon.com/cdk/latest/guide/)\.

1. Run the following command from the command line, replacing *STACK\_NAME* with a unique name for the stack\.
**Important**  
The stack name must be unique within an AWS Region and AWS account\. You can specify up to 128 characters, and numbers and hyphens are allowed\.

   ```
   aws cloudformation create-stack --stack-name STACK_NAME --template-body file://setup.yaml --capabilities CAPABILITY_IAM
   ```

   For more information on the `create-stack` command parameters, see the [AWS CLI Command Reference guide](https://docs.aws.amazon.com/cli/latest/reference/cloudformation/create-stack.html), and the [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-cli-creating-stack.html)\.

1. Copy and paste the code below into `describe-stack-resources.js`\.

   ```
   // Load the AWS SDK for Node.js
   const {
     CloudFormationClient,
     DescribeStackResourcesCommand,
     CreateStackCommand,
     DescribeStacksCommand
   } = require("@aws-sdk/client-cloudformation");
   
   // Create S3 service object
   const cloudformation = new CloudFormationClient();
   
   var params = {
     StackName: process.argv[2]
   }
   
   const getVariables = async () => {
         try {
           const data = await cloudformation.send(
               new DescribeStacksCommand({StackName: params.StackName}));
           console.log('Status: ', data.Stacks[0].StackStatus);
           if (data.Stacks[0].StackStatus == "CREATE_COMPLETE") {
             const data = await cloudformation.send(
                 new DescribeStackResourcesCommand({StackName: params.StackName})
             );
             for (var i = 0; i < data.StackResources.length; i++) {
               var obj = data.StackResources[i].ResourceType;
               if (obj == "AWS::IAM::Policy") {
                 const IDENTITY_POOL_ID = data.StackResources[i].LogicalResourceId;
                 console.log("IDENTITY_POOL_ID:", IDENTITY_POOL_ID);
                 var identity_pool_id = IDENTITY_POOL_ID;
               }
               if (obj == "AWS::S3::Bucket") {
                 const BUCKET_NAME = data.StackResources[i].PhysicalResourceId;
                 console.log("BUCKET_NAME:", BUCKET_NAME);
                 var bucket = BUCKET_NAME;
               }
               if (obj == "AWS::IAM::Role") {
                 const IAM_ROLE = data.StackResources[i].StackId;
                 console.log("IAM_ROLE:", IAM_ROLE);
                 var iam_role = IAM_ROLE;
               }
             }
           }
           else{
             console.log('Stack not ready yet. Try again in a few minutes.')
           }
         }catch (err) {
           console.log("Error listing resources", err);
         }
       }
   ;
   getVariables();
   ```

    This code is available [ here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/lambda/lambda_create_function/describe-stack-resources.js)\.

1. Run the following command from the command line, replacing *STACK\_NAME* with a unique name for the stack\.

   ```
   node describe-stack-resources.js STACK_NAME
   ```

   Take note of the *IAM\_ROLE*, *IDENTITY\_POOL\_ID*, and *BUCKET\_NAME* returned in the command line, as you need them for this tutorial\.