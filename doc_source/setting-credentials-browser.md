--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Setting credentials in a web browser<a name="setting-credentials-browser"></a>

There are several ways to supply your credentials to the SDK from browser scripts\. Some of these are more secure and others afford greater convenience while developing a script\.

 Here are the ways you can supply your credentials, in order of recommendation:

1. Using Amazon Cognito Identity to authenticate users and supply credentials

1. Using web federated identity

1. Hard coding in the script

**Warning**  
We do not recommend hard coding your AWS credentials in your scripts\. Hard coding credentials poses a risk of exposing your access key ID and secret access key\.

**Topics**
+ [Using Amazon Cognito Identity to authenticate users](loading-browser-credentials-cognito.md)