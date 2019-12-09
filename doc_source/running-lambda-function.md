# Run the Lambda Function<a name="running-lambda-function"></a>

In this task, you run the application you created starting with [Tutorial: Creating and Using Lambda Functions](using-lambda-functions.md)\.

**To run the browser application**

1. Open a web browser\.

1. Point the browser at the URL for the Amazon S3 bucket that hosts the application\.  
![\[Slot machine web application running in a browser that invokes a Lambda function\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/app_02.png)![\[Slot machine web application running in a browser that invokes a Lambda function\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Slot machine web application running in a browser that invokes a Lambda function\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

1. Select the handle on the right side of the slot machine\. The wheels begin to spin as the browser script invokes the Lambda function to generate results for this turn\.  
![\[Web application running in a browser that invokes a Lambda function.\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/app_01.png)![\[Web application running in a browser that invokes a Lambda function.\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Web application running in a browser that invokes a Lambda function.\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

1. Once the Lambda function returns the spin results to the browser script, the browser script sets the game display to show the images that the Lambda function selected\.

1. Select the handle again to start another spin\.

## Tutorial Clean Up<a name="lambda-tutorial-cleanup"></a>

To avoid ongoing charges for the resources and services used in this tutorial, delete the following resources in their respective service consoles:
+ The Lambda function in the AWS Lambda console at [https://console\.aws\.amazon\.com/lambda/](https://console.aws.amazon.com/lambda/)\.
+ The DynamoDB table in the Amazon DynamoDB console at [https://console\.aws\.amazon\.com/dynamodb/](https://console.aws.amazon.com/dynamodb/)\.
+ The objects in the Amazon S3 bucket and the bucket itself in the Amazon S3 console at [https://console\.aws\.amazon\.com/s3/](https://console.aws.amazon.com/s3/)\.
+ The Amazon Cognito identity pool in the Amazon Cognito console at [https://console\.aws\.amazon\.com/cognito/](https://console.aws.amazon.com/cognito/)\.
+ The Lambda execution role in the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

Congratulations\! You have now finished the tutorial\.