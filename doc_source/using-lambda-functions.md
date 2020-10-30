--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Tutorial: Creating and using Lambda functions<a name="using-lambda-functions"></a>

In this tutorial, you learn how to:
+ Create AWS Lambda functions in Node\.js and call them from JavaScript running in a web browser\.
+ Call another service within a Lambda function and process the asynchronous responses before forwarding those responses to the browser script\.
+ Use Node\.js scripts to create the resources needed by the Lambda function\.

## The scenario<a name="using-lambda-scenario"></a>

In this example, a simulated browser\-based slot machine game invokes a Lambda function that generates the random results of each slot pull\. Those results are returned as the file names of the images that are used to display to the user\. The images are stored in an Amazon S3 bucket that is configured to function as a static web host for the HTML, CSS, and other assets used to present the application experience\.

This diagram illustrates most of the elements in this application and how they relate to one another\. Versions of this diagram will appear throughout the tutorial to show the focus of each task\.

![\[JavaScript running in a browser that invokes a Lambda function\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/browser-calls-lambda-function.png)![\[JavaScript running in a browser that invokes a Lambda function\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[JavaScript running in a browser that invokes a Lambda function\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

## Prerequisites<a name="using-lambda-prerequisites"></a>

Before you begin the tutorial, set up a project environment to run Node TypeScript examples\. For more information, see[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/lambda/README.md)\. 

This environment contains the `MyLambdaApp` directory that contains browser assets that are used by the application, the `slotpull.js` file that contains the Node\.js code that's used in the Lambda function, and several setup scripts\. In this tutorial, you run the setup scripts, use `webpack` module bundler to parse your application code, and upload all the browser asset files to an Amazon S3 bucket you provision for this application\. As part of creating the Lambda function, you also modify the Node\.js code in `slotpull.js` before uploading it to the Amazon S3 bucket\.

**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so with minor adjustments these example can also be run in JavaScript\.

The tutorial should take about 30 minutes to complete\.

**Note**  
The code examples in this tutorial import and use the required AWS Service V3 package clients, V3 commands, and use the `send` method in an async/await pattern\. You can create these examples using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

## Tutorial steps<a name="using-lambda-procedures"></a>

To create this application you'll need resources from multiple services that must be connected and configured in both the code of the browser script and the Node\.js code of the Lambda function\.

**To construct the tutorial application and the Lambda function it uses**

1. [Create an Amazon S3 bucket configured as a static website](using-lambda-s3-setup.md)\.

1. [Prepare the browser script code](using-lambda-browser-script.md)\.

1. [Create a Lambda execution role in IAM](using-lambda-iam-role-setup.md)\.

1. [Create and populate an Amazon DynamoDB table](using-lambda-ddb-setup.md)\.

1. [Prepare and create the Lambda function](using-lambda-function-prep.md)\.

1. [Run the Lambda function\.](running-lambda-function.md)