--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Configuring the SDK for JavaScript<a name="configuring-the-jssdk"></a>

Before you use the SDK for JavaScript to invoke web services using the API, you must configure the SDK\. At a minimum, you must configure these settings:
+ The AWS Region in which you will request services
+ The *credentials* that authorize your access to SDK resources

In addition to these settings, you might also have to configure permissions for your AWS resources\. For example, you can limit access to an Amazon S3 bucket or restrict an Amazon DynamoDB table for read\-only access\.

The topics in this section describe the ways to configure the SDK for JavaScript for Node\.js and JavaScript running in a web browser\.

**Topics**
+ [Configuration per service](global-config-object.md)
+ [Setting the AWS Region](setting-region.md)
+ [Getting your credentials](getting-your-credentials.md)
+ [Setting credentials](setting-credentials.md)
+ [Node\.js considerations](node-js-considerations.md)
+ [Browser Script Considerations](browser-js-considerations.md)