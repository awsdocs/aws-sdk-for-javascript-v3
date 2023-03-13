--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Capturing Webpage Scroll Progress with Amazon Kinesis<a name="kinesis-examples-capturing-page-scrolling"></a>

In this example, a simple HTML page simulates the content of a blog page\. As the reader scrolls the simulated blog post, the browser script uses the SDK for JavaScript to record the scroll distance down the page and send that data to Kinesis using the [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-kinesis/classes/kinesis.html#putrecords](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-kinesis/classes/kinesis.html#putrecords) method of the Kinesis client class\. The streaming data captured by Amazon Kinesis Data Streams can then be processed by Amazon EC2 instances and stored in any of several data stores including Amazon DynamoDB and Amazon Redshift\.

**To build the app:**

1. [Complete prerequisite tasks ](kinesis-page-scrolling-prerequisites.md)

1. [Create the AWS resources ](kinesis-page-scrolling-provision-resources.md)

1. [Create the HTML ](kinesis-page-scrolling-create-html.md)

1. [Prepare the browser script ](kinesis-page-scrolling-browser-script.md)

1. [Run the example](kinesis-page-scrolling-run.md)

1. [Delete the resources](kinesis-page-scrolling-destroy.md)