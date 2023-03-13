--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Lambda examples using SDK for JavaScript \(v3\)<a name="javascript_lambda_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for JavaScript \(v3\) with Lambda\.

*Actions* are code excerpts that show you how to call individual service functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple functions within the same service\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#actions)
+ [Scenarios](#scenarios)

## Actions<a name="actions"></a>

### Create a function<a name="lambda_CreateFunction_javascript_topic"></a>

The following code example shows how to create a Lambda function\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/lambda#code-examples)\. 
  

```
const createFunction = async (funcName, roleArn) => {
  const client = createClientForDefaultRegion(LambdaClient);
  const code = await readFile(`${dirname}../functions/${funcName}.zip`);

  const command = new CreateFunctionCommand({
    Code: { ZipFile: code },
    FunctionName: funcName,
    Role: roleArn,
    Architectures: [Architecture.arm64],
    Handler: "index.handler", // Required when sending a .zip file
    PackageType: PackageType.Zip, // Required when sending a .zip file
    Runtime: Runtime.nodejs16x, // Required when sending a .zip file
  });

  return client.send(command);
};
```
+  For API details, see [CreateFunction](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-lambda/classes/createfunctioncommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Delete a function<a name="lambda_DeleteFunction_javascript_topic"></a>

The following code example shows how to delete a Lambda function\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/lambda#code-examples)\. 
  

```
const deleteFunction = (funcName) => {
  const client = createClientForDefaultRegion(LambdaClient);
  const command = new DeleteFunctionCommand({ FunctionName: funcName });
  return client.send(command);
};
```
+  For API details, see [DeleteFunction](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-lambda/classes/deletefunctioncommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Get a function<a name="lambda_GetFunction_javascript_topic"></a>

The following code example shows how to get a Lambda function\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/lambda#code-examples)\. 
  

```
const getFunction = (funcName) => {
  const client = createClientForDefaultRegion(LambdaClient);
  const command = new GetFunctionCommand({ FunctionName: funcName });
  return client.send(command);
};
```
+  For API details, see [GetFunction](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-lambda/classes/getfunctioncommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Invoke a function<a name="lambda_Invoke_javascript_topic"></a>

The following code example shows how to invoke a Lambda function\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/lambda#code-examples)\. 
  

```
const invoke = async (funcName, payload) => {
  const client = createClientForDefaultRegion(LambdaClient);
  const command = new InvokeCommand({
    FunctionName: funcName,
    Payload: JSON.stringify(payload),
    LogType: LogType.Tail,
  });

  const { Payload, LogResult } = await client.send(command);
  const result = Buffer.from(Payload).toString();
  const logs = Buffer.from(LogResult, "base64").toString();
  return { logs, result };
};
```
+  For API details, see [Invoke](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-lambda/classes/invokecommand.html) in *AWS SDK for JavaScript API Reference*\. 

### List functions<a name="lambda_ListFunctions_javascript_topic"></a>

The following code example shows how to list Lambda functions\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/lambda#code-examples)\. 
  

```
const listFunctions = async () => {
  const client = createClientForDefaultRegion(LambdaClient);
  const command = new ListFunctionsCommand({});

  return client.send(command);
};
```
+  For API details, see [ListFunctions](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-lambda/classes/listfunctionscommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Update function code<a name="lambda_UpdateFunctionCode_javascript_topic"></a>

The following code example shows how to update Lambda function code\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/lambda#code-examples)\. 
  

```
const updateFunctionCode = async (funcName, newFunc) => {
  const client = createClientForDefaultRegion(LambdaClient);
  const code = await readFile(`${dirname}../functions/${newFunc}.zip`);
  const command = new UpdateFunctionCodeCommand({
    ZipFile: code,
    FunctionName: funcName,
    Architectures: [Architecture.arm64],
    Handler: "index.handler", // Required when sending a .zip file
    PackageType: PackageType.Zip, // Required when sending a .zip file
    Runtime: Runtime.nodejs16x, // Required when sending a .zip file
  });

  return client.send(command);
};
```
+  For API details, see [UpdateFunctionCode](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-lambda/classes/updatefunctioncodecommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Update function configuration<a name="lambda_UpdateFunctionConfiguration_javascript_topic"></a>

The following code example shows how to update Lambda function configuration\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/lambda#code-examples)\. 
  

```
const updateFunctionConfiguration = async (funcName) => {
  const client = createClientForDefaultRegion(LambdaClient);
  const config = readFileSync(
    `${dirname}../functions/config.json`
  ).toString();
  const command = new UpdateFunctionConfigurationCommand({
    ...JSON.parse(config),
    FunctionName: funcName,
  });
  return client.send(command);
};
```
+  For API details, see [UpdateFunctionConfiguration](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-lambda/classes/updatefunctionconfigurationcommand.html) in *AWS SDK for JavaScript API Reference*\. 

## Scenarios<a name="scenarios"></a>

### Get started with functions<a name="lambda_Scenario_GettingStartedFunctions_javascript_topic"></a>

The following code example shows how to:
+ Create an AWS Identity and Access Management \(IAM\) role that grants Lambda permission to write to logs\.
+ Create a Lambda function and upload handler code\.
+ Invoke the function with a single parameter and get results\.
+ Update the function code and configure its Lambda environment with an environment variable\.
+ Invoke the function with new parameters and get results\. Display the execution log that's returned from the invocation\.
+ List the functions for your account\.
+ Delete the IAM role and the Lambda function\.

For more information, see [Create a Lambda function with the console](https://docs.aws.amazon.com/lambda/latest/dg/getting-started-create-function.html)\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/lambda/scenarios/basic#code-examples)\. 
Create an AWS Identity and Access Management \(IAM\) role that grants Lambda permission to write to logs\.  

```
    log(`Creating role (${NAME_ROLE_LAMBDA})...`);
    const response = await createRole({
      AssumeRolePolicyDocument: parseString({
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
      }),
      RoleName: NAME_ROLE_LAMBDA,
    });

const attachRolePolicy = async (roleName, policyArn) => {
  const client = createClientForDefaultRegion(IAMClient);
  const command = new AttachRolePolicyCommand({
    PolicyArn: policyArn, // For example, arn:aws:iam::aws:policy/service-role/AWSLambdaBasicExecutionRole
    RoleName: roleName, // For example, lambda-basic-execution-role
  });

  return client.send(command);
};
```
Create a Lambda function and upload handler code\.  

```
const createFunction = async (funcName, roleArn) => {
  const client = createClientForDefaultRegion(LambdaClient);
  const code = await readFile(`${dirname}../functions/${funcName}.zip`);

  const command = new CreateFunctionCommand({
    Code: { ZipFile: code },
    FunctionName: funcName,
    Role: roleArn,
    Architectures: [Architecture.arm64],
    Handler: "index.handler", // Required when sending a .zip file
    PackageType: PackageType.Zip, // Required when sending a .zip file
    Runtime: Runtime.nodejs16x, // Required when sending a .zip file
  });

  return client.send(command);
};
```
Invoke the function with a single parameter and get results\.  

```
const invoke = async (funcName, payload) => {
  const client = createClientForDefaultRegion(LambdaClient);
  const command = new InvokeCommand({
    FunctionName: funcName,
    Payload: JSON.stringify(payload),
    LogType: LogType.Tail,
  });

  const { Payload, LogResult } = await client.send(command);
  const result = Buffer.from(Payload).toString();
  const logs = Buffer.from(LogResult, "base64").toString();
  return { logs, result };
};
```
Update the function code and configure its Lambda environment with an environment variable\.  

```
const updateFunctionCode = async (funcName, newFunc) => {
  const client = createClientForDefaultRegion(LambdaClient);
  const code = await readFile(`${dirname}../functions/${newFunc}.zip`);
  const command = new UpdateFunctionCodeCommand({
    ZipFile: code,
    FunctionName: funcName,
    Architectures: [Architecture.arm64],
    Handler: "index.handler", // Required when sending a .zip file
    PackageType: PackageType.Zip, // Required when sending a .zip file
    Runtime: Runtime.nodejs16x, // Required when sending a .zip file
  });

  return client.send(command);
};

const updateFunctionConfiguration = async (funcName) => {
  const client = createClientForDefaultRegion(LambdaClient);
  const config = readFileSync(
    `${dirname}../functions/config.json`
  ).toString();
  const command = new UpdateFunctionConfigurationCommand({
    ...JSON.parse(config),
    FunctionName: funcName,
  });
  return client.send(command);
};
```
List the functions for your account\.  

```
const listFunctions = async () => {
  const client = createClientForDefaultRegion(LambdaClient);
  const command = new ListFunctionsCommand({});

  return client.send(command);
};
```
Delete the IAM role and the Lambda function\.  

```
const deleteRole = (roleName) => {
  const client = createClientForDefaultRegion(IAMClient);
  const command = new DeleteRoleCommand({ RoleName: roleName });
  return client.send(command);
};

const deleteFunction = (funcName) => {
  const client = createClientForDefaultRegion(LambdaClient);
  const command = new DeleteFunctionCommand({ FunctionName: funcName });
  return client.send(command);
};
```
+ For API details, see the following topics in *AWS SDK for JavaScript API Reference*\.
  + [CreateFunction](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-lambda/classes/createfunctioncommand.html)
  + [DeleteFunction](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-lambda/classes/deletefunctioncommand.html)
  + [GetFunction](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-lambda/classes/getfunctioncommand.html)
  + [Invoke](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-lambda/classes/invokecommand.html)
  + [ListFunctions](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-lambda/classes/listfunctionscommand.html)
  + [UpdateFunctionCode](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-lambda/classes/updatefunctioncodecommand.html)
  + [UpdateFunctionConfiguration](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-lambda/classes/updatefunctionconfigurationcommand.html)