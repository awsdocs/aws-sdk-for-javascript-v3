--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Amazon Simple Notification Service Examples<a name="sns-examples"></a>

Amazon Simple Notification Service \(Amazon SNS\) is a web service that coordinates and manages the delivery or sending of messages to subscribing endpoints or clients\. 

In Amazon SNS, there are two types of clients—publishers and subscribers—also referred to as producers and consumers\. 

![\[Relationship between JavaScript environments, the SDK, and Amazon SNS\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/code-samples-sns.png)

Publishers communicate asynchronously with subscribers by producing and sending a message to a topic, which is a logical access point and communication channel\. Subscribers \(web servers, email addresses, Amazon SQS queues, AWS Lambda functions\) consume or receive the message or notification over one of the supported protocols \(Amazon SQS, HTTP/S, email, SMS, AWS Lambda\) when they are subscribed to the topic\. 

The JavaScript API for Amazon SNS is exposed through the [Class: SNS](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sns/classes/sns.html)\. 

**Topics**
+ [Managing Topics in Amazon SNS](sns-examples-managing-topics.md)
+ [Publishing Messages in Amazon SNS](sns-examples-publishing-messages.md)
+ [Managing Subscriptions in Amazon SNS](sns-examples-subscribing-unubscribing-topics.md)
+ [Sending SMS Messages with Amazon SNS](sns-examples-sending-sms.md)