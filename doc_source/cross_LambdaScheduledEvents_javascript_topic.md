--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Use scheduled events to invoke a Lambda function<a name="cross_LambdaScheduledEvents_javascript_topic"></a>

**SDK for JavaScript \(v3\)**  
 Shows how to create an Amazon EventBridge scheduled event that invokes an AWS Lambda function\. Configure EventBridge to use a cron expression to schedule when the Lambda function is invoked\. In this example, you create a Lambda function by using the Lambda JavaScript runtime API\. This example invokes different AWS services to perform a specific use case\. This example demonstrates how to create an app that sends a mobile text message to your employees that congratulates them at the one year anniversary date\.   
 For complete source code and instructions on how to set up and run, see the full example on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cross-services/lambda-scheduled-events)\.   
This example is also available in the [AWS SDK for JavaScript v3 developer guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/scheduled-events-invoking-lambda-example.html)\.  

**Services used in this example**
+ DynamoDB
+ EventBridge
+ Lambda
+ Amazon SNS