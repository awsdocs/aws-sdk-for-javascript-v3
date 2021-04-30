--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Deploy the Lambda function<a name="lambda-create-table-deploy-function"></a>

This topic is part of a tutorial that demonstrates how to create, deploy, and run a Lambda function using the AWS SDK for JavaScript\. To start at the beginning of the tutorial, see [Creating and using Lambda functions](lambda-create-table-example.md)\.

In the root of your project, create a `lambda-function-setup.ts` file, and paste the content below into it\.

Replace *BUCKET\_NAME* with the name of the Amazon S3 bucket you uploaded the ZIP version of your Lambda function to\. Replace *KEY* with the name of name the ZIP version of your Lambda function\. Replace *ROLE* with the Amazon Resource Number \(ARN\) of the IAM role you created in the [Create the AWS resources ](lambda-create-table-provision-resources.md) topic of this tutorial\. Replace *LAMBDA\_FUNCTION* with the same name you gave the function in the `/Lambda/index.ts` in the [Prepare the browser script](lambda-create-table-browser-script.md) topic of this tutorial\.

```
// Load the Lambda client.
const {
    LambdaClient,
    CreateFunctionCommand
} = require("@aws-sdk/client-lambda");

//Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"

// Instantiate an AWS Lambda client service object.
const lambda = new LambdaClient({ region: REGION });

// Set the parameters.
const params = {
    Code: {
        S3Bucket: "BUCKET_NAME", // BUCKET_NAME
        S3Key: "ZIP_FILE_NAME", // ZIP_FILE_NAME
    },
    FunctionName: "FUNCTION_NAME",
    Handler: "index.handler",
    Role: "IAM_ROLE_ARN", // IAM_ROLE_ARN; e.g., arn:aws:iam::650138640062:role/v3-lambda-tutorial-lambda-role
    Runtime: "nodejs12.x",
    Description: "Creates an Amazon DynamoDB table.",
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

This code example is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/lambda/lambda_create_function/src/lambda-function-setup.ts)\.