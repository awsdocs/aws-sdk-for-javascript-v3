--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Creating the workflow<a name="serverless-step-functions-example-understand-workflow"></a>

This topic is part of a tutorial that demonstrates to to invoke Lambda functions using AWS Step Functions\. To start at the beginning of the tutorial, see [Creating AWS serverless workflows using AWS SDK for JavaScript](serverless-step-functions-example.md)\.

The following figure shows the workflow you’ll create with this tutorial\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

The following is what happens at each step in the workflow:

 \+ **Start** \- Initiates the workflow\.

 \+ **Open Case** – Handles a support ticket ID value by passing it to the workflow\.

 \+ **Assign Case** – Assigns the support case to an employee and stores the data in a DynamoDB table\.

 \+ **Send Email** – Sends the employee an email message by using the Amazon Simple Email Service \(Amazon SES\) to inform them there is a new ticket\.

 \+ **End** \- Stops the workflow\.

## Create a serverless workflow by using Step functions<a name="serverless-step-functions-example-create-step-functions"></a>

You can create a workflow that processes support tickets\. To define a workflow by using Step Functions, you create an Amazon States Language \(JSON\-based\) document to define your state machine\. An Amazon States Language document describes each step\. After you define the document, Step functions provides a visual representation of the workflow\. The following figure shows the Amazon States Language document and the visual representation of the workflow\.

Workflows can pass data between steps\. For example, the **Open Case** step processes a case ID value \(passed to the workflow\) and passes that value to the **Assign Case** step\. Later in this tutorial, you’ll create application logic in the Lambda function to read and process the data values\.

### To create a workflow<a name="serverless-step-functions-example-workflow"></a>

1. Open the [Amazon Web Services Console](https://us-west-2.console.aws.amazon.com/states/home)\.

1. Choose **Create State Machine**\.

1. Choose **Author with code snippets**\. In the **Type** area, choose **Standard**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

1. Specify the Amazon States Language document by entering the following code\.

   ```
   {
   "Comment": "A simple AWS Step Functions state machine that automates a call center support session.",
   "StartAt": "Open Case",
   "States": {
   "Open Case": {
   "Type": "Task",
   "Resource": "arn:aws:lambda:REGION:ACCOUNT_ID:function:FUNCTION_NAME",
   "Next": "Assign Case"
   },
   "Assign Case": {
   "Type": "Task",
   "Resource": "arn:aws:lambda:REGION:ACCOUNT_ID:function:FUNCTION_NAME",
   "Next": "Send Email"
   },
   "Send Email": {
   "Type": "Task",
   "Resource": "arn:aws:lambda:REGION:ACCOUNT_ID:function:FUNCTION_NAME",
   "End": true
   }
   }
   }
   ```
**Note**  
 Don’t worry about the errors related to the Lambda resource values\. You’ll update these values later in this tutorial\.

1. Choose **Next**\.

1. In the name field, enter **SupportStateMachine**\.

1. In the **Permission** section, choose **Choose an existing role**\.

1. Choose **workflow\-support** \(the IAM role that you created\)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

1. Choose **Create state machine**\. A message appears that states the state machine was successfully created\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)