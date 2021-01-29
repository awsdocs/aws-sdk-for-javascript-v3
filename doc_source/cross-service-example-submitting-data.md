--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Tutorial: Build an app to submit data to DynamoDB<a name="cross-service-example-submitting-data"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This cross\-service Node\.js tutorial shows how to build an app that enables users to submit data to an Amazon DynamoDB table\. This app uses the following services:**
+ AWS Identity and Access Management \(IAM\) and Amazon Cognito for authorization and permissions\.
+ Amazon Simple Storage Service \(Amazon S3\) to host the app\.
+ Amazon DynamoDB \(DynamoDB\) to create and update the tables\.
+ Amazon Simple Notification Service \(Amazon SNS\) to notify the app administrator when a user updates the table\.

## The scenario<a name="cross-service-example-submitting-data-scenario"></a>

In this tutorial, an HTML page provides a browser\-based application for submitting data to a Amazon DynamoDB table\. The table is hosted as a static website on Amazon S3, and uses Amazon SNS to notify the app administrator when a user updates the table\.

![\[Relationship among the browser interface, the SDK, and AWS services in the submitting data app.\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/submitting_data.png)

## Prerequisites<a name="s3-crossservices-adddata-prereqs"></a>

Complete the following prerequisite tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/cross-services/submit-data-app/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypeScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so with minor adjustments these examples can also be run in JavaScript\. For more information, see [this article](https://aws.amazon.com/blogs/developer/first-class-typescript-support-in-modular-aws-sdk-for-javascript/) in the AWS Developer Blog\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Tutorial steps<a name="s3-crossservice-tutorial-steps"></a>

To create this application, you need resources from multiple services that must be connected and configured in both the code of the browser script and the Node\.js code\.

**To construct the tutorial application**

1. [Create an Amazon Cognito identity pool with an unauthenticated identity\.](s3-crossservices-adddata-create-idpool.md)

1. [Create an Amazon S3 bucket\.](s3-crossservices-adddata-create-bucket.md)

1. [Create the DynamoDB table\.](s3-crossservices-adddata-create-table.md)

1. [Create the HTML front\-end page for the app\.](cross-service-submitdata-front-end.md)

1. [Create the browser script to run the app\.](cross-service-submitdata-browser-script.md)

1. [Upload the app to the Amazon S3 bucket, and convert it into a static web host\.](cross-service-submitdata-create-website.md)

1. [Run the app in your browser\.](cross-service-submitdata-run-app.md)