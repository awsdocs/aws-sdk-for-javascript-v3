--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Detect PPE in images with Amazon Rekognition using an AWS SDK<a name="cross_RekognitionPhotoAnalyzerPPE_javascript_topic"></a>

**SDK for JavaScript \(v3\)**  
 Shows how to use Amazon Rekognition with the AWS SDK for JavaScript to create an application to detect personal protective equipment \(PPE\) in images located in an Amazon Simple Storage Service \(Amazon S3\) bucket\. The app saves the results to an Amazon DynamoDB table, and sends the admin an email notification with the results using Amazon Simple Email Service \(Amazon SES\)\.   
Learn how to:  
+ Create an unauthenticated user using Amazon Cognito\.
+ Analyze images for PPE using Amazon Rekognition\.
+ Verify an email address for Amazon SES\.
+ Update a DynamoDB table with results\.
+ Send an email notification using Amazon SES\.
For complete source code and instructions on how to set up and run, see the full example on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cross-services/photo-analyzer-ppe)\.   

**Services used in this example**
+ DynamoDB
+ Amazon Rekognition
+ Amazon S3
+ Amazon SES