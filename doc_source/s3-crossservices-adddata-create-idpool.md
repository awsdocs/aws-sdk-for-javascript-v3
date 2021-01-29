--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Create an Amazon Cognito identity pool with an unauthenticated identity<a name="s3-crossservices-adddata-create-idpool"></a>

This topic is part of a larger tutorial about using the AWS SDK for JavaScript wit AWS Lambda functions\. To start at the beginning of the tutorial, see [Tutorial: Creating and using Lambda functions](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/sdk-cross-service-example-submitting-data.html)\. 

In this task, you create a Amazon Cognito identity pool, and an unauthenticated AWS Identity and Access Management role\. 

**Note**  
This example demonstrates creating an identity pool with an unauthenticated IAM role using the AWS SDK for JavaScript\. Alternatively, you can create it through the AWS Management Console\. For more information, see the [Amazon S3 getting started guide](https://docs.aws.amazon.com/AmazonS3/latest/gsg/GetStartedWithS3.html)\.

Make sure to configure the SDK as previously shown, including installing the required clients and packages\. In `DynamoDBAppHelperFiles`, create a Node\.js module with the file name `create_cognito_id_pool.ts`\. In `create_cognito_id_pool.ts`, load the `Cognito` and `IAM` client modules\.

Create an object for the parameters for creating the identity pool, replacing *IDENTITY\_POOL\_NAME* with a name\.

Then create an object for the parameters for creating the IAM role\. This includes the JSON for the trust relationship policy that grants unauthenticated users permission to assume the role\. For more information, see [\.]( http://docs.aws.amazon.com/IAM/latest/APIReference/#createPolicy-property) 

Create an `IAM` client service object, and a `Cognito` client service object\. Create a parameters object, specifying the node type to be provisioned, the main username and password for the database instance that's automatically created in the cluster, and the cluster type\.

To create the identity pool, enter the following at the command prompt\.

```
ts-node create_cognito_id_pool.ts
```

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  CognitoIdentity,
  SetIdentityPoolRolesCommand,
  CreateIdentityPoolCommand,
} = require("@aws-sdk/client-cognito-identity");
const { IAMClient, CreateRoleCommand } = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters for creating the identity pool
const createPoolParams = {
  AllowClassicFlow: true,
  AllowUnauthenticatedIdentities: true,
  IdentityPoolName: "IDENTITY_POOL_NAME", //IDENTITY_POOL_NAME
};

// Set the parameters for creating the IAM role
// Define the JSON for the trust relationship
const trustRelationship = {
  Version: "2012-10-17",
  Statement: [
    {
      Effect: "Allow",
      Principal: {
        Federated: "cognito-identity.amazonaws.com",
      },
      Action: "sts:AssumeRoleWithWebIdentity",
      Condition: {
        "ForAnyValue:StringLike": {
          "cognito-identity.amazonaws.com:amr": "unauthenticated",
        },
      },
    },
  ],
};
// Stringify the Amazon IAM role trust relationship
const trustPolicy = JSON.stringify(trustRelationship);

// Set the parameters for attaching the role to the policy
const params = {
  AssumeRolePolicyDocument: trustPolicy,
  Path: "/",
  RoleName: "Cognito_" + createPoolParams.IdentityPoolName + "_UnauthRole",
};

// Create the IAM and Cognito service objects
const iamClient = new IAMClient({});
const CogClient = new CognitoIdentity({});

const run = async () => {
  try {
    // Create the identity pool
    const data = await CogClient.send(
      new CreateIdentityPoolCommand(createPoolParams)
    );
    console.log("Identity pool created", data.IdentityPoolId);
    const newPoolID = data.IdentityPoolId;
    try {
      //create the unauthenticaed IAM role
      const data = await iamClient.send(new CreateRoleCommand(params));
      console.log("Role created", data.Role.Arn);
      const roleARN = data.Role.Arn;
      try {
        // Attach the unauthenticated role to the identity pool
        const attachRoleParams = {
          IdentityPoolId: newPoolID,
          Roles: {
            unauthenticated: roleARN,
          },
        };
        const data = await CogClient.send(
          new SetIdentityPoolRolesCommand(attachRoleParams)
        );
        console.log(
          "Role " +
            params.RoleName +
            " added to identity pool " +
            createPoolParams.IdentityPoolName
        );
      } catch (err) {
        console.log("Error", err);
      }
    } catch (err) {
      console.log("Error", err);
    }
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/submit-data-app/src/dynamoAppHelperFiles/create-cognito-id-pool.ts)\.