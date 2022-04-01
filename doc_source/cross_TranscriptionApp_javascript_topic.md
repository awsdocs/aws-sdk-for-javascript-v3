--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Build an Amazon Transcribe app<a name="cross_TranscriptionApp_javascript_topic"></a>

**SDK for JavaScript V3**  
 Create an app that uses Amazon Transcribe to transcribe and display voice recordings in the browser\. The app uses two Amazon Simple Storage Service \(Amazon S3\) buckets, one to host the application code, and another to store transcriptions\. The app uses an Amazon Cognito user pool to authenticate your users\. Authenticated users have AWS Identity and Access Management \(IAM\) permissions to access the required AWS services\.   
 For complete source code and instructions on how to set up and run, see the full example on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cross-services/transcription-app)\.   
This example is also available in the [AWS SDK for JavaScript v3 developer guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/transcribe-app.html)\.  

**Services used in this example**
+ Amazon Cognito Identity
+ Amazon S3
+ Amazon Transcribe