--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Run the app<a name="transcribe-app-run"></a>

This topic is part of a tutorial about building an app that transcribes and displays voice messages for authenticated users\. To start at the beginning of the tutorial, see [Build a transcription app with authenticated users](transcribe-app.md)\. 

You can the view the app at the location below\.

```
DOMAIN/login?client_id=APP_CLIENT_ID&response_type=token&scope=aws.cognito.signin.user.admin+email+openid+phone+profile&redirect_uri=REDIRECT_URL
```

Amazon Cognito makes it easy to run the app by providing a link in the AWS Web Services Management Console\. Simply navigate to the App client setting of your Amazon Cognito user pool, and select the **Launch Hosted UI**\. The URL for the app has the following format\.

**Important**  
The Hosted UI defaults to a response type of 'code'\. However, this tutorial is designed for the 'token' response type, so you have to change it\.