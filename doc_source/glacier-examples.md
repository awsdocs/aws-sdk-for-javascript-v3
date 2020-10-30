--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Amazon S3 Glacier examples<a name="glacier-examples"></a>

Amazon S3 Glacier is a secure cloud storage service for data archiving and long\-term backup\. The service is optimized for infrequently accessed data where a retrieval time of several hours is suitable\.

![\[Relationship between JavaScript environments, the SDK, and S3 Glacier\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/code-samples-glacier.png)

The JavaScript API for Amazon S3 Glacier is exposed through the `Glacier` client class\. For more information about using the S3 Glacier client class, see [Class: Glacier](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Glacier.html) in the API reference\.

**Topics**
+ [Creating a S3 Glacier vault](glacier-example-creating-a-vault.md)
+ [Uploading an archive to S3 Glacier](glacier-example-uploadarchive.md)