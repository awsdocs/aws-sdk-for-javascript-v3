--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Loading the SDK for JavaScript<a name="loading-the-jssdk"></a>

After you install the SDK, you can load a client package in your node application using `require`\. For example, to load the Amazon S3 client, use the following\.

```
const {S3} = require('@aws-sdk/client-s3');
```