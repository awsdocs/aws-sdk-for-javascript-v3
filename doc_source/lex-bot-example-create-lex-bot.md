--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Create an Amazon Lex bot<a name="lex-bot-example-create-lex-bot"></a>

**Important**  
Use V1 of the the Amazon Lex console to create the bot\. This exmaple does not work with bots created using V2\.

The first step is to create an Amazon Lex chatbot by using the Amazon Web Services Management Console\. In this example, the Amazon Lex **BookTrip** example is used\. For more information, see [Book Trip](https://docs.aws.amazon.com/lex/latest/dg/ex-book-trip.html)\.
+ Sign in to the Amazon Web Services Management Console and open the Amazon Lex console at [Amazon Web Services Console](https://console.aws.amazon.com/lex/)\.
+ On the Bots page, choose **Create**\.
+ Choose **BookTrip** blueprint \(leave the default bot name **BookTrip**\)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)
+ Fill in the default settings and choose **Create** \(the console shows the **BookTrip** bot\)\. On the Editor tab, review the details of the preconfigured intents\.
+ Test the bot in the test window\. Start the test by typing *I want to book a hotel room*\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)
+ Choose **Publish** and specify an alias name \(you will need this value when using the AWS SDK for JavaScript\)\.

**Note**  
 You need to reference the **bot name** and the **bot alias** in your JavaScript code\.