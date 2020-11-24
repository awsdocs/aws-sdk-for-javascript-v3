--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# What is the AWS SDK for JavaScript?<a name="welcome"></a>

Welcome to the AWS SDK for JavaScript Developer Guide\. This guide provides general information about setting up and configuring the AWS SDK for JavaScript\. It also walks you through examples and tutorial of running various AWS services using the AWS SDK for JavaScript\.

![\[Relationship between JavaScript environments, the SDK, and Amazon Web Services\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/sdk-overview-v3.png)

## Maintenance and support for SDK major versions<a name="sdks-major-versions-maintenance-support"></a>

For information about maintenance and support for SDK major versions and their underlying dependencies, see the following in the [AWS SDKs and Tools Shared Configuration and Credentials Reference Guide](https://docs.aws.amazon.com/credref/latest/refdocs/overview.html):
+ [AWS SDKs and Tools Maintenance Policy](https://docs.aws.amazon.com/credref/latest/refdocs/maint-policy.html)
+ [AWS SDKs and Tools Version Support Matrix](https://docs.aws.amazon.com/credref/latest/refdocs/version-support-matrix.html)

## What's new in Version 3<a name="welcome_whats_new_v3"></a>

Version 3 of the SDK for JavaScript \(V3\) contains the following new features\.

Modularized packages  
Users can now use a separate package for each service\.

New middleware stack  
Users can now use a middleware stack to control the lifecycle of an operation call\.

In addition, the SDK is written in TypeScript, which has many advantages, such as static typing\.

### Modularized packages<a name="welcome_whats_new_v3_modularized_packages"></a>

Version 2 of the SDK for JavaScript \(V2\) required you to use the entire AWS SDK, as follows\.

```
var AWS = require("aws-sdk");
```

Loading the entire SDK isn’t an issue if your application is using many AWS services\. However, if you need to use only a few AWS services, it means increasing the size of your application with code you don't need or use\.

In V3, you can load and use only Comparing individual AWS Services you need\. This is shown in the following example, which gives you access to Amazon DynamoDB \(DynamoDB\)\.

```
const {DynamoDB} = require("@aws-sdk/client-dynamodb");
```

Not only can you load and use individual AWS services, but you can also load and use only the service commands you need\. This is shown in the following examples, which gives you access to DynamoDB client and the `ListTablesCommand` command\.

```
const {
  DynamoDBClient,
  ListTablesCommand
} = require('@aws-sdk/client-dynamodb')
```

**Important**  
You should not import submodules into modules\. For example, the following code might result in errors\.  

```
const {CognitoIdentity} = require("@aws-sdk/client-cognito-identity/CognitoIdentity")
```
The following is the correct code\.  

```
const {CognitoIdentity} = require("@aws-sdk/client-cognito-identity")
```

#### Comparing code size<a name="welcome_whats_new_v3_modularized_packages_code_size"></a>

In Version 2 \(V2\), a simple code example that lists all of your Amazon DynamoDB tables in the `us-west-2` Region might look like the following\.

```
var AWS = require("aws-sdk");
// Set the Region
AWS.config.update({region: "us-west-2"});
// Create DynamoDB service object
var ddb = new AWS.DynamoDB({apiVersion: "2006-03-01"});

// Call DynamoDB to retrieve the list of tables
ddb.listTables({Limit:10}, function(err, data) {
  if (err) {
    console.log("Error", err.code);
  } else {
    console.log("Tables names are ", data.TableNames);
  }
});
```

V3 looks like the following\.

```
(async function () {
   const { DynamoDBClient, 
           ListTablesCommand 
   }= require('@aws-sdk/client-dynamodb');
   const dbclient = new DynamoDBClient({ region: 'us-west-2'});
 
  try {
    const results = await dbclient.send(new ListTablesCommand);
    results.Tables.forEach(function (item, index) {
      console.log(item.Name);
    });
  } catch (err) {
    console.error(err)
  }
})();
```

The `aws-sdk` package adds about 40 MB to your application\. Replacing `var AWS = require("aws-sdk")` with `const {DynamoDB} = require("@aws-sdk/client-dynamodb")` reduces that overhead to about 3 MB\. Restricting the import to just the DynamoDB client and `ListTablesCommand` command reduces the overhead to less than 100 KB\.

```
// Load the DynamoDB client and ListTablesCommand command for Node.js
const { 
  DynamoDBClient, 
  ListTablesCommand 
} = require('@aws-sdk/client-dynamodb');
const dbclient = new DynamoDBClient({});
```

#### Calling commands in V3<a name="welcome_whats_new_v3_function_examples"></a>

You can perform operations in V3 using either V2 or V3 commands\. To use V3 commands you import the commands and the required AWS Services package clients, and run the command using the `.send` method using the async/await pattern\.

To use V2 commands you import the required AWS Services packages, and run the V2 command directly in the package using either a callback or async/await pattern\.

##### Using V3 commands<a name="using_v3_commands"></a>

V3 provides a set of commands for each AWS Service package to enable you to perform operations for that AWS Service\. After you install an AWS Service, you can browse the available commands in your project's `node-modules/@aws-sdk/client-PACKAGE_NAME/commands folder.` 

You must import the commands you want to use\. For example, the following code loads the DynamoDB service, and the `CreateTableCommand` command\. 

```
const {DynamoDB, CreateTableCommand} = require('@aws-sdk/client-dynamodb');
```

To call these commands in the recommend async/await pattern, use the following syntax\. 

```
  CLIENT.send(newXXXCommand)
```

For example, the following example creates a DynamoDB table using the recommended async/await pattern\.

```
const {DynamoDB, CreateTableCommand} = require('@aws-sdk/client-dynamodb');
const dynamodb = new DynamoDB({region: 'us-west-2'});
var tableParams = {
    Table : TABLE_NAME
};
async function run() => {
      try{
           const data = await dynamodb.send(new CreateTableCommand(tableParams));
           console.log("Success", data);
      } 
      catch (err) {
           console.log("Error", err);
      }
};
run();
```

##### Using V2 commands<a name="using_v2_commands"></a>

To use V2 commands in the ^SDK for JavaScript, you import the full AWS Service packages, as demonstrated in the following code\.

```
const {DynamoDB} = require('@aws-sdk/client-dynamodb');
```

 To call V2 commands in the recommended async/await pattern, use the following syntax\. 

```
client.command(parameters)
```

The following example uses the V2 `createBucket` command to create a DynamoDB table using the recommended async/await pattern\.

```
const {DynamoDB} = require('@aws-sdk/client-dynamodb');
const dymamoDB = new DynamoDB({region: 'us-west-2'});
var tableParams = {
    TableName : TABLE_NAME
};
async function run() => {
      try {
           const data = await dynamoDB.createTable(tableParams);
           console.log("Success", data);
      } 
      catch (err) {
           console.log("Error", err);
      }
};
run();
```

The following example uses the V2 `createBucket` command to create a DynamoDB table using the recommended async/await pattern\.

```
const {DynamoDB} = require('@aws-sdk/client-dynamodb');
const dymamoDB = new DynamoDB({region: 'us-west-2'});
var tableParams = {
    TableName : TABLE_NAME
};
async function run() => {
      try {
           const data = dynamoDB.createTable(tableParams);
           console.log("Success", data);
      } 
      catch (err) {
           console.log("Error", err);
      }
};
run();
```

The following example uses the V2 `createBucket` command to create a DynamoDB table using the c pattern\.

```
const {S3} = require('@aws-sdk/client-s3');
const s3 = new S3({region: 'us-west-2'});
var bucketParams = {
    Bucket : BUCKET_NAME
};
function run(){
         s3.createBucket(bucketParams, function(err, data) {
         if (err) {
         console.log("Error", err);
         } else {
         console.log("Success", data.Location);
         }
    })
};
```

### New middleware stack<a name="welcome_whats_new_v3_middleware_stack"></a>

V2 of the SDK enabled you to modify a request throughout the multiple stages of its lifecycle by attaching event listeners to the request\. This approach can make it difficult to debug what went wrong during a request’s lifecycle\.

In V3, you can use a new middleware stack to control the lifecycle of an operation call\. This approach provides a couple of benefits\. Each middleware stage in the stack calls the next middleware stage after making any changes to the request object\. This also makes debugging issues in the stack much easier, because you can see exactly which middleware stages were called leading up to the error\.

The following example adds a custom header to a Amazon DynamoDB client \(which we created and showed earlier\) using middleware\. The first argument is a function that accepts `next`, which is the next middleware stage in the stack to call, and `context`, which is an object that contains some information about the operation being called\. The function returns a function that accepts `args`, which is an object that contains the parameters passed to the operation and the request\. It returns the result from calling the next middleware with `args`\.

```
dbclient.middlewareStack.add(
  (next, context) => args => {
    args.request.headers["Custom-Header"] = "value";
    return next(args);
  },
  {
    step: "build"
  }
);  

dbclient.send(new PutObjectCommand(params));
```

## Using the SDK with Node\.js<a name="welcome_node"></a>

Node\.js is a cross\-platform runtime for running server\-side JavaScript applications\. You can set up Node\.js on an Amazon Elastic Compute Cloud \(Amazon EC2\) instance to run on a server\. You can also use Node\.js to write on\-demand AWS Lambda functions\.

Using the SDK for Node\.js differs from the way in which you use it for JavaScript in a web browser\. The difference comes from the way in which you load the SDK and in how you obtain the credentials needed to access specific web services\. When use of particular APIs differs between Node\.js and the browser, we call out those differences\.

## Using the SDK with AWS Cloud9<a name="welcome_cloud9"></a>

You can also develop Node\.js applications using the SDK for JavaScript in the AWS Cloud9 IDE\.  For more information about using AWS Cloud9 with the SDK for JavaScript, see [Using AWS Cloud9 with the AWS SDK for JavaScript](cloud9-javascript.md)\.

## Using the SDK with AWS Amplify<a name="welcome_amplify"></a>

For browser\-based web, mobile, and hybrid apps, you can also use the [AWS Amplify library on GitHub](https://github.com/aws/aws-amplify)\. It extends the SDK for JavaScript, providing a declarative interface\.

**Note**  
Frameworks such as Amplify might not offer the same browser support as the SDK for JavaScript\. See the framework's documentation for details\.

## Using the SDK with web browsers<a name="welcome_web"></a>

All major web browsers support execution of JavaScript\. JavaScript code that is running in a web browser is often called *client\-side JavaScript*\.

For a list of browsers that are supported by the AWS SDK for JavaScript, see [Supported web browsers](browsers-supported.md)\.

Using the SDK for JavaScript in a web browser differs from the way in which you use it for Node\.js\. The difference comes from the way in which you load the SDK and in how you obtain the credentials needed to access specific web services\. When use of particular APIs differs between Node\.js and the browser, we call out those differences\.

### Using browsers in V3<a name="v3_browsers"></a>

V3 enables you to bundle and include in the browser only the SDK for JavaScript files you require, reducing overhead\.

To use V3 of the SDK for JavaScript in your HTML pages, you must bundle the required client modules and all required JavaScript functions into a single JavaScript file using Webpack, and add it in a script tag in the `<head>` of your HTML pages\. For example:

```
<script src="./main.js"></script>
```

**Note**  
For more information about Webpack, see [Bundling applications with webpack](webpack.md)\.

To use V2 of the SDK for JavaScript you instead add a script tag that points to the latest version of the V2 SDK\. For more information, see [https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/getting-started-browser.html#getting-started-browser-run-sample](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/getting-started-browser.html#getting-started-browser-run-sample)the SDK for JavaScript v2 Developer Guide\.

### Common use cases<a name="welcome_use_cases"></a>

Using the SDK for JavaScript in browser scripts makes it possible to realize a number of compelling use cases\. Here are several ideas for things you can build in a browser application by using the SDK for JavaScript to access various web services\.
+ Build a custom console to AWS services in which you access and combine features across Regions and services to best meet your organizational or project needs\.
+ Use Amazon Cognito Identity to enable authenticated user access to your browser applications and websites, including use of third\-party authentication from Facebook and others\.
+ Use Amazon Kinesis to process click streams or other marketing data in real time\.
+ Use Amazon DynamoDB for serverless data persistence, such as individual user preferences for website visitors or application users\.
+ Use AWS Lambda to encapsulate proprietary logic that you can invoke from browser scripts without downloading and revealing your intellectual property to users\.

### About the examples<a name="welcome_examples"></a>

You can browse the SDK for JavaScript examples in the [AWS Code Example Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code)\.

### Resources<a name="welcome_resources"></a>

In addition to this guide, the following online resources are available for SDK for JavaScript developers:
+ [AWS SDK for JavaScript API Reference](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/)
+ [JavaScript Developer Blog](https://aws.amazon.com/blogs/developer/category/programing-language/javascript/)
+ [AWS JavaScript Forum](https://forums.aws.amazon.com/forum.jspa?forumID=148)
+ [JavaScript examples in the AWS Code Catalog](https://docs.aws.amazon.com/code-samples/latest/catalog/code-catalog-javascript.html)
+ [AWS Code Example Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code)
+ [Gitter channel](https://gitter.im/aws/aws-sdk-js)
+ [Stack Overflow](https://stackoverflow.com/search?tab=newest&q=aws-sdk-js)
+ [Stack Overflow questions tagged aws\-sdk\-js](https://stackoverflow.com/questions/tagged/aws-sdk-js?sort=newest)
+ GitHub
  + [SDK Source](https://github.com/aws/aws-sdk-js-v3/)
  + [Documentation Source](https://github.com/awsdocs/aws-sdk-for-javascript-v3)
