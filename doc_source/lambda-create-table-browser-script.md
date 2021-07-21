--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Prepare the browser script<a name="lambda-create-table-browser-script"></a>

This topic is part of a tutorial that demonstrates how to create, deploy, and run a Lambda function using the AWS SDK for JavaScript\. To start at the beginning of the tutorial, see [Creating and using Lambda functions](lambda-create-table-example.md)\.

First, create the required service client objects\. Create a `libs` folder, and in it create two files, `dynamoClient.js` and `lambdaClient.js`\. Paste the code below into `dynamoClient.js`\.

```
const { CognitoIdentityClient } = require ( "@aws-sdk/client-cognito-identity" );
const { fromCognitoIdentityPool } = require ( "@aws-sdk/credential-provider-cognito-identity" );
const { DynamoDBClient } = require ( "@aws-sdk/client-dynamodb" );

const REGION = "REGION";
const IDENTITY_POOL_ID = "IDENTITY_POOL_ID"; // An Amazon Cognito Identity Pool ID.

// Create an Amazon DynamoDB service client object.
const dynamoClient = new DynamoDBClient({
  region: REGION,
  credentials: fromCognitoIdentityPool({
    client: new CognitoIdentityClient({ region: REGION }),
    identityPoolId: IDENTITY_POOL_ID,
  }),
});

module.exports = { dynamoClient };
```

Paste the code below into `lambdaClient.js`\.

```
const { lambdaClient } = require ("@aws-sdk/client-lambda" );
const {
  fromCognitoIdentityPool,
} = require ( "@aws-sdk/credential-provider-cognito-identity" );
const { CognitoIdentityClient }  = require ("@aws-sdk/client-cognito-identity" );

// Set the AWS Region.
const REGION = "eu-west-1"; // e.g., 'us-east-2'
const IDENTITY_POOL_ID = "eu-west-1:dc7d706a-1f07-4fa5-baa7-edfabc05f293";

// Create an AWS Lambda client service object that initializes the Amazon Cognito credentials provider.
const lambdaClient = new LambdaClient({
  region: REGION,
  credentials: fromCognitoIdentityPool({
    client: new CognitoIdentityClient({ region: REGION }),
    identityPoolId: IDENTITY_POOL_ID
  }),
});
module.exports = {lambdaClient}
```

In both, replace *REGION* with the AWS region\. Create an Lambda client service object as show\. Replace *IDENTITY\_POOL\_ID* with the `IdentityPoolId` of the Amazon Cognito identity pool you created in the [Create the AWS resources ](lambda-create-table-provision-resources.md) topic of this tutorial\.

In the `LambdaApp` folder, create a file name `index.js`, and paste the content below into it\.

```
// Load the required clients and packages.
const { InvokeCommand } = require ("@aws-sdk/client-lambda" );
const { lambdaClient } = require ( "../libs/lambdaClient" );

// Set the parmaeters.
const params={
  // The name of the AWS Lambda function.
  FunctionName: "LAMBDA_FUNCTION",
  InvocationType: "RequestResponse",
  LogType: "None"
}

// Call the Lambda function.
window.createTable = async () => {
  try {
    const data = await lambdaClient.send(new InvokeCommand(params));
    console.log("Table Created", data);
    document.getElementById('message').innerHTML = "Success, table created"
  } catch (err) {
    console.log("Error", err);
  }
};
```

In the parameters, replace *LAMBDA\_FUNCTION* with a name unique in your AWS account, for example `createTable`\.

This code example is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/lambda/lambda_create_function/src/LambdaApp/index.js)\.

Finally, run the following at the command prompt to bundle the JavaScript for this example in a file named `main.js`\.

```
webpack LambdaApp/index.js --mode development --target web --devtool false -o LambdaApp/main.js
```

**Note**  
For information about installing webpack, see [Bundling applications with webpack](webpack.md)\.