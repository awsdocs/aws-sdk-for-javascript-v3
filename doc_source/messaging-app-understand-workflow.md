--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Understand the AWS Messaging application<a name="messaging-app-understand-workflow"></a>

This topic is part of a tutorial that create an AWS application that sends and retrieves messages by using the AWS SDK for JavaScript and Amazon Simple Queue Service \(Amazon SQS\)\. To start at the beginning of the tutorial, see [Creating an example messaging application](messaging-app.md)\.

To send a message to a SQS queue, enter the message into the application and choose Send\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

After the message is sent, the application displays the message, as shown in this figure\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

You can choose **Purge** to purge the messages from the Amazon SQS queue\. This results in an empty queue, and no messages are displayed in the application\.

The following describes how the application handles a message:
+ The user selects their name and enters their message, and submits the message, which initiates the `pushMessage` function\.
+ `pushMessage` retrieves the Amazon SQS Queue Url, and then sends a message with a unique message ID value \(a GUID\)the message text, and the user to the Amazon SQS Queue\.
+ `pushMessage` retrieves the messages from the Amazon SQS Queue, extracts the user and message for each message, and displays the messages\.
+ The user can purge the messages, which delete the messages from the Amazon SQS Queue and from the user interface\. 