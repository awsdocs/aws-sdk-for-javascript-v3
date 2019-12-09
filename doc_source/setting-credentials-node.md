# Setting Credentials in Node\.js<a name="setting-credentials-node"></a>

There are several ways in Node\.js to supply your credentials to the SDK\. Some of these are more secure and others afford greater convenience while developing an application\. When obtaining credentials in Node\.js, be careful about relying on more than one source such as an environment variable and a JSON file you load\. You can change the permissions under which your code runs without realizing the change has happened\.

Here are the ways you can supply your credentials in order of recommendation:

1. Loaded from AWS Identity and Access Management \(IAM\) roles for Amazon EC2

1. Loaded from the shared credentials file \(`~/.aws/credentials`\)

1. Loaded from environment variables

1. Loaded from a JSON file on disk

1. Other credential\-provider classes provided by the JavaScript SDK

If more than one credential source is available to the SDK, the default precedence of selection is as follows:

1. Credentials that are explicitly set through the service\-client constructor

1. Environment variables

1. The shared credentials file

1. Credentials loaded from the Amazon ECS credentials provider \(if applicable\)

1. Credentials that are obtained by using a credential process specified in the shared AWS config file or the shared credentials file\. For more information, see [Loading Credentials in Node\.js using a Configured Credential Process](loading-node-credentials-configured-credential-process.md)\.

1. Credentials loaded from AWS Identity and Access Management \(IAM\) using the credentials provider of the Amazon EC2 instance \(if configured in the instance metadata\)

For more information, see [Class: AWS\.Credentials](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Credentials.html) and [Class: AWS\.CredentialProviderChain](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CredentialProviderChain.html) in the API reference\.

**Warning**  
While it is possible to do so, we do not recommend hard\-coding your AWS credentials in your application\. Hard\-coding credentials poses a risk of exposing your access key ID and secret access key\.

The topics in this section describe how to load credentials into Node\.js\.

**Topics**
+ [Loading Credentials in Node\.js from IAM Roles for Amazon EC2](loading-node-credentials-iam.md)
+ [Loading Credentials for a Node\.js Lambda Function](loading-node-credentials-lambda.md)
+ [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)
+ [Loading Credentials in Node\.js from Environment Variables](loading-node-credentials-environment.md)
+ [Loading Credentials in Node\.js using a Configured Credential Process](loading-node-credentials-configured-credential-process.md)