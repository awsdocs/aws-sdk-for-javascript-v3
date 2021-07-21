--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Building an Amazon Lex chatbot<a name="lex-bot-example"></a>

You can create an Amazon Lex chatbot within a web application to engage your web site visitors\. An Amazon Lex chatbot is functionality that performs on\-line chat conversation with users without providing direct contact with a person\. For example, the following illustration shows an Amazon Lex chatbot that engages a user about booking a hotel room\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

The Amazon Lex chatbot created in this AWS tutorial is able to handle multiple languages\. For example, a user who speaks French can enter French text and get back a response in French\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

Likewise, a user can communicate with the Amazon Lex chatbot in Italian\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

This AWS tutorial guides you through creating an Amazon Lex chatbot and integrating it into a Node\.js web application\. The AWS SDK for JavaScript \(version 3\) is used to invoke these AWS services:
+ Amazon Lex
+ Amazon Comprehend
+ Amazon Translate

**Cost to complete:** The AWS services included in this document are included in the [AWS Free Tier](https://aws.amazon.com/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc)\.

**Note:** Be sure to terminate all of the resources you create while going through this tutorial to ensure that you’re not charged\.

**To build the app:**

1. [Prerequisites](lex-bot-example-prerequisites.md)

1. [Provision resources](lex-bot-provision-resources.md)

1. [Create Amazon Lex chatbot](lex-bot-example-create-lex-bot.md)

1. [Create the HTML](lex-bot-example-html.md)

1. [Create the browser script](lex-bot-example-script.md)

1. [Next steps](lex-bot-example-next-steps.md)