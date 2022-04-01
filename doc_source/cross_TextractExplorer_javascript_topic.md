--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Create an Amazon Textract explorer application<a name="cross_TextractExplorer_javascript_topic"></a>

**SDK for JavaScript V3**  
 Shows how to use the AWS SDK for JavaScript to build a React application that uses Amazon Textract to extract data from a document image and display it in an interactive web page\. This example runs in a web browser and requires an authenticated Amazon Cognito identity for credentials\. It uses Amazon Simple Storage Service \(Amazon S3\) for storage, and for notifications it polls an Amazon Simple Queue Service \(Amazon SQS\) queue that is subscribed to an Amazon Simple Notification Service \(Amazon SNS\) topic\.   
 For complete source code and instructions on how to set up and run, see the full example on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cross-services/textract-react)\.   

**Services used in this example**
+ Amazon Cognito Identity
+ Amazon S3
+ Amazon SNS
+ Amazon SQS
+ Amazon Textract