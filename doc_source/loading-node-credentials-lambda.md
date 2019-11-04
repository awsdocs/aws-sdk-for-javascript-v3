# Loading Credentials for a Node\.js Lambda Function<a name="loading-node-credentials-lambda"></a>

When you create an AWS Lambda function, you must create a special IAM role that has permission to execute the function\. This role is called the *execution role*\. When you set up a Lambda function, you must specify the IAM role you created as the corresponding execution role\.

The execution role provides the Lambda function with the credentials it needs to run and to invoke other web services\. As a result, you do not need to provide credentials to the Node\.js code you write within a Lambda function\.

For more information about creating a Lambda execution role, see [Manage Permissions: Using an IAM Role \(Execution Role\)](https://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html#lambda-intro-execution-role) in the *AWS Lambda Developer Guide*\.