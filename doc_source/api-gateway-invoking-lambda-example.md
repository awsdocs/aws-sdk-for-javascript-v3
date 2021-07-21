--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Invoking Lambda with API Gateway<a name="api-gateway-invoking-lambda-example"></a>

You can invoke an Lambda function by using Amazon API Gateway, which is an AWS service for creating, publishing, maintaining, monitoring, and securing REST, HTTP, and WebSocket APIs at scale\. API developers can create APIs that access AWS or other web services, as well as data stored in the AWS Cloud\. As an API Gateway developer, you can create APIs for use in your own client applications\. For more information, see [What is Amazon API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)\. 

AWS Lambda is a compute service that enables you to run code without provisioning or managing servers\. You can create Lambda functions in various programming languages\. For more information about AWS Lambda, see [What is AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)\.

In this example, you create a Lambda function by using the Lambda JavaScript runtime API\. This example invokes different AWS services to perform a specific use case\. For example, assume that an organization sends a mobile text message to its employees that congratulates them at the one year anniversary date, as shown in this illustration\.

![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

The example should take about 20 minutes to complete\.

This example shows you how to use JavaScript logic to create a solution that performs this use case\. For example, you'll learn how to read a database to determine which employees have reached the one year anniversary date, how to process the data, and send out a text message all by using a Lambda function\. Then you’ll learn how to use API Gateway to invoke this AWS Lambda function by using a Rest endpoint\. For example, you can invoke the Lambda function by using this curl command:

```
curl -XGET "https://xxxxqjko1o3.execute-api.us-east-1.amazonaws.com/cronstage/employee" 
```

This AWS tutorial uses an Amazon DynamoDB table named Employee that contains these fields\.
+ **id** \- the primary key for the table\.
+ **firstName** \- employee’s first name\.
+ **phone** \- employee’s phone number\.
+ **startDate** \- employee’s start date\.

![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

**Important**  
Cost to complete: The AWS services included in this document are included in the AWS Free Tier\. However, be sure to terminate all of the resources after you have completed this example to ensure that you are not charged\.

**To build the app:**

1. [Complete prerequisites ](api-gateway-invoking-lambda-provision-resources.md)

1. [Create the AWS resources ](api-gateway-invoking-lambda-provision-resources.md)

1. [Prepare the browser script ](api-gateway-invoking-lambda-browser-script.md)

1. [Create and upload Lambda function ](api-gateway-invoking-lambda-browser-script.md)

1. [Deploy the Lambda function ](api-gateway-invoking-lambda-deploy-function.md)

1. [Run the app](api-gateway-invoking-lambda-run.md)

1. [Delete the resources](api-gateway-invoking-lambda-destroy.md)