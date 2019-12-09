# Amazon Simple Notification Service Examples<a name="sns-examples"></a>

Amazon Simple Notification Service \(Amazon SNS\) is a web service that coordinates and manages the delivery or sending of messages to subscribing endpoints or clients\. 

In Amazon SNS, there are two types of clients—publishers and subscribers—also referred to as producers and consumers\. 

![\[Relationship between JavaScript environments, the SDK, and Amazon SNS\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/code-samples-sns.png)

Publishers communicate asynchronously with subscribers by producing and sending a message to a topic, which is a logical access point and communication channel\. Subscribers \(web servers, email addresses, Amazon SQS queues, Lambda functions\) consume or receive the message or notification over one of the supported protocols \(Amazon SQS, HTTP/S, email, SMS, AWS Lambda\) when they are subscribed to the topic\. 

The JavaScript API for Amazon SNS is exposed through the [Class: AWS\.SNS](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SNS.html)\. 

**Topics**
+ [Managing Topics in Amazon SNS](sns-examples-managing-topics.md)
+ [Publishing Messages in Amazon SNS](sns-examples-publishing-messages.md)
+ [Managing Subscriptions in Amazon SNS](sns-examples-subscribing-unubscribing-topics.md)
+ [Sending SMS Messages with Amazon SNS](sns-examples-sending-sms.md)