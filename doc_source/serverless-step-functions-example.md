--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Creating AWS serverless workflows using AWS SDK for JavaScript<a name="serverless-step-functions-example"></a>

You can create an AWS serverless workflow by using the AWS SDK for Java and AWS Step Functions\. Each workflow step is implemented using anAWS Lambda function\. Lambda is a compute service that enables you to run code without provisioning or managing servers\. Step Functions is a serverless orchestration service that lets you combine Lambda functions and other AWS services to build business\-critical applications\. 

**Note**  
 You can create Lambda functions in various programming languages\. For this tutorial, Lambda functions implemented by using the Lambda Java API\. For more information about Lambda, see [What is AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)\.

In this tutorial, you create a workflow that creates support tickets for an organization\. Each workflow step performs an operation on the ticket\. This tutorial shows you how to use JavaScript to process workflow data\. For example, you’ll learn how to read data that’s passed to the workflow, how to pass data between steps, and how to invoke AWS services from the workflow\.

**Cost to complete:** The AWS services included in this document are included in the [AWS Free Tier](https://aws.amazon.com/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc)\.

**Note:** Be sure to terminate all of the resources you create while going through this tutorial to ensure that you’re no longer charged\.

**To build the app:**

1. [Prerequisite tasks](serverless-step-functions-example-prerequisites.md)

1. [Create the AWS resources ](serverless-step-functions-example-create-resources.md)

1. [Creating the workflow](serverless-step-functions-example-understand-workflow.md)

1. [Create the Lambda functions ](serverless-step-functions-example-create-lambda-functions.md)

1. [Execute your workflow by using the Step Functions console](serverless-step-functions-example-execute-workflow.md)