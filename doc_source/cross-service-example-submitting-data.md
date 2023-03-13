--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Build an app to submit data to DynamoDB<a name="cross-service-example-submitting-data"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This cross\-service Node\.js tutorial shows how to build an app that enables users to submit data to an Amazon DynamoDB table\. This app uses the following services:**
+ AWS Identity and Access Management \(IAM\) and Amazon Cognito for authorization and permissions\.
+ Amazon DynamoDB \(DynamoDB\) to create and update the tables\.
+ Amazon Simple Notification Service \(Amazon SNS\) to notify the app administrator when a user updates the table\.

## The scenario<a name="cross-service-example-submitting-data-scenario"></a>

In this tutorial, an HTML page provides a browser\-based application for submitting data to a Amazon DynamoDB table\. The app uses Amazon SNS to notify the app administrator when a user updates the table\.



**To build the app:**

1. [Prerequisites](s3-crossservices-adddata-prereqs.md)

1. [Provision resources](s3-crossservices-adddata-provision-resources.md)

1. [Create the HTML](s3-crossservices-adddata-front-end.md)

1. [Create the browser script](cross-service-submitdata-browser-script.md)

1. [Next steps](s3-crossservices-adddata-destroy.md)