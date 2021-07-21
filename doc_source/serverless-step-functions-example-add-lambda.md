--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Add the Lambda functions to workflows<a name="serverless-step-functions-example-add-lambda"></a>

This topic is part of a tutorial that demonstrates to to invoke Lambda functions using AWS Step Functions\. To start at the beginning of the tutorial, see [Creating AWS serverless workflows using AWS SDK for JavaScript](serverless-step-functions-example.md)\.

1. Open the Lambda console\. Notice that you can view the Lambda Amazon Resource Name \(ARN\) value in the upper\-right corner\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

1. Copy the value and then paste it into step 1 of the Amazon States Language document, located in the Step Functions console\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

1. Update the Resource for the **Assign Case** and **Send Email** steps\. This is how you hook in Lambda functions created by using the AWS SDK for Java into a workflow created by using Step Functions\.