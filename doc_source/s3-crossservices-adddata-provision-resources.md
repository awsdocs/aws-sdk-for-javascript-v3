--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Create the AWS resources<a name="s3-crossservices-adddata-provision-resources"></a>

This app requires the following resources:
+ AWS Identity and Access Management \(IAM\) Unaunthenticated Amazon Cognito user role with the following permissions:
  + sns:Publish
  + dynamodb:PutItem
+ A DynamoDB table\.

You can create these resources manually in the AWS console, but we recommend provisioning these resources using the AWS CloudFormation \(AWS CloudFormation\) as described in this tutorial\.

## Create the AWS resources using AWS CloudFormation<a name="s3-crossservices-adddata-cli"></a>

AWS CloudFormation enables you to create and provision AWS infrastructure deployments predictably and repeatedly\. For more information about AWS CloudFormation, see the [AWS CloudFormation developer guide\.](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/Welcome.html)\.

To create the AWS CloudFormation stack using the AWS CLI:

1. Install and configure the AWS CLI following the instructions in the [AWS CLI User Guide](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)\.

1. Create a file named `setup.yaml` in the root directory of your project folder, and copy the content [ here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/submit-data-app/setup.yaml) into it\.
**Note**  
The AWS CloudFormation template was generated using the AWS CDK available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/resources/cdk/submit-data-app-unauthenticated-role)\. For more information about the AWS CDK, see the [AWS Cloud Development Kit \(CDK\) Developer Guide](https://docs.aws.amazon.com/cdk/latest/guide/)\.

1. Run the following command from the command line, replacing *STACK\_NAME* with a unique name for the stack, and *REGION* in your AWS region\.
**Important**  
The stack name must be unique within an AWS Region and AWS account\. You can specify up to 128 characters, and numbers and hyphens are allowed\.

   ```
   aws cloudformation create-stack --stack-name STACK_NAME --template-body file://setup.yaml --capabilities CAPABILITY_IAM --region REGION
   ```

   For more information on the `create-stack` command parameters, see the [AWS CLI Command Reference guide](https://docs.aws.amazon.com/cli/latest/reference/cloudformation/create-stack.html), and the [AWS CloudFormation User Guide](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-cli-creating-stack.html)\.

   To view the resources created, open AWS CloudFormation in the AWS management console, choose the stack, and select the **Resources** tab\.

1. When the stack is create, use the AWS SDK for JavaScript to populate the DynamoDB table, as described in [Populating the table](#s3-crossservices-adddata-resources-create-table)\.

## Populating the table<a name="s3-crossservices-adddata-resources-create-table"></a>

To populate the table, first create a diretory named `libs`, and in it create a file named `dynamoClient.js`, and paste the content below into it\. Replace *REGION* with your AWS Region\. This creates the DynamoDB client object\.

```
import { CognitoIdentityClient } from "@aws-sdk/client-cognito-identity";
import { fromCognitoIdentityPool } from "@aws-sdk/credential-provider-cognito-identity";
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";

const REGION = "REGION";
const IDENTITY_POOL_ID = "IDENTITY_POOL_ID"; // An Amazon Cognito Identity Pool ID.

// Create an Amazon DynaomDB service client object.
const dynamoClient = new DynamoDBClient({
  region: REGION,
  credentials: fromCognitoIdentityPool({
    client: new CognitoIdentityClient({ region: REGION }),
    identityPoolId: IDENTITY_POOL_ID,
  }),
});

export { dynamoClient };
```

 This code is available [ here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/submit-data-app/src/libs/dynamoClient.js)\.

Next, create a `dynamoAppHelperFiles` folder in your project folder, create a file `update-table.js` in it, and copy the content [ here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/submit-data-app/src/dynamoAppHelperFiles/update-table.js) into it\. 

```
// Import required AWS SDK clients and commands for Node.js
import { PutItemCommand } from "@aws-sdk/client-dynamodb";
import { dynamoClient } from "../libs/dynamoClient.js";

// Set the parameters
export const params = {
  TableName: "Items",
  Item: {
    id: { N: "1" },
    title: { S: "aTitle" },
    name: { S: "aName" },
    body: { S: "aBody" },
  },
};

export const run = async () => {
  try {
    const data = await dynamoClient.send(new PutItemCommand(params));
    console.log("success");
    console.log(data);
  } catch (err) {
    console.error(err);
  }
};
run();
```

Run the following command from the command line\.

```
node update-table.js
```

 This code is available [ here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/lambda-scheduled-events/src/helper-functions/populate-table.js)\.