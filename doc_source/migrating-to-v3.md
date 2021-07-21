--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Migrating your code to SDK for JavaScript V3<a name="migrating-to-v3"></a>

There are a number of migration paths to the SDK for JavaScript version 3 \(V3\)\. To take full advantage of the reduction in capacity potential of V3, we recommend using path 3\.

**Important**  
AWS SDK for JavaScript version 3 \(v3\) also comes with modernized interfaces for client configurations and utilities, which include credentials, Amazon S3 multipart upload, DynamoDB document client, waiters, and so forth\)\. You can find what changed in v2 and the v3 equivalents for each change in the [migration guide on the AWS SDK for JavaScript GitHub repo](https://github.com/aws/aws-sdk-js-v3/blob/main/UPGRADING.md)\.

------
#### [ Path 1 ]

Perform minimal changes:
+ Install only the specific AWS Service packages you need\.
+ Create and use V3 service clients, replacing the use of any global configuration values, such as Region, with configuration values passed in as arguments to the client\. 
**Note**  
You can set the AWS Region using an environment variable, or a shared configuration `config` file\. For more information, see [ Setting credentials in Node\.js  There are several ways in Node\.js to supply your credentials to the SDK\. Some of these are more secure and others afford greater convenience while developing an application\. When obtaining credentials in Node\.js, be careful about relying on more than one source, such as an environment variable and a JSON file you load\. You can change the permissions under which your code runs without realizing the change has happened\. You can supply your credentials in order of recommendation:   Loaded from AWS Identity and Access Management \(IAM\) roles for Amazon EC2   Loaded from the shared credentials file \(`~/.aws/credentials`\)   Loaded from environment variables   Loaded from a JSON file on disk   Other credential\-provider classes provided by the JavaScript SDK   If no credential provider is supplied to a client, the default precedence of selection is as follows:   Environment variables   The shared credentials file   Credentials loaded from the Amazon ECS credentials provider \(if applicable\)   Credentials loaded from AWS Identity and Access Management using the credentials provider of the Amazon EC2 instance \(if configured in the instance metadata\)    We don't recommend hard\-coding your AWS credentials in your application\. Hard\-coding credentials poses a risk of exposing your access key ID and secret access key\.  The topics in this section describe how to load credentials into Node\.js\.    Loading credentials in Node\.js from IAM roles for Amazon EC2Credentials for Amazon EC2 from IAM roles  If you run your Node\.js application on an Amazon EC2 instance, you can leverage IAM roles for Amazon EC2 to automatically provide credentials to the instance\. If you configure your instance to use IAM roles, the SDK automatically selects the IAM credentials for your application, eliminating the need to manually provide credentials\. For more information about adding IAM roles to an Amazon EC2 instance, see [IAM roles for Amazon EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html)\.    Loading credentials for a Node\.js Lambda functionCredentials for a Node\.js Lambda function  When you create an AWS Lambda function, you must create a special IAM role that has permission to execute the function\. This role is called the *execution role*\. When you set up a Lambda function, you must specify the IAM role you created as the corresponding execution role\. The execution role provides the Lambda function with the credentials it needs to run and to invoke other web services\. As a result, you don't need to provide credentials to the Node\.js code you write within a Lambda function\. For more information about creating a Lambda execution role, see [Manage permissions: Using an IAM role \(execution role\)](https://docs.aws.amazon.com/lambda/latest/dg/intro-permission-model.html#lambda-intro-execution-role) in the *AWS Lambda Developer Guide*\.    Loading credentials in Node\.js from the shared credentials fileCredentials from the shared credentials file  You can keep your AWS credentials data in a shared file used by SDKs and the command line interface\. When the SDK for JavaScript loads, it automatically searches the shared credentials file, which is named "credentials"\. Where you keep the shared credentials file depends on your operating system:   The shared credentials file on Linux, Unix, and macOS: `~/.aws/credentials`   The shared credentials file on Windows: `C:\Users\USER_NAME\.aws\credentials`   If you do not already have a shared credentials file, see [Getting your credentials](getting-your-credentials.md)\. Once you follow those instructions, you should see text similar to the following in the credentials file, where *<YOUR\_ACCESS\_KEY\_ID>* is your access key ID and *<YOUR\_SECRET\_ACCESS\_KEY>* is your secret access key: 

```
[default]
aws_access_key_id = <YOUR_ACCESS_KEY_ID>
aws_secret_access_key = <YOUR_SECRET_ACCESS_KEY>
``` For an example showing this file being used, see [Getting started in Node\.js](getting-started-nodejs.md)\. The `[default]` section heading specifies a default profile and associated values for credentials\. You can create additional profiles in the same shared configuration file, each with its own credential information\. The following example shows a configuration file with the default profile and two additional profiles: 

```
[default] ; default profile
aws_access_key_id = <DEFAULT_ACCESS_KEY_ID>
aws_secret_access_key = <DEFAULT_SECRET_ACCESS_KEY>
    
[personal-account] ; personal account profile
aws_access_key_id = <PERSONAL_ACCESS_KEY_ID>
aws_secret_access_key = <PERSONAL_SECRET_ACCESS_KEY>
    
[work-account] ; work account profile
aws_access_key_id = <WORK_ACCESS_KEY_ID>
aws_secret_access_key = <WORK_SECRET_ACCESS_KEY>
``` By default, the SDK checks the `AWS_PROFILE` environment variable to determine which profile to use\. If the `AWS_PROFILE` variable is not set in your environment, the SDK uses the credentials for the `[default]` profile\. To use one of the alternate profiles, set or change the value of the `AWS_PROFILE` environment variable\. For example, given the configuration file shown, to use the credentials from the work account, set the `AWS_PROFILE` environment variable to `work-account` \(as appropriate for your operating system\)\.  When setting environment variables, be sure to take appropriate actions afterward \(according to the needs of your operating system\) to make the variables available in the shell or command environment\.  After setting the environment variable \(if needed\), you can run a file named `script.js` that uses the SDK as follows\. 

```
$ node script.js
``` You can also explicitly select the profile used by a client, as shown in the following example\. 

```
const {fromIni} = require("@aws-sdk/credential-provider-ini");
const s3Client = new S3.S3Client({
  credentials: fromIni({profile: 'work-account'})
});
```    Loading credentials in Node\.js from environment variablesCredentials from environment variables  The SDK automatically detects AWS credentials set as variables in your environment and uses them for SDK requests\. This eliminates the need to manage credentials in your application\. The environment variables that you set to provide your credentials are:   `AWS_ACCESS_KEY_ID`   `AWS_SECRET_ACCESS_KEY`   `AWS_SESSION_TOKEN` \(Optional\)    When setting environment variables, be sure to take appropriate actions afterward \(according to the needs of your operating system\) to make the variables available in the shell or command environment\.       Loading credentials in Node\.js using a configured credential processCredentials using a configured credential process  For details about specifying a credential process in the shared AWS `config` file or the shared credentials file, see [Sourcing credentials from external processes](https://docs.aws.amazon.com/cli/latest/topic/config-vars.html#sourcing-credentials-from-external-processes)\.  ](setting-credentials-node.md#setting-credentials-node.title)\.

------
#### [ Path 2 ]

Follow path 1 and remove `.promise`, which are not required in V3\.

------
#### [ Path 3 ]

Follow path 1 and use the async/await programming model\.

------

**Important**  
For information about significant changes from AWS SDK for JavaScript v2 to v3, please see [Upgrading Notes \(2\.x to 3\.x\)](https://github.com/aws/aws-sdk-js-v3/blob/main/UPGRADING.md) on GitHub\.

The following sections describe these paths in detail, with examples\.

## Path 1 example<a name="path1-examples"></a>

The following code installs the AWS Service package for Amazon S3\. 

```
npm install @aws-sdk/client-s3
```

The following code loads the Amazon S3 service\.

```
const {S3} = require('@aws-sdk/client-s3');
```

**Note**  
To use this approach you must import the full AWS Service packages, `S3` in this case, and not just the service clients\.

The following code creates an Amazon S3 service object in the `us-west-2` Region\.

```
const client = new S3({region: 'us-west-2'});
```

The following code creates and Amazon S3 bucket using a callback function, using the following syntax from V2\.

```
client.command(parameters)
```

```
const {S3} = require('@aws-sdk/client-s3');
const client = new S3({region: 'us-west-2'});
const bucketParams = {
    Bucket : BUCKET_NAME
};
function run(){
         client.createBucket(bucketParams, function(err, data) {
         if (err) {
         console.log("Error", err);
         } else {
         console.log("Success", data.Location);
         }
    })
};
run();
```

## Path 2 example<a name="path2-examples"></a>

Here is a function call in V2 using a `promise`\.

```
const data =  await v2client.command(params).promise()
```

Here is the V3 version\.

```
const data = await v2client.command(params)
```

## Path 3 examples<a name="path3-examples"></a>

The following command installs the AWS Service package for Amazon S3\.

```
npm install @aws-sdk/client-s3; 
```

The following code loads only the Amazon S3 client, reducing the overhead\.

```
const {S3Client, CreateBucketCommand} = require('@aws-sdk/client-s3');
```

 If you install only the client of a package, you must also import the V3 commands you want to use\. In this case, the code imports the `CreateBucketCommand`, which enables you to create an Amazon S3 bucket\. You can browse the available commands in your project's `node-modules/@aws-sdk/client-PACKAGE_NAME/commands` folder\. 

The following code creates an Amazon S3 service client object in the `us-west-2` Region\. 

```
const client = new S3Client({region: 'us-west-2'});
```

To call imported commands using the recommended async/await pattern, you must import the commands you want to use, and use the following syntax to run the command\.

```
  CLIENT.send(newXXXCommand)
```

The following example creates an Amazon S3 bucket using the async/await pattern, using only the client of the Amazon S3 service package to reduce overhead\.

```
const {S3Client, CreateBucketCommand} = require('@aws-sdk/client-s3');
const client = new S3Client({region: 'us-west-2'});
const bucketParams = {
    Bucket : BUCKET_NAME
};

const run = async () => {
      try{
        const data = await client.send(new CreateBucketCommand(bucketParams));
        console.log("Success", data);
      } catch (err) {
        console.log("Error", err);
      }
};
await run();
```

For more examples, see [SDK for JavaScript code examples](sdk-code-samples.md)\.