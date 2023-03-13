--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Loading credentials for a Node\.js Lambda function<a name="loading-node-credentials-lambda"></a>

When you create an AWS Lambda function, you must create a special IAM role that has permission to execute the function\. This role is called the *execution role*\. When you set up a Lambda function, you must specify the IAM role you created as the corresponding execution role\.

The execution role provides the Lambda function with the credentials it needs to run and to invoke other web services\. As a result, you don't need to provide credentials to the Node\.js code you write within a Lambda function\.

For more information about creating a Lambda execution role, see [Manage permissions: Using an IAM role \(execution role\)](https://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html#lambda-intro-execution-role) in the *AWS Lambda Developer Guide*\.