# Tutorial: Creating and Using Lambda Functions<a name="using-lambda-functions"></a>

In this tutorial, you learn how to:
+ Create AWS Lambda functions in Node\.js and call them from JavaScript running in a web browser\.
+ Call another service within a Lambda function and process the asynchronous responses before forwarding those responses to the browser script\.
+ Use Node\.js scripts to create the resources needed by the Lambda function\.

## The Scenario<a name="using-lambda-scenario"></a>

In this example, a simulated browser\-based slot machine game invokes a Lambda function that generates the random results of each slot pull\. Those results are returned as the file names of the images that are used to display to the user\. The images are stored in an Amazon S3 bucket that is configured to function as a static web host for the HTML, CSS, and other assets used to present the application experience\.

This diagram illustrates most of the elements in this application and how they relate to one another\. Versions of this diagram will appear throughout the tutorial to show the focus of each task

![\[JavaScript running in a browser that invokes a Lambda function\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/browser-calls-lambda-function.png)![\[JavaScript running in a browser that invokes a Lambda function\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[JavaScript running in a browser that invokes a Lambda function\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

## Prerequisites<a name="using-lambda-prerequisites"></a>

You must complete the following tasks before you can begin the tutorial:
+ Install Node\.js on your computer to run various scripts that help set up the Amazon S3 bucket and the Amazon DynamoDB table, and create and configure the Lambda function\. The Lambda function itself runs in the AWS Lambda Node\.js environment\. For information about installing Node\.js, see [www\.nodejs\.org](http://www.nodejs.org)\.
+ Install the AWS SDK for JavaScript on your computer to run the setup scripts\. For information on installing the AWS SDK for JavaScript for Node\.js, see [Installing the SDK for JavaScript](installing-jssdk.md)\.

The tutorial should take about 30 minutes to complete\.

## Tutorial Steps<a name="using-lambda-procedures"></a>

To create this application you'll need resources from multiple services that must be connected and configured in both the code of the browser script and the Node\.js code of the Lambda function\.

**To construct the tutorial application and the Lambda function it uses**

1. Create a working directory on your computer for this tutorial\.

   On Linux or Mac let's use `~/MyLambdaApp`; on Windows let's use `C:\MyLambdaApp`\. From now on we'll just call it `MyLambdaApp`\.

1. Download `slotassets.zip` from the [code example archive on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/lambda/tutorial/slotassets.zip)\. This archive contains the browser assets that are used by the application, the Node\.js code that's used in the Lambda function, and several setup scripts\. In this tutorial, you modify the `index.html` file and upload all the browser asset files to an Amazon S3 bucket you provision for this application\. As part of creating the Lambda function, you also modify the Node\.js code in `slotpull.js` before uploading it to the Amazon S3 bucket\.

   Unzip the contents of `slotassets.zip` as the directory `slotassets` in `MyLambdaApp`\. The `slotassets` directory should contain the 30 files\.

1. [Create an Amazon S3 bucket configured as a static website](using-lambda-s3-setup.md)\.

1. [Prepare the browser script](using-lambda-browser-script.md)\. Save the edited copy of `index.html` for upload to Amazon S3\.

1. [Create a Lambda execution role in IAM](using-lambda-iam-role-setup.md)\.

1. [Create and populate an Amazon DynamoDB table](using-lambda-ddb-setup.md)\.

1. [Prepare and create the Lambda function](using-lambda-function-prep.md)\.

1. [Run the Lambda function\.](running-lambda-function.md)