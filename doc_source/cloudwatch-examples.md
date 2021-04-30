--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Amazon CloudWatch examples<a name="cloudwatch-examples"></a>

Amazon CloudWatch \(CloudWatch\) is a web service that monitors your Amazon Web Services resources and applications you run on AWS in real time\. You can use CloudWatch to collect and track metrics, which are variables you can measure for your resources and applications\. CloudWatch alarms send notifications or automatically make changes to the resources you are monitoring based on rules that you define\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/code-samples-cloudwatch.png)

The JavaScript API for CloudWatch is exposed through the `CloudWatch`, `CloudWatchEvents`, and `CloudWatchLogs` client classes\. For more information about using the CloudWatch client classes, see [Class: CloudWatch](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch/classes/cloudwatch.html), [Class: CloudWatchEvents](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch-events/classes/cloudwatchevents.html), and [Class: CloudWatchLogs](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cloudwatch-logs/classes/cloudwatchlogs.html) in the *Amazon CloudWatch API reference*\.

**Topics**
+ [Creating alarms in Amazon CloudWatch](cloudwatch-examples-creating-alarms.md)
+ [Using alarm actions in Amazon CloudWatch](cloudwatch-examples-using-alarm-actions.md)
+ [Getting metrics from Amazon CloudWatch](cloudwatch-examples-getting-metrics.md)
+ [Sending events to Amazon CloudWatch Events](cloudwatch-examples-sending-events.md)
+ [Using subscription filters in Amazon CloudWatch Logs](cloudwatch-examples-subscriptions.md)