--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Deploy the Lambda function<a name="api-gateway-invoking-lambda-deploy-function"></a>

This topic is part of a tutorial that demonstrates how to invoke a Lambda function through Amazon API Gateway using the AWS SDK for JavaScript\. To start at the beginning of the tutorial, see [Invoking Lambda with API Gateway](api-gateway-invoking-lambda-example.md)\.

In the root of your project, create a `lambda-function-setup.ts` file, and paste the content below into it\.

Replace *BUCKET\_NAME* with the name of the Amazon S3 bucket you uploaded the ZIP version of your Lambda function to\. Replace *ZIP\_FILE\_NAME* with the name of name the ZIP version of your Lambda function\. Replace *ROLE* with the Amazon Resource Number \(ARN\) of the IAM role you created in the [Create the AWS resources ](api-gateway-invoking-lambda-provision-resources.md) topic of this tutorial\. Replace *LAMBDA\_FUNCTION\_NAME* with a name for the Lambda function\.

```
// Load the required Lambda client and commands.
const {
  LambdaClient,
  CreateFunctionCommand,
} = require("@aws-sdk/client-lambda");

// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"

// Instantiate an Lambda client service object.
const lambda = new LambdaClient({ region: REGION });

// Set the parameters.
const params = {
  Code: {
    S3Bucket: "BUCKET_NAME", // BUCKET_NAME
    S3Key: "ZIP_FILE_NAME", // ZIP_FILE_NAME
  },
  FunctionName: "LAMBDA_FUNCTION_NAME",
  Handler: "index.handler",
  Role: "IAM_ROLE_ARN", // IAM_ROLE_ARN; e.g., arn:aws:iam::650138640062:role/v3-lambda-tutorial-lambda-role
  Runtime: "nodejs12.x",
  Description:
    "Scans a DynamoDB table of employee details and using Amazon Simple Notification Services (Amazon SNS) to " +
    "send employees an email the each anniversary of their start-date.",
};

const run = async () => {
  try {
    const data = await lambda.send(new CreateFunctionCommand(params));
    console.log("Success", data); // successful response
  } catch (err) {
    console.log("Error", err); // an error occurred
  }
};
run();
```

Enter the following at the command line to deploy the Lambda function\.

```
ts-node lambda-function-setup.ts
```

This code example is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/lambda-api-gateway/src/helper-functions/lambda-function-setup.ts)\.