--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Create the rest API<a name="api-gateway-invoking-lambda-run-create"></a>

You can use the API Gateway console to create a rest endpoint for the Lambda function\. Once done, you are able to invoke the Lambda function using a restful call\.



1. Sign in to the [Amazon API Gateway console](https://console.aws.amazon.com/apigateway)\.

1. Under Rest API, choose **Build**\.

1. Select **New API**\.  
![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

1. Specify **Employee** as the API name and provide a description\.  
![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

1. Choose **Create API**\.

1. Choose **Resources** under the **Employee** section\.  
![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

1. In the name field, specify **employees**\.

1. Choose **Create Resources**\.

1. From the **Actions** dropdown, choose **Create Resources**\.  
![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

1. Choose **/employees**, select **Create Method** from the **Actions**, then select **GET** from the drop\-down menu below **/employees**\. Choose the checkmark icon\.  
![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[DynamoDB table\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

1. Choose **Lambda function** and enter **mylambdafunction** as the Lambda function name\. Choose **Save **\.