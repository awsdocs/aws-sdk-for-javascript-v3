--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Build a transcription app with authenticated users<a name="transcribe-app"></a>

In this tutorial, you learn how to:
+ Implement authentication using an Amazon Cognito identity pool to accept users federated with a Amazon Cognito user pool\.
+ Use Amazon Transcribe to transcribe and display voice recordings in the browser\.

## The scenario<a name="transcribe-app-scenario"></a>

The app enables users to sign up with a unique email and username\. On confirmation of their email, they can record voice messages that are automatically transcribed and displayed in the app\.

### How it works<a name="transcribe-app-how-it-works"></a>

The app uses two Amazon S3 buckets, one to host the application code, and another to store transcriptions\. The app uses an Amazon Cognito user pool to authenticate your users\. Authenticated users have IAM permissions to access the required AWS services\. 

The first time a user records a voice message, Amazon S3 creates a unique folder with the user’s name in the Amazon S3 bucket for storing transcriptions\. Amazon Transcribe transcribes the voice message to text, and saves it in JSON in the user’s folder\. When the user refreshes the app, their transcriptions are displayed and available for downloading or deletion\. 

The tutorial should take about 30 minutes to complete\.