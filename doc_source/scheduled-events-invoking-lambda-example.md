--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Creating scheduled events to execute AWS Lambda functions<a name="scheduled-events-invoking-lambda-example"></a>

You can create a scheduled event that invokes an Lambda function by using an Amazon CloudWatch Event\. You can configure a CloudWatch Event to use a cron expression to schedule when a Lambda function is invoked\. For example, you can schedule a CloudWatch Event to invoke an Lambda function every weekday\. Lambda is a compute service that enables you to run code without provisioning or managing servers\.

AWS Lambda is a compute service that enables you to run code without provisioning or managing servers\. You can create Lambda functions in various programming languages\. For more information about AWS Lambda, see [What is AWS Lambda](https://docs.aws.amazon.com/lambda/latest/dg/welcome.html)\.

In this tutorial, you create a Lambda function by using the Lambda JavaScript runtime API\. This example invokes different AWS services to perform a specific use case\. For example, assume that an organization sends a mobile text message to its employees that congratulates them at the one year anniversary date, as shown in this illustration\.

![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

The tutorial should take about 20 minutes to complete\.

This tutorial shows you how to use JavaScript logic to create a solution that performs this use case\. For example, you'll learn how to read a database to determine which employees have reached the one year anniversary date, how to process the data, and send out a text message all by using a Lambda function\. Then you’ll learn how to use a cron expression to invoke the Lambda function every weekday\.

This AWS tutorial uses an Amazon DynamoDB table named Employee that contains these fields\.
+ **id** \- the primary key for the table\.
+ **firstName** \- employee’s first name\.
+ **phone** \- employee’s phone number\.
+ **startDate** \- employee’s start date\.

![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

**Important**  
Cost to complete: The AWS services included in this document are included in the AWS Free Tier\. However, be sure to terminate all of the resources after you have completed this tutorial to ensure that you are not charged\.

**To build the app:**

1. [Complete prerequisites ](scheduled-events-invoking-lambda-provision-resources.md)

1. [Create the AWS resources ](scheduled-events-invoking-lambda-provision-resources.md)

1. [Prepare the browser script ](scheduled-events-invoking-lambda-browser-script.md)

1. [Create and upload Lambda function ](scheduled-events-invoking-lambda-browser-script.md)

1. [Deploy the Lambda function ](scheduled-events-invoking-lambda-deploy-function.md)

1. [Run the app](scheduled-events-invoking-lambda-run.md)

1. [Delete the resources](scheduled-events-invoking-lambda-destroy.md)