# Configuring the SDK for JavaScript<a name="configuring-the-jssdk"></a>

Before you use the SDK for JavaScript to invoke web services using the API, you must configure the SDK\. At a minimum, you must configure these settings:
+ The AWS Region in which you will request services\.
+ The *credentials* that authorize your access to SDK resources\.

In addition to these settings, you may also have to configure permissions for your AWS resources\. For example, you can limit access to an Amazon S3 bucket or restrict an Amazon DynamoDB table for read\-only access\.

The topics in this section describe various ways to configure the SDK for JavaScript for Node\.js and JavaScript running in a web browser\.

**Topics**
+ [Setting the AWS Region](setting-region.md)
+ [Specifying Custom Endpoints](specifying-endpoints.md)
+ [Getting Your Credentials](getting-your-credentials.md)
+ [Setting Credentials](setting-credentials.md)
+ [Node\.js Considerations](node-js-considerations.md)
+ [Browser Script Considerations](browser-js-considerations.md)
+ [Bundling Applications with webpack](webpack.md)