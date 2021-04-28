--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Prepare the browser script<a name="lambda-create-table-browser-script"></a>

This topic is part of a tutorial that demonstrates how to create, deploy, and run a Lambda function using the AWS SDK for JavaScript\. To start at the beginning of the tutorial, see [Tutorial: Create and using Lambda functions](lambda-create-table-example.md)\.

In the `LambdaApp` folder, create a file name `index.ts`, and paste the content below into it\.

 First, replace *REGION* with the AWS region\. Create an Lambda client service object as show\. Replace *IDENTITY\_POOL\_ID* with the `IdentityPoolId` of the Amazon Cognito identity pool you created in the [Create the AWS resources ](lambda-create-table-provision-resources.md) topic of this tutorial\. In the parameters, replace *LAMBDA\_FUNCTION* with a name unique in your AWS account, for example `createTable`\.

**Note**  

![\[Preparing an Amazon Cognito identity pool for the browser script\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/identity-pool-id.png)![\[Preparing an Amazon Cognito identity pool for the browser script\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Preparing an Amazon Cognito identity pool for the browser script\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

```
// Load the required clients and packages.
const { CognitoIdentityClient } = require("@aws-sdk/client-cognito-identity");
const {
  fromCognitoIdentityPool,
} = require("@aws-sdk/credential-provider-cognito-identity");
const { LambdaClient, InvokeCommand } = require("@aws-sdk/client-lambda");

// Set the AWS Region.
const REGION = "REGION"; // e.g., 'us-east-2'

// Set the parmaeters.
const params={
  // The name of the Amazon Lambda function.
  FunctionName: "LAMBDA_FUNCTION",
  InvocationType: "RequestResponse",
  LogType: "None"
}

// Create an Amazon Lambda client service object that initializes the Amazon Cognito credentials provider.
const lambda = new LambdaClient({
  region: REGION,
  credentials: fromCognitoIdentityPool({
    client: new CognitoIdentityClient({ region: REGION }),
    identityPoolId: "IDENTITY_POOL_ID", // IDENTITY_POOL_ID e.g., eu-west-1:xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx
  }),
});

// Call the Lambda function.
window.createTable = async () => {
  try {
    const data = await lambda.send(new InvokeCommand(params));
    console.log("Table Created", data);
    document.getElementById('message').innerHTML = "Success, table created"
  } catch (err) {
    console.log("Error", err);
  }
};
```

This code example is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/lambda/lambda_create_function/src/LambdaApp/index.ts)\.

Finally, run the following at the command prompt to bundle the JavaScript for this example in a file named `main.js`\.

```
webpack index.ts --mode development --target web --devtool false -o main.js
```

**Note**  
For information about installing webpack, see [Bundling applications with webpack](webpack.md)\.