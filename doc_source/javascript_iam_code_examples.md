--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# IAM examples using SDK for JavaScript V3<a name="javascript_iam_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for JavaScript V3 with IAM\.

*Actions* are code excerpts that show you how to call individual IAM functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple IAM functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w362aac23b9c21c13)
+ [Scenarios](#w362aac23b9c21c15)

## Actions<a name="w362aac23b9c21c13"></a>

### Attach a policy to a role<a name="iam_AttachRolePolicy_javascript_topic"></a>

The following code example shows how to attach an IAM policy to a role\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Attach the policy\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import {
  ListAttachedRolePoliciesCommand,
  AttachRolePolicyCommand,
} from "@aws-sdk/client-iam";

// Set the parameters.
const ROLENAME = "ROLE_NAME";
const paramsRoleList = { RoleName: ROLENAME }; //ROLE_NAME
export const params = {
  PolicyArn: "arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess",
  RoleName: ROLENAME,
};
export const run = async () => {
  try {
    const data = await iamClient.send(
      new ListAttachedRolePoliciesCommand(paramsRoleList)
    );
    return data;
    const myRolePolicies = data.AttachedPolicies;
    myRolePolicies.forEach(function (val, index, array) {
      if (myRolePolicies[index].PolicyName === "AmazonDynamoDBFullAccess") {
        console.log(
          "AmazonDynamoDBFullAccess is already attached to this role."
        );
        process.exit();
      }
    });
    try {
      const data = await iamClient.send(new AttachRolePolicyCommand(params));
      console.log("Role attached successfully");
      return data;
    } catch (err) {
      console.log("Error", err);
    }
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-policies.html#iam-examples-policies-attaching-role-policy)\. 
+  For API details, see [AttachRolePolicy](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/attachrolepolicycommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var paramsRoleList = {
  RoleName: process.argv[2]
};

iam.listAttachedRolePolicies(paramsRoleList, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    var myRolePolicies = data.AttachedPolicies;
    myRolePolicies.forEach(function (val, index, array) {
      if (myRolePolicies[index].PolicyName === 'AmazonDynamoDBFullAccess') {
        console.log("AmazonDynamoDBFullAccess is already attached to this role.")
        process.exit();
      }
    });
    var params = {
      PolicyArn: 'arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess',
      RoleName: process.argv[2]
    };
    iam.attachRolePolicy(params, function(err, data) {
      if (err) {
        console.log("Unable to attach policy to role", err);
      } else {
        console.log("Role attached successfully");
      }
    });
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/iam-examples-policies.html#iam-examples-policies-attaching-role-policy)\. 
+  For API details, see [AttachRolePolicy](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/AttachRolePolicy) in *AWS SDK for JavaScript API Reference*\. 

### Create a policy<a name="iam_CreatePolicy_javascript_topic"></a>

The following code example shows how to create an IAM policy\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Create the policy\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { CreatePolicyCommand } from "@aws-sdk/client-iam";

// Set the parameters.
const myManagedPolicy = {
  Version: "2012-10-17",
  Statement: [
    {
      Effect: "Allow",
      Action: "logs:CreateLogGroup",
      Resource: "RESOURCE_ARN", // RESOURCE_ARN
    },
    {
      Effect: "Allow",
      Action: [
        "dynamodb:DeleteItem",
        "dynamodb:GetItem",
        "dynamodb:PutItem",
        "dynamodb:Scan",
        "dynamodb:UpdateItem",
      ],
      Resource: "DYNAMODB_POLICY_NAME", // DYNAMODB_POLICY_NAME; For example, "myDynamoDBName".
    },
  ],
};
export const params = {
  PolicyDocument: JSON.stringify(myManagedPolicy),
  PolicyName: "IAM_POLICY_NAME",
};

export const run = async () => {
  try {
    const data = await iamClient.send(new CreatePolicyCommand(params));
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-policies.html#iam-examples-policies-creating)\. 
+  For API details, see [CreatePolicy](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/createpolicycommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var myManagedPolicy = {
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "logs:CreateLogGroup",
            "Resource": "RESOURCE_ARN"
        },
        {
            "Effect": "Allow",
            "Action": [
                "dynamodb:DeleteItem",
                "dynamodb:GetItem",
                "dynamodb:PutItem",
                "dynamodb:Scan",
                "dynamodb:UpdateItem"
            ],
            "Resource": "RESOURCE_ARN"
        }
    ]
};

var params = {
  PolicyDocument: JSON.stringify(myManagedPolicy),
  PolicyName: 'myDynamoDBPolicy',
};

iam.createPolicy(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/iam-examples-policies.html#iam-examples-policies-creating)\. 
+  For API details, see [CreatePolicy](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/CreatePolicy) in *AWS SDK for JavaScript API Reference*\. 

### Create a role<a name="iam_CreateRole_javascript_topic"></a>

The following code example shows how to create an IAM role\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Create the role\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { CreateRoleCommand } from "@aws-sdk/client-iam";

// Sample assume role policy JSON.
const role_json = {
    Version: "2012-10-17",
    Statement: [
        {
            Effect: "Allow",
            Principal: {
                AWS: "USER_ARN", // The ARN of the user.
            },
            Action: "sts:AssumeRole",
        },
    ],
};
// Stringify the assume role policy JSON.
const myJson = JSON.stringify(role_json);

// Set the parameters.
const params = {
    AssumeRolePolicyDocument: myJson,
    Path: "/",
    RoleName: "ROLE_NAME"
};

const run = async () => {
    try {
        const data = await iamClient.send(new CreateRoleCommand(params));
        console.log("Success. Role created. Role Arn: ", data.Role.RoleName);
        } catch (err) {
            console.log("Error", err);
        }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For API details, see [CreateRole](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/createrolecommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create a service\-linked role<a name="iam_CreateServiceLinkedRole_javascript_topic"></a>

The following code example shows how to create an IAM service\-linked role\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Create a service\-linked role\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { CreateServiceLinkedRoleCommand } from "@aws-sdk/client-iam";
// Set the parameters.
const params = {
  AWSServiceName: "AWS_SERVICE_NAME" /* required */,
};

const run = async () => {
  try {
    const data = await iamClient.send(
      new CreateServiceLinkedRoleCommand(params)
    );
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For API details, see [CreateServiceLinkedRole](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/createservicelinkedrolecommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create a user<a name="iam_CreateUser_javascript_topic"></a>

The following code example shows how to create an IAM user\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Create the user\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { GetUserCommand, CreateUserCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = { UserName: "USER_NAME" }; //USER_NAME

export const run = async () => {
  try {
    const data = await iamClient.send(new GetUserCommand(params));
    console.log(
      "User " + "USER_NAME" + " already exists",
      data.User.UserId
    );
    return data;
  } catch (err) {
    try {
      const results = await iamClient.send(new CreateUserCommand(params));
      console.log("Success", results);
      return results;
    } catch (err) {
      console.log("Error", err);
    }
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-managing-users.html#iam-examples-managing-users-creating-users)\. 
+  For API details, see [CreateUser](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/createusercommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var params = {
  UserName: process.argv[2]
};

iam.getUser(params, function(err, data) {
  if (err && err.code === 'NoSuchEntity') {
    iam.createUser(params, function(err, data) {
      if (err) {
        console.log("Error", err);
      } else {
        console.log("Success", data);
      }
    });
  } else {
    console.log("User " + process.argv[2] + " already exists", data.User.UserId);
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/iam-examples-managing-users.html#iam-examples-managing-users-creating-users)\. 
+  For API details, see [CreateUser](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/CreateUser) in *AWS SDK for JavaScript API Reference*\. 

### Create an access key<a name="iam_CreateAccessKey_javascript_topic"></a>

The following code example shows how to create an IAM access key\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Create the access key\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { CreateAccessKeyCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = {UserName: "IAM_USER_NAME"}; //IAM_USER_NAME

export const run = async () => {
  try {
    const data = await iamClient.send(new CreateAccessKeyCommand(params));
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-managing-access-keys.html#iam-examples-managing-access-keys-creating)\. 
+  For API details, see [CreateAccessKey](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/createaccesskeycommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

iam.createAccessKey({UserName: 'IAM_USER_NAME'}, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.AccessKey);
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/iam-examples-managing-access-keys.html#iam-examples-managing-access-keys-creating)\. 
+  For API details, see [CreateAccessKey](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/CreateAccessKey) in *AWS SDK for JavaScript API Reference*\. 

### Create an alias for an account<a name="iam_CreateAccountAlias_javascript_topic"></a>

The following code example shows how to create an alias for an IAM account\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Create the account alias\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { CreateAccountAliasCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = { AccountAlias: "ACCOUNT_ALIAS" }; //ACCOUNT_ALIAS

export const run = async () => {
  try {
    const data = await iamClient.send(new CreateAccountAliasCommand(params));
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-account-aliases.html#iam-examples-account-aliases-creating)\. 
+  For API details, see [CreateAccountAlias](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/createaccountaliascommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

iam.createAccountAlias({AccountAlias: process.argv[2]}, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/iam-examples-account-aliases.html#iam-examples-account-aliases-creating)\. 
+  For API details, see [CreateAccountAlias](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/CreateAccountAlias) in *AWS SDK for JavaScript API Reference*\. 

### Delete a policy<a name="iam_DeletePolicy_javascript_topic"></a>

The following code example shows how to delete an IAM policy\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Delete the policy\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { DeletePolicyCommand } from "@aws-sdk/client-iam";

// Set the parameters.
const params = { PolicyArn: "POLICY_ARN"};

const run = async () => {
    try {
        const data = await iamClient.send(new DeletePolicyCommand(params));
        console.log("Success. Policy deleted.", data);

    } catch (err) {
        console.log("Error", err);
    }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam/#code-examples)\. 
+  For API details, see [DeletePolicy](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/deletepolicycommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Delete a role<a name="iam_DeleteRole_javascript_topic"></a>

The following code example shows how to delete an IAM role\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Delete the role\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { DeleteRoleCommand } from "@aws-sdk/client-iam";

// Set the parameters.
const params = {
    RoleName: "ROLE_NAME"
}

const run = async () => {
    try {
        const data = await iamClient.send(new DeleteRoleCommand(params));
        console.log("Success. Role deleted.", data);
    } catch (err) {
        console.log("Error", err);
    }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For API details, see [DeleteRole](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/deleterolecommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Delete a server certificate<a name="iam_DeleteServerCertificate_javascript_topic"></a>

The following code example shows how to delete an IAM server certificate\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Delete a server certificate\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { DeleteServerCertificateCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = { ServerCertificateName: "CERTIFICATE_NAME" }; // CERTIFICATE_NAME

export const run = async () => {
  try {
    const data = await iamClient.send(
      new DeleteServerCertificateCommand(params)
    );
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-server-certificates.html#iam-examples-server-certificates-deleting)\. 
+  For API details, see [DeleteServerCertificate](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/deleteservercertificatecommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

iam.deleteServerCertificate({ServerCertificateName: 'CERTIFICATE_NAME'}, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/iam-examples-server-certificates.html#iam-examples-server-certificates-deleting)\. 
+  For API details, see [DeleteServerCertificate](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/DeleteServerCertificate) in *AWS SDK for JavaScript API Reference*\. 

### Delete a user<a name="iam_DeleteUser_javascript_topic"></a>

The following code example shows how to delete an IAM user\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Delete the user\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { DeleteUserCommand, GetUserCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = { UserName: "USER_NAME" }; //USER_NAME

export const run = async () => {
  try {
    const data = await iamClient.send(new GetUserCommand(params));
    return data;
    try {
      const results = await iamClient.send(new DeleteUserCommand(params));
      console.log("Success", results);
      return results;
    } catch (err) {
      console.log("Error", err);
    }
  } catch (err) {
    console.log("User " + "USER_NAME" + " does not exist.");
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-managing-users.html#iam-examples-managing-users-deleting-users)\. 
+  For API details, see [DeleteUser](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/deleteusercommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var params = {
  UserName: process.argv[2]
};

iam.getUser(params, function(err, data) {
  if (err && err.code === 'NoSuchEntity') {
    console.log("User " + process.argv[2] + " does not exist.");
  } else {
    iam.deleteUser(params, function(err, data) {
      if (err) {
        console.log("Error", err);
      } else {
        console.log("Success", data);
      }
    });
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/23/developer-guide/iam-examples-managing-users.html#iam-examples-managing-users-deleting-users)\. 
+  For API details, see [DeleteUser](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/DeleteUser) in *AWS SDK for JavaScript API Reference*\. 

### Delete an access key<a name="iam_DeleteAccessKey_javascript_topic"></a>

The following code example shows how to delete an IAM access key\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Delete the access key\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { DeleteAccessKeyCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = {
  AccessKeyId: "ACCESS_KEY_ID", // ACCESS_KEY_ID
  UserName: "USER_NAME", // USER_NAME
};

export const run = async () => {
  try {
    const data = await iamClient.send(new DeleteAccessKeyCommand(params));
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-managing-access-keys.html#iam-examples-managing-access-keys-deleting)\. 
+  For API details, see [DeleteAccessKey](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/deleteaccesskeycommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var params = {
  AccessKeyId: 'ACCESS_KEY_ID',
  UserName: 'USER_NAME'
};

iam.deleteAccessKey(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/iam-examples-managing-access-keys.html#iam-examples-managing-access-keys-deleting)\. 
+  For API details, see [DeleteAccessKey](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/DeleteAccessKey) in *AWS SDK for JavaScript API Reference*\. 

### Delete an account alias<a name="iam_DeleteAccountAlias_javascript_topic"></a>

The following code example shows how to delete an IAM account alias\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Delete the account alias\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { DeleteAccountAliasCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = { AccountAlias: "ALIAS" }; // ALIAS

export const run = async () => {
  try {
    const data = await iamClient.send(new DeleteAccountAliasCommand(params));
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-account-aliases.html#iam-examples-account-aliases-deleting)\. 
+  For API details, see [DeleteAccountAlias](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/deleteaccountaliascommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

iam.deleteAccountAlias({AccountAlias: process.argv[2]}, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/iam-examples-account-aliases.html#iam-examples-account-aliases-deleting)\. 
+  For API details, see [DeleteAccountAlias](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/DeleteAccountAlias) in *AWS SDK for JavaScript API Reference*\. 

### Detach a policy from a role<a name="iam_DetachRolePolicy_javascript_topic"></a>

The following code example shows how to detach an IAM policy from a role\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Detach the policy\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import {
  ListAttachedRolePoliciesCommand,
  DetachRolePolicyCommand,
} from "@aws-sdk/client-iam";

// Set the parameters.
export const params = { RoleName: "ROLE_NAME" }; //ROLE_NAME

export const run = async () => {
  try {
    const data = await iamClient.send(
      new ListAttachedRolePoliciesCommand(params)
    );
    return data;
    const myRolePolicies = data.AttachedPolicies;
    myRolePolicies.forEach(function (val, index, array) {
      if (myRolePolicies[index].PolicyName === "AmazonDynamoDBFullAccess") {
         const params = {
          PolicyArn: "arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess",
          paramsRoleList,
        };
        try {
          const results = iamClient.send(
            new DetachRolePolicyCommand(paramsRoleList)
          );
          console.log("Policy detached from role successfully");
          process.exit();
        } catch (err) {
          console.log("Unable to detach policy from role", err);
        }
      } else {
      }
    });
  } catch (err) {
    console.log("User " + "USER_NAME" + " does not exist.");
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-policies.html#iam-examples-policies-detaching-role-policy)\. 
+  For API details, see [DetachRolePolicy](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/detachrolepolicycommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var paramsRoleList = {
  RoleName: process.argv[2]
};

iam.listAttachedRolePolicies(paramsRoleList, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    var myRolePolicies = data.AttachedPolicies;
    myRolePolicies.forEach(function (val, index, array) {
      if (myRolePolicies[index].PolicyName === 'AmazonDynamoDBFullAccess') {
        var params = {
          PolicyArn: 'arn:aws:iam::aws:policy/AmazonDynamoDBFullAccess',
          RoleName: process.argv[2]
        };
        iam.detachRolePolicy(params, function(err, data) {
          if (err) {
            console.log("Unable to detach policy from role", err);
          } else {
            console.log("Policy detached from role successfully");
            process.exit();
          }
        });
      }
    });
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/iam-examples-policies.html#iam-examples-policies-detaching-role-policy)\. 
+  For API details, see [DetachRolePolicy](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/DetachRolePolicy) in *AWS SDK for JavaScript API Reference*\. 

### Get a policy<a name="iam_GetPolicy_javascript_topic"></a>

The following code example shows how to get an IAM policy\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Get the policy\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { GetPolicyCommand } from "@aws-sdk/client-iam";

// Set the parameters.
const params = {
  PolicyArn: "POLICY_ARN" /* required */,
};

const run = async () => {
  try {
    const data = await iamClient.send(new GetPolicyCommand(params));
    console.log("Success", data.Policy);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-policies.html#iam-examples-policies-getting)\. 
+  For API details, see [GetPolicy](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/getpolicycommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var params = {
  PolicyArn: 'arn:aws:iam::aws:policy/AWSLambdaExecute'
};

iam.getPolicy(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.Policy.Description);
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/iam-examples-policies.html#iam-examples-policies-getting)\. 
+  For API details, see [GetPolicy](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/GetPolicy) in *AWS SDK for JavaScript API Reference*\. 

### Get a role<a name="iam_GetRole_javascript_topic"></a>

The following code example shows how to get an IAM role\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Get the role\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { GetRoleCommand } from "@aws-sdk/client-iam";

// Set the parameters.
const params = {
  RoleName: "ROLE_NAME" /* required */
};

const run = async () => {
  try {
    const data = await iamClient.send(new GetRoleCommand(params));
    console.log("Success", data.Role);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For API details, see [GetRole](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/getrolecommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Get a server certificate<a name="iam_GetServerCertificate_javascript_topic"></a>

The following code example shows how to get an IAM server certificate\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Get a server certificate\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { GetServerCertificateCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = { ServerCertificateName: "CERTIFICATE_NAME" }; //CERTIFICATE_NAME

export const run = async () => {
  try {
    const data = await iamClient.send(new GetServerCertificateCommand(params));
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-server-certificates.html#iam-examples-server-certificates-getting)\. 
+  For API details, see [GetServerCertificate](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/getservercertificatecommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

iam.getServerCertificate({ServerCertificateName: 'CERTIFICATE_NAME'}, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/iam-examples-server-certificates.html#iam-examples-server-certificates-getting)\. 
+  For API details, see [GetServerCertificate](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/GetServerCertificate) in *AWS SDK for JavaScript API Reference*\. 

### Get data about the last use of an access key<a name="iam_GetAccessKeyLastUsed_javascript_topic"></a>

The following code example shows how to get data about the last use of an IAM access key\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Get the access key\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { GetAccessKeyLastUsedCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = { AccessKeyId: "ACCESS_KEY_ID" }; //ACCESS_KEY_ID

export const run = async () => {
  try {
    const data = await iamClient.send(new GetAccessKeyLastUsedCommand(params));
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-managing-access-keys.html#iam-examples-managing-access-keys-last-used)\. 
+  For API details, see [GetAccessKeyLastUsed](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/getaccesskeylastusedcommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

iam.getAccessKeyLastUsed({AccessKeyId: 'ACCESS_KEY_ID'}, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.AccessKeyLastUsed);
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/iam-examples-managing-access-keys.html#iam-examples-managing-access-keys-last-used)\. 
+  For API details, see [GetAccessKeyLastUsed](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/GetAccessKeyLastUsed) in *AWS SDK for JavaScript API Reference*\. 

### Get the account password policy<a name="iam_GetAccountPasswordPolicy_javascript_topic"></a>

The following code example shows how to get the IAM account password policy\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Get the account password policy\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { GetAccountPasswordPolicyCommand } from "@aws-sdk/client-iam";

const run = async () => {
  try {
    const data = await iamClient.send(new GetAccountPasswordPolicyCommand({}));
    console.log("Success", data.PasswordPolicy);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For API details, see [GetAccountPasswordPolicy](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/getaccountpasswordpolicycommand.html) in *AWS SDK for JavaScript API Reference*\. 

### List SAML providers<a name="iam_ListSAMLProviders_javascript_topic"></a>

The following code example shows how to list SAML providers for IAM\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
List the SAML providers\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import {ListSAMLProvidersCommand} from "@aws-sdk/client-iam";

export const run = async () => {
    try {
        const results = await iamClient.send(new ListSAMLProvidersCommand({}));
        console.log("Success", results);
        return results;
    } catch (err) {
        console.log("Error", err);
    }
}
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For API details, see [ListSAMLProviders](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/listsamlproviderscommand.html) in *AWS SDK for JavaScript API Reference*\. 

### List a user's access keys<a name="iam_ListAccessKeys_javascript_topic"></a>

The following code example shows how to list a user's IAM access keys\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
List the access keys\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { ListAccessKeysCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = {
  MaxItems: 5,
  UserName: "IAM_USER_NAME", //IAM_USER_NAME
};

export const run = async () => {
  try {
    const data = await iamClient.send(new ListAccessKeysCommand(params));
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-managing-access-keys.html#iam-examples-managing-access-keys-listing)\. 
+  For API details, see [ListAccessKeys](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/listaccesskeyscommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var params = {
  MaxItems: 5,
  UserName: 'IAM_USER_NAME'
};

iam.listAccessKeys(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/iam-examples-managing-access-keys.html#iiam-examples-managing-access-keys-listing)\. 
+  For API details, see [ListAccessKeys](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/ListAccessKeys) in *AWS SDK for JavaScript API Reference*\. 

### List account aliases<a name="iam_ListAccountAliases_javascript_topic"></a>

The following code example shows how to list IAM account aliases\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
List the account aliases\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { ListAccountAliasesCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = { MaxItems: 5 };

export const run = async () => {
  try {
    const data = await iamClient.send(new ListAccountAliasesCommand(params));
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-account-aliases.html#iam-examples-account-aliases-listing)\. 
+  For API details, see [ListAccountAliases](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/listaccountaliasescommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

iam.listAccountAliases({MaxItems: 10}, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/iam-examples-account-aliases.html4#iam-examples-account-aliases-listing)\. 
+  For API details, see [ListAccountAliases](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/ListAccountAliases) in *AWS SDK for JavaScript API Reference*\. 

### List groups<a name="iam_ListGroups_javascript_topic"></a>

The following code example shows how to list IAM groups\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
List the groups\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import {ListGroupsCommand} from "@aws-sdk/client-iam";

// Set the parameters.
export const params = {
    RoleName: 'ROLE_NAME', /* This is a number value. Required */
    Marker: 'MARKER', /* This is a string value. Optional */
    MaxItems: 'MAX_ITEMS' /* This is a number value. Optional */
};

export const run = async () => {
    try {
        const data = await iamClient.send(new ListGroupsCommand({}));
        console.log("Success", data.Groups);
    } catch (err) {
        console.log("Error", err);
    }
}
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For API details, see [ListGroups](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/listgroupscommand.html) in *AWS SDK for JavaScript API Reference*\. 

### List inline policies for a role<a name="iam_ListRolePolicies_javascript_topic"></a>

The following code example shows how to list inline policies for an IAM role\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
List the policies\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import {ListRolePoliciesCommand} from "@aws-sdk/client-iam";

// Set the parameters.
export const params = {
    RoleName: 'ROLE_NAME', /* This is a number value. Required */
    Marker: 'MARKER', /* This is a string value. Optional */
    MaxItems: 'MAX_ITEMS' /* This is a number value. Optional */
};

export const run = async () => {
    try {
        const results = await iamClient.send(new ListRolePoliciesCommand(params));
        console.log("Success", results);
        return results;
    } catch (err) {
        console.log("Error", err);
    }
}
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For API details, see [ListRolePolicies](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/listrolepoliciescommand.html) in *AWS SDK for JavaScript API Reference*\. 

### List policies<a name="iam_ListPolicies_javascript_topic"></a>

The following code example shows how to list IAM policies\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
List the policies\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import {ListPoliciesCommand} from "@aws-sdk/client-iam";

// Set the parameters.
export const params = {
    Marker: 'MARKER',
    MaxItems: 'MAX_ITEMS',
    OnlyAttached: "ONLY_ATTACHED", /* Options are "true" or "false"*/
    PathPrefix: 'PATH_PREFIX',
    PolicyUsageFilter: "POLICY_USAGE_FILTER", /* Options are "PermissionsPolicy" or "PermissionsBoundary"*/
    Scope: "SCOPE" /* Options are "All", "AWS", "Local"*/
};

export const run = async () => {
    try {
        const results = await iamClient.send(new ListPoliciesCommand(params));
        console.log("Success", results);
        return results;
    } catch (err) {
        console.log("Error", err);
    }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For API details, see [ListPolicies](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/listpoliciescommand.html) in *AWS SDK for JavaScript API Reference*\. 

### List policies attached to a role<a name="iam_ListAttachedRolePolicies_javascript_topic"></a>

The following code example shows how to list policies attached to an IAM role\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
List the policies that are attached to a role\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import {ListAttachedRolePoliciesCommand} from "@aws-sdk/client-iam";

// Set the parameters.
export const params = {
    RoleName: 'ROLE_NAME' /* required */
};

export const run = async () => {
    try {
        const data = await iamClient.send(new ListAttachedRolePoliciesCommand(params));
        console.log("Success", data.AttachedPolicies);
    } catch (err) {
        console.log("Error", err);
    }
}
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For API details, see [ListAttachedRolePolicies](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/listattachedrolepoliciescommand.html) in *AWS SDK for JavaScript API Reference*\. 

### List roles<a name="iam_ListRoles_javascript_topic"></a>

The following code example shows how to list IAM roles\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
List the roles\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { ListRolesCommand } from "@aws-sdk/client-iam";

// Set the parameters.
const params = {
    Marker: 'MARKER', // This is a string value.
    MaxItems: 'MAX_ITEMS' // This is a number value.
};

const run = async () => {
        try {
            const results = await iamClient.send(new ListRolesCommand(params));
            console.log("Success", results);
            return results;
        } catch (err) {
            console.log("Error", err);
        }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For API details, see [ListRoles](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/listrolescommand.html) in *AWS SDK for JavaScript API Reference*\. 

### List server certificates<a name="iam_ListServerCertificates_javascript_topic"></a>

The following code example shows how to list IAM server certificates\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
List the certificates\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { ListServerCertificatesCommand } from "@aws-sdk/client-iam";

export const run = async () => {
  try {
    const data = await iamClient.send(new ListServerCertificatesCommand({}));
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-server-certificates.html#iam-examples-server-certificates-listing)\. 
+  For API details, see [ListServerCertificates](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/listservercertificatescommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

iam.listServerCertificates({}, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/iam-examples-server-certificates.html#iam-examples-server-certificates-listing)\. 
+  For API details, see [ListServerCertificates](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/ListServerCertificates) in *AWS SDK for JavaScript API Reference*\. 

### List users<a name="iam_ListUsers_javascript_topic"></a>

The following code example shows how to list IAM users\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
List the users\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { ListUsersCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = { MaxItems: 10 };

export const run = async () => {
  try {
    const data = await iamClient.send(new ListUsersCommand(params));
    return data;
    const users = data.Users || [];
    users.forEach(function (user) {
      console.log("User " + user.UserName + " created", user.CreateDate);
    });
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-managing-users.html#iam-examples-managing-users-listing-users)\. 
+  For API details, see [ListUsers](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/listuserscommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var params = {
  MaxItems: 10
};

iam.listUsers(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    var users = data.Users || [];
    users.forEach(function(user) {
      console.log("User " + user.UserName + " created", user.CreateDate);
    });
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/iam-examples-managing-users.html#iam-examples-managing-users-listing-users)\. 
+  For API details, see [ListUsers](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/ListUsers) in *AWS SDK for JavaScript API Reference*\. 

### Update a server certificate<a name="iam_UpdateServerCertificate_javascript_topic"></a>

The following code example shows how to update an IAM server certificate\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Update a server certificate\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { UpdateServerCertificateCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = {
  ServerCertificateName: "CERTIFICATE_NAME", //CERTIFICATE_NAME
  NewServerCertificateName: "NEW_CERTIFICATE_NAME", //NEW_CERTIFICATE_NAME
};

export const run = async () => {
  try {
    const data = await iamClient.send(
      new UpdateServerCertificateCommand(params)
    );
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-server-certificates.html#iam-examples-server-certificates-updating)\. 
+  For API details, see [UpdateServerCertificate](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/updateservercertificatecommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var params = {
  ServerCertificateName: 'CERTIFICATE_NAME',
  NewServerCertificateName: 'NEW_CERTIFICATE_NAME'
};

iam.updateServerCertificate(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/iam-examples-server-certificates.html#iam-examples-server-certificates-updating)\. 
+  For API details, see [UpdateServerCertificate](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/UpdateServerCertificate) in *AWS SDK for JavaScript API Reference*\. 

### Update a user<a name="iam_UpdateUser_javascript_topic"></a>

The following code example shows how to update an IAM user\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Update the user\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { UpdateUserCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = {
  UserName: "ORIGINAL_USER_NAME", //ORIGINAL_USER_NAME
  NewUserName: "NEW_USER_NAME", //NEW_USER_NAME
};

export const run = async () => {
  try {
    const data = await iamClient.send(new UpdateUserCommand(params));
    console.log("Success, username updated");
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-managing-users.html#iam-examples-managing-users-updating-users)\. 
+  For API details, see [UpdateUser](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/updateusercommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var params = {
  UserName: process.argv[2],
  NewUserName: process.argv[3]
};

iam.updateUser(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/23/developer-guide/iam-examples-managing-users.html#iam-examples-managing-users-updating-users)\. 
+  For API details, see [UpdateUser](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/UpdateUser) in *AWS SDK for JavaScript API Reference*\. 

### Update an access key<a name="iam_UpdateAccessKey_javascript_topic"></a>

The following code example shows how to update an IAM access key\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```
Update the access key\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { UpdateAccessKeyCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = {
  AccessKeyId: "ACCESS_KEY_ID", //ACCESS_KEY_ID
  Status: "Active",
  UserName: "USER_NAME", //USER_NAME
};

export const run = async () => {
  try {
    const data = await iamClient.send(new UpdateAccessKeyCommand(params));
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/iam-examples-managing-access-keys.html#iam-examples-managing-access-keys-updating)\. 
+  For API details, see [UpdateAccessKey](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/updateaccesskeycommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var params = {
  AccessKeyId: 'ACCESS_KEY_ID',
  Status: 'Active',
  UserName: 'USER_NAME'
};

iam.updateAccessKey(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/iam#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/iam-examples-managing-access-keys.html#iam-examples-managing-access-keys-updating)\. 
+  For API details, see [UpdateAccessKey](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/iam-2010-05-08/UpdateAccessKey) in *AWS SDK for JavaScript API Reference*\. 

## Scenarios<a name="w362aac23b9c21c15"></a>

### Create a user and assume a role<a name="iam_Scenario_CreateUserAssumeRole_javascript_topic"></a>

The following code example shows how to:
+ Create a user who has no permissions\.
+ Create a role that grants permission to list Amazon S3 buckets for the account\.
+ Add a policy to let the user assume the role\.
+ Assume the role and list Amazon S3 buckets using temporary credentials\.
+ Delete the policy, role, and user\.

**SDK for JavaScript V3**  
Create the client\.  

```
// Create service client module using ES6 syntax.
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
export const REGION = "REGION"; // For example, "us-east-1".
// Create an Amazon S3 service client object.
export const iamClient = new IAMClient({ region: REGION });
```
Create an IAM user and a role that grants permission to list Amazon S3 buckets\. The user has rights only to assume the role\. After assuming the role, use temporary credentials to list buckets for the account\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient, REGION } from "../libs/iamClient.js"; // Helper function that creates an IAM service client module.
import {
  CreateUserCommand,
  CreateAccessKeyCommand,
  CreatePolicyCommand,
  CreateRoleCommand,
  AttachRolePolicyCommand,
  AttachUserPolicyCommand,
  DeleteAccessKeyCommand,
  DeleteUserCommand,
  DeleteRoleCommand,
  DeletePolicyCommand,
  DetachUserPolicyCommand,
  DetachRolePolicyCommand,
} from "@aws-sdk/client-iam";
import { ListBucketsCommand, S3Client } from "@aws-sdk/client-s3";
import { AssumeRoleCommand, STSClient } from "@aws-sdk/client-sts";

if (process.argv.length < 6) {
  console.log(
      "Usage: node iam_basics.js <user name> <s3 policy name> <role name> <assume policy name>\n" +
      "Example: node iam_basics.js test-user my-s3-policy my-iam-role my-assume-role"
  );
}
// Set the parameters.
const region = REGION;
const userName = process.argv[2];
const s3_policy_name = process.argv[3];
const role_name = process.argv[4];
const assume_policy_name = process.argv[5];

// Helper function to delay running the code while the AWS service calls wait for responses.
function wait(ms) {
  var start = Date.now();
  var end = start
  while (end < start + ms){
    end = Date.now()
  }
}

export const run = async (
    userName,
    s3_policy_name,
    role_name,
    assume_policy_name
) => {
  try {
    // Create a new user.
    const user_params = { UserName: userName };
    console.log("\nCreating a user name " + user_params.UserName + "...\n");
    const data = await iamClient.send(
        new CreateUserCommand({ UserName: userName })
    );
    const user_arn = data.User.Arn;
    const user_name = data.User.UserName;
    console.log(
        "User with name" + user_name + " and ARN " + user_arn + " created."
    );
    try {
      // Create access keys for the new user.
      console.log(
          "\nCreating access keys for " + user_params.UserName + "...\n"
      );
      const access_key_params = { UserName: user_name };
      const data = await iamClient.send(
          new CreateAccessKeyCommand(access_key_params)
      );
      console.log("Success. Access key created: ", data.AccessKey.AccessKeyId);
      var myAccessKey = data.AccessKey.AccessKeyId;
      var mySecretAccessKey = data.AccessKey.SecretAccessKey;

      try {
        // Attempt to list S3 buckets.
        console.log(
            "\nWaiting 10 seconds for user and access keys to be created...\n"
        );
        wait(10000);
        console.log(
            "Attempt to list S3 buckets with the new user (without permissions)...\n"
        );
        // Use the credentials for the new user that you created.
        var user_creds = {
          accessKeyId: myAccessKey,
          secretAccessKey: mySecretAccessKey,
        };
        const s3Client = new S3Client({
          credentials: user_creds,
          region: region,
        });
        await s3Client.send(new ListBucketsCommand({}));
      } catch (err) {
        console.log(
            "Error. As expected the new user has no permissions to list buckets. ",
            err.stack
        );
        console.log(
            "\nCreating policy to allow the new user to list all buckets, and to assume an STS role...\n"
        );
        const myManagedPolicy = {
          Version: "2012-10-17",
          Statement: [
            {
              Effect: "Allow",
              Action: ["s3:ListAllMyBuckets", "sts:AssumeRole"],
              Resource: "*",
            },
          ],
        };
        const policy_params = {
          PolicyDocument: JSON.stringify(myManagedPolicy),
          PolicyName: s3_policy_name, // Name of the new policy.
        };
        const data = await iamClient.send(
            new CreatePolicyCommand(policy_params)
        );
        console.log(
            "Success. Policy created that allows listing of all S3 buckets.\n" +
            "Policy ARN: " +
            data.Policy.Arn +
            "\n" +
            "Policy name: " +
            data.Policy.PolicyName +
            "\n"
        );

        var s3_policy_arn = data.Policy.Arn;

        try {
          console.log(
              "\nCreating a role with a trust policy that lets the user assume the role....\n"
          );

          const role_json = {
            Version: "2012-10-17",
            Statement: [
              {
                Effect: "Allow",
                Principal: {
                  AWS: user_arn, // The ARN of the user.
                },
                Action: "sts:AssumeRole",
              },
            ],
          };
          const myJson = JSON.stringify(role_json);

          const role_params = {
            AssumeRolePolicyDocument: myJson, // Trust relationship policy document.
            Path: "/",
            RoleName: role_name // The name of the new role.
          };
          const data = await iamClient.send(new CreateRoleCommand(role_params));
          console.log("Success. Role created. Role Arn: ", data.Role.Arn);
          const role_arn = data.Role.Arn;
          try {
            console.log(
                "\nAttaching to the role the policy with permissions to list all buckets....\n"
            );
            const params = {
              PolicyArn: s3_policy_arn,
              RoleName: role_name,
            };
            await iamClient.send(new AttachRolePolicyCommand(params));
            console.log("Success. Policy attached successfully to role.");
            try {
              console.log(
                  "\nCreate a policy that enables the user to assume the role ....\n"
              );
              const myNewPolicy = {
                Version: "2012-10-17",
                Statement: [
                  {
                    Effect: "Allow",
                    Action: ["sts:AssumeRole"],
                    Resource: role_arn,
                  },
                ],
              };
              const policy_params = {
                PolicyDocument: JSON.stringify(myNewPolicy),
                PolicyName: assume_policy_name,
              };
              const data = await iamClient.send(
                  new CreatePolicyCommand(policy_params)
              );
              console.log(
                  "Success. Policy created. Policy ARN: " + data.Policy.Arn
              );

              const assume_policy_arn = data.Policy.Arn;
              try {
                console.log("\nAttaching the policy to the user.....\n");
                const attach_policy_to_user_params = {
                  PolicyArn: assume_policy_arn,
                  UserName: user_name,
                };
                const data = await iamClient.send(
                    new AttachUserPolicyCommand(attach_policy_to_user_params)
                );
                console.log(
                    "\nWaiting 10 seconds for policy to be attached...\n"
                );
                wait(10000);
                console.log(
                    "Success. Policy attached to user " + user_name + "."
                );
                try {
                  console.log(
                      "\nAssume for the user the role with permission to list all buckets....\n"
                  );
                  const assume_role_params = {
                    RoleArn: role_arn, //ARN_OF_ROLE_TO_ASSUME
                    RoleSessionName: "session1",
                    DurationSeconds: 900,
                  };
                  // Create an AWS STS client with the credentials for the user. Remember, the user has permissions to assume roles using AWS STS.
                  const stsClientWithUsersCreds = new STSClient({
                    credentials: user_creds,
                    region: REGION,
                  });

                  const data = await stsClientWithUsersCreds.send(
                      new AssumeRoleCommand(assume_role_params)
                  );
                  console.log(
                      "Success assuming role. Access key id is " +
                      data.Credentials.AccessKeyId +
                      "\n" +
                      "Secret access key is " +
                      data.Credentials.SecretAccessKey
                  );

                  const newAccessKey = data.Credentials.AccessKeyId;
                  const newSecretAccessKey = data.Credentials.SecretAccessKey;

                  console.log(
                      "\nWaiting 10 seconds for the user to assume the role with permission to list all buckets...\n"
                  );
                  wait(10000);
                  // Set the parameters for the temporary credentials. This grants permission to list S3 buckets.
                  var new_role_creds = {
                    accessKeyId: newAccessKey,
                    secretAccessKey: newSecretAccessKey,
                    sessionToken: data.Credentials.SessionToken,
                  };
                  try {
                    console.log(
                        "Listing the S3 buckets using the credentials of the assumed role... \n"
                    );
                    // Create an S3 client with the temporary credentials.
                    const s3ClientWithNewCreds = new S3Client({
                      credentials: new_role_creds,
                      region: REGION,
                    });
                    const data = await s3ClientWithNewCreds.send(
                        new ListBucketsCommand({})
                    );
                    console.log("Success. Your S3 buckets are:", data.Buckets);
                    try {
                      console.log(
                          "Detaching s3 policy from user " + userName + " ... \n"
                      );
                      const data = await iamClient.send(
                          new DetachUserPolicyCommand({
                            PolicyArn: assume_policy_arn,
                            UserName: userName,
                          })
                      );
                      console.log("Success, S3 policy detached from user.");
                      try {
                        console.log(
                            "Detaching role policy from " + role_name + " ... \n"
                        );
                        const data = await iamClient.send(
                            new DetachRolePolicyCommand({
                              PolicyArn: s3_policy_arn,
                              RoleName: role_name,
                            })
                        );
                        console.log(
                            "Success, assume policy detached from role."
                        );
                        try {
                          console.log("Deleting s3 policy ... \n");
                          const data = await iamClient.send(
                              new DeletePolicyCommand({
                                PolicyArn: s3_policy_arn,
                              })
                          );
                          console.log("Success, S3 policy deleted.");
                          try {
                            console.log("Deleting assume role policy ... \n");
                            const data = await iamClient.send(
                                new DeletePolicyCommand({
                                  PolicyArn: assume_policy_arn,
                                })
                            );
                            try {
                              console.log("Deleting access keys ... \n");
                              const data = await iamClient.send(
                                  new DeleteAccessKeyCommand({
                                    UserName: userName,
                                    AccessKeyId: myAccessKey,
                                  })
                              );
                              try {
                                console.log(
                                    "Deleting user " + user_name + " ... \n"
                                );
                                const data = await iamClient.send(
                                    new DeleteUserCommand({ UserName: userName })
                                );
                                console.log("Success, user deleted.");
                                try {
                                  console.log(
                                      "Deleting role " + role_name + " ... \n"
                                  );
                                  const data = await iamClient.send(
                                      new DeleteRoleCommand({
                                        RoleName: role_name,
                                      })
                                  );
                                  console.log("Success, role deleted.");
                                  return "Run successfully"; // For unit tests.
                                } catch (err) {
                                  console.log("Error deleting  role .", err);
                                }
                              } catch (err) {
                                console.log("Error deleting user.", err);
                              }
                            } catch (err) {
                              console.log("Error deleting access keys.", err);
                            }
                          } catch (err) {
                            console.log(
                                "Error detaching assume role policy from user.",
                                err
                            );
                          }
                        } catch (err) {
                          console.log("Error deleting role.", err);
                        }
                      } catch (err) {
                        console.log("Error deleting user.", err);
                      }
                    } catch (err) {
                      console.log("Error detaching S3 policy from role.", err);
                      process.exit(1);
                    }
                  } catch (err) {
                    console.log("Error listing S3 buckets.", err);
                    process.exit(1);
                  }
                } catch (err) {
                  console.log("Error assuming role.", err);
                  process.exit(1);
                }
              } catch (err) {
                console.log(
                    "Error adding permissions to user to assume role.",
                    err
                );
                process.exit(1);
              }
            } catch (err) {
              console.log("Error assuming role.", err);
              process.exit(1);
            }
          } catch (err) {
            console.log("Error creating policy. ", err);
            process.exit(1);
          }
        } catch (err) {
          console.log("Error attaching policy to role.", err);
          process.exit(1);
        }
      }
    } catch (err) {
      console.log("Error creating access keys. ", err);
      process.exit(1);
    }
  } catch (err) {
    console.log("Error creating user. ", err);
  }
};
run(userName, s3_policy_name, role_name, assume_policy_name);
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/iam#code-examples)\. 
+ For API details, see the following topics in *AWS SDK for JavaScript API Reference*\.
  + [AttachRolePolicy](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/attachrolepolicycommand.html)
  + [CreateAccessKey](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/createaccesskeycommand.html)
  + [CreatePolicy](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/createpolicycommand.html)
  + [CreateRole](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/createrolecommand.html)
  + [CreateUser](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/createusercommand.html)
  + [DeleteAccessKey](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/deleteaccesskeycommand.html)
  + [DeletePolicy](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/deletepolicycommand.html)
  + [DeleteRole](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/deleterolecommand.html)
  + [DeleteUser](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/deleteusercommand.html)
  + [DeleteUserPolicy](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/deleteuserpolicycommand.html)
  + [DetachRolePolicy](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/detachrolepolicycommand.html)
  + [PutUserPolicy](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/putuserpolicycommand.html)