--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Loading credentials in Node\.js from environment variables<a name="loading-node-credentials-environment"></a>

The SDK automatically detects AWS credentials set as variables in your environment and uses them for SDK requests\. This eliminates the need to manage credentials in your application\. The environment variables that you set to provide your credentials are:
+ `AWS_ACCESS_KEY_ID`
+ `AWS_SECRET_ACCESS_KEY`
+ `AWS_SESSION_TOKEN` \(Optional\)

**Note**  
When setting environment variables, be sure to take appropriate actions afterward \(according to the needs of your operating system\) to make the variables available in the shell or command environment\.