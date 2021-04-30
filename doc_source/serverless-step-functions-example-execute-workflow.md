--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Execute your workflow by using the Step Functions console<a name="serverless-step-functions-example-execute-workflow"></a>

This topic is part of a tutorial that demonstrates to to invoke Lambda functions using AWS Step Functions\. To start at the beginning of the tutorial, see [Creating AWS serverless workflows using AWS SDK for JavaScript](serverless-step-functions-example.md)\.

You can invoke the workflow on the Step Functions console\. An execution receives JSON input\. For this example, you can pass the following JSON data to the workflow\.

```
                {
                "inputCaseID": "001"
                }
```

To execute your workflow:

1. On the Step Functions console, choose **Start execution**\.

1. In the **Input** section, pass the JSON data\. View the workflow\. As each step is completed, it turns green\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/lambda1.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

1. If the step turns red, an error occurred\. You can click the step and view the logs that are accessible from the right side\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/lambda14.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

   When the workflow is finished, you can view the data in the DynamoDB table\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/lambda15.png)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

Congratulations, you have created an AWS serverless workflow by using the AWS SDK for Java\. As stated at the beginning of this tutorial, be sure to terminate all of the resources you create while going through this tutorial to ensure that you’re no longer charged\.

For more AWS SDK for JavaScript cross\-service examples, see>[Cross\-service examples for the AWS SDK for JavaScript](tutorials.md)\.