# Create a Lambda Execution Role in IAM<a name="using-lambda-iam-role-setup"></a>

This topic is part of a larger tutorial about using the AWS SDK for JavaScript with AWS Lambda functions\. To start at the beginning of the tutorial, see [Tutorial: Creating and Using Lambda Functions](using-lambda-functions.md)\.

In this task, you will focus on creating IAM role used by the application to execute the Lambda function\.

![\[Create an IAM execution role for Lambda\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/create-iam-role.png)![\[Create an IAM execution role for Lambda\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Create an IAM execution role for Lambda\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

A Lambda function requires an execution role created in IAM that provides the function with the necessary permissions to run\. For more information about the Lambda execution role, see [Manage Permissions: Using an IAM Role \(Execution Role\)](https://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html#lambda-intro-execution-role) in the *AWS Lambda Developer Guide*\. 

**To create the Lambda execution role in IAM**

1. Open `lambda-role-setup.js` in the `slotassets` directory in a text editor\.

1. Find this line of code\.

   `RoleName: "ROLE"`

   Replace **ROLE** with another name\. 

1. Save your changes and close the file\. 

1. At the command line, type the following\.

   `node lambda-role-setup.js`

1. Make a note of the ARN returned by the script\. You need this value to create the Lambda function\. 

## Setup Script Code<a name="using-lambda-s3-setup-script"></a>

The following code is the setup script that creates the Lambda execution role\. The setup script creates the JSON that defines the trust relationship needed for a Lambda execution role\. It also creates the JSON parameters for attaching the AWSLambdaRole managed policy\. Then it assigns the string version of the JSON to the parameters for the `createRole` method of the IAM service object\. 

The `createRole` method automatically URL\-encodes the JSON to create the execution role\. When the new role is successfully created, the script displays its ARN\. Then the script calls the `attachRolePolicy` method of the IAM service object to attach the managed policy\. When the policy is successfully attached, the script displays a confirmation message\. 

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Load credentials and set Region from JSON file
AWS.config.loadFromPath('./config.json');

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var myPolicy = {
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": {
        "Service": "lambda.amazonaws.com"
      },
      "Action": "sts:AssumeRole"
    }
  ]
};

var createParams = {
 AssumeRolePolicyDocument: JSON.stringify(myPolicy),
 RoleName: "ROLE"
};

var policyParams = {
 PolicyArn: "arn:aws:iam::policy/service-role/AWSLambdaRole",
 RoleName: "ROLE"
};

iam.createRole(createParams, function(err, data) {
  if (err) {
     console.log(err, err.stack); // an error occurred
  } else {
     console.log("Role ARN is", data.Role.Arn);           // successful response
     iam.attachRolePolicy(policyParams , function(err, data) {
     if (err) {
        console.log(err, err.stack);
     } else {
        console.log("AWSLambdaRole Policy attached");
     }
  });
 }
});
```

Click **next** to continue the tutorial\.