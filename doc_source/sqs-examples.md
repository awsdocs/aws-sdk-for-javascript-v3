--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Amazon SQS examples<a name="sqs-examples"></a>

Amazon Simple Queue Service \(SQS\) is a fast, reliable, scalable, fully managed message queuing service\. Amazon SQS lets you decouple the components of a cloud application\. Amazon SQS includes standard queues with high throughput and at\-least\-once processing, and FIFO queues that provide first\-in, first\-out \(FIFO\) delivery and exactly\-once processing\.

![\[Relationship between JavaScript environments, the SDK, and Amazon SQS\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/code-samples-sqs.png)

The JavaScript API for Amazon SQS is exposed through the `SQS` client class\. For more information about using the Amazon SQS; client class, see [Class: SQS](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SQS.html) in the API Reference\.

**Topics**
+ [Using queues in Amazon SQS](sqs-examples-using-queues.md)
+ [Sending and receiving messages in Amazon SQS](sqs-examples-send-receive-messages.md)
+ [Managing visibility timeout in Amazon SQS](sqs-examples-managing-visibility-timeout.md)
+ [Enabling long polling in Amazon SQS](sqs-examples-enable-long-polling.md)
+ [Using dead\-letter queues in Amazon SQS](sqs-examples-dead-letter-queues.md)