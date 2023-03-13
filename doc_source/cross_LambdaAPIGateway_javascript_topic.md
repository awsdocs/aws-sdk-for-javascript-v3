--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Use API Gateway to invoke a Lambda function<a name="cross_LambdaAPIGateway_javascript_topic"></a>

**SDK for JavaScript \(v3\)**  
 Shows how to create an AWS Lambda function by using the Lambda JavaScript runtime API\. This example invokes different AWS services to perform a specific use case\. This example demonstrates how to create a Lambda function invoked by Amazon API Gateway that scans an Amazon DynamoDB table for work anniversaries and uses Amazon Simple Notification Service \(Amazon SNS\) to send a text message to your employees that congratulates them at their one year anniversary date\.   
 For complete source code and instructions on how to set up and run, see the full example on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cross-services/lambda-api-gateway)\.   
This example is also available in the [AWS SDK for JavaScript v3 developer guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/api-gateway-invoking-lambda-example.html)\.  

**Services used in this example**
+ API Gateway
+ DynamoDB
+ Lambda
+ Amazon SNS