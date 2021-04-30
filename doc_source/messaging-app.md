--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Creating an example messaging application<a name="messaging-app"></a>

You can create an AWS application that sends and retrieves messages by using the AWS SDK for JavaScript and Amazon Simple Queue Service \(Amazon SQS\)\. Messages are stored in a first in, first out \(FIFO\) queue that ensures that the order of the messages is consistent\. For example, the first message that’s stored in the queue is the first message read from the queue\.

**Note**  
 For more information about Amazon SQS, see [What is Amazon Simple Queue Service?](https://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/welcome.html)

In this tutorial, you create a Node\.js application named AWS Messaging\. The following figure shows the AWS Messaging application\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/create-messaging-app/client1a.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

**Cost to complete:** The AWS services included in this document are included in the [AWS Free Tier](https://aws.amazon.com/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc)\.

**Note:** Be sure to terminate all of the resources you create while going through this tutorial to ensure that you’re not charged\.

**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypeScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so with minor adjustments these examples can also be run in JavaScript\. For more information, see [this article](https://aws.amazon.com/blogs/developer/first-class-typescript-support-in-modular-aws-sdk-for-javascript/) in the AWS Developer Blog\.

**Note**  
The code examples in this tutorial import and use the required AWS Service V3 package clients, V3 commands, and use the `send` method in an async/await pattern\. You can create these examples using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**To build the app:**

1. [Prerequisites](messaging-app-prerequisites.md)

1. [Provision resources](messaging-app-provision-resources.md)

1. [Understand the workflow](messaging-app-understand-workflow.md)

1. [Create the HTML](messaging-app-html.md)

1. [Create the browser script](messaging-app-browser-script.md)

1. [Next steps](messaging-app-next-steps.md)