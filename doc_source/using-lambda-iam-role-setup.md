--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Create a Lambda execution role in IAM<a name="using-lambda-iam-role-setup"></a>

This topic is part of a larger tutorial about using the AWS SDK for JavaScript with AWS Lambda functions\. To start at the beginning of the tutorial, see [Tutorial: Creating and using Lambda functions](using-lambda-functions.md)\.

In this task, you will focus on creating IAM that the application to run the Lambda function\.

![\[Create an IAM execution role for Lambda\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/create-iam-role.png)![\[Create an IAM execution role for Lambda\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Create an IAM execution role for Lambda\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

A Lambda function requires an execution role created in IAM that provides the function with the necessary permissions to run\. For more information about the Lambda execution role, see [Manage permissions: Using an IAM role \(execution role\)](https://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html#lambda-intro-execution-role) in the *AWS Lambda Developer Guide*\. 

**To create the Lambda execution role in IAM**

1. In a text editor open `lambda-role-setup.js` in the `src` directory\.

1. Find this line of code\.

   `RoleName: "ROLE"`

   Replace **ROLE** with another name\. 

1. Save your changes and close the file\. 

1. At the command line, type the following\.

   `node lambda-role-setup.ts`

1. Make a note of the ARN returned by the script\. You need this value to create the Lambda function\. 

## Setup script code<a name="using-lambda-s3-setup-script"></a>

The following code is the setup script that creates the Lambda execution role\. The setup script creates the JSON that defines the trust relationship needed for a Lambda execution role\. It also creates the JSON parameters for attaching the `AWSLambdaRole` managed policy\. Then it assigns the string version of the JSON to the parameters for the `createRole` method of the IAM service object\. 

The `createRole` method automatically URL\-encodes the JSON to create the execution role\. When the new role is successfully created, the script displays its ARN\. Then the script calls the `attachRolePolicy` method of the IAM service object to attach the managed policy\. When the policy is successfully attached, the script displays a confirmation message\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import a non-modular IAM client
const {
  IAMClient,
  CreateRoleCommand,
  AttachRolePolicyCommand
} = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Instantiate the IAM client
const iam = new IAMClient(REGION);

// Set the parameters
const ROLE = "NEW_ROLENAME"; //NEW_ROLENAME
const myPolicy = {
  Version: "2012-10-17",
  Statement: [
    {
      Effect: "Allow",
      Principal: {
        Service: "lambda.amazonaws.com",
      },
      Action: "sts:AssumeRole",
    },
  ],
};

const createParams = {
  AssumeRolePolicyDocument: JSON.stringify(myPolicy),
  RoleName: ROLE,
};

const lambdaPolicyParams = {
  PolicyArn: "arn:aws:iam::aws:policy/service-role/AWSLambdaRole",
  RoleName: ROLE,
};

const dynamoPolicyParams = {
  PolicyArn: "arn:aws:iam::aws:policy/AmazonDynamoDBReadOnlyAccess",
  RoleName: ROLE,
};

const run = async () => {
  try {
    const data = await iam.send(new CreateRoleCommand(createParams));
    console.log("Role ARN is", data.Role.Arn); // successful response
  } catch (err) {
    console.log("Error when creating role."); // an error occurred
    throw err;
  }
  try {
    await iam.send(new AttachRolePolicyCommand(lambdaPolicyParams));
    console.log("AWSLambdaRole policy attached"); // successful response
  } catch (err) {
    console.log("Error when attaching Lambda policy to role."); // an error occurred
    throw err;
  }
  try {
    await iam.send(new AttachRolePolicyCommand(dynamoPolicyParams));
    console.log("DynamoDB read-only policy attached"); // successful response
  } catch (err) {
    console.log("Error when attaching dynamodb policy to role."); // an error occurred
    throw err;
  }
};

run();
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/lambda/src/lambda-role-setup.ts)\.

Choose **next** to continue the tutorial\.