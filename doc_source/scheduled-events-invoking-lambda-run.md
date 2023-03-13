--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Configure CloudWatch to invoke the Lambda functions<a name="scheduled-events-invoking-lambda-run"></a>

To configure CloudWatch to invoke the Lambda functions:

1. Open the **Functions** page on the Lambda console\.

1. Choose the Lambda function\.

1. Under **Designer**, choose **Add trigger**\.

1. Set the trigger type to **CloudWatch Events/EventBridge**\.

1. For Rule, choose **Create a new rule**\.

1.  Fill in the Rule name and Rule description\.

1. For rule type, select **Schedule expression**\.

1. In the **Schedule expression** field, enter a cron expression\. For example, **cron\(0 12 ? \* MON\-FRI \*\)**\.

1. Choose **Add**\.
**Note**  
For more information, see [Using Lambda with CloudWatch Events](https://docs.aws.amazon.com/lambda/latest/dg/services-cloudwatchevents.html)\.