# Amazon S3 Glacier Examples<a name="glacier-examples"></a>

Amazon S3 Glacier is a secure cloud storage service for data archiving and long\-term backup\. The service is optimized for infrequently accessed data where a retrieval time of several hours is suitable\.

![\[Relationship between JavaScript environments, the SDK, and Glacier\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/code-samples-glacier.png)

The JavaScript API for Amazon S3 Glacier is exposed through the `AWS.Glacier` client class\. For more information about using the Glacier client class, see [Class: AWS\.Glacier](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Glacier.html) in the API reference\.

**Topics**
+ [Creating a Glacier Vault](glacier-example-creating-a-vault.md)
+ [Uploading an Archive to Glacier](glacier-example-uploadrchive.md)
+ [Doing a Multipart Upload to Glacier](glacier-example-multipart-upload.md)