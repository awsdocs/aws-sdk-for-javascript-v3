--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# The Scenario<a name="kinesis-examples-capturing-page-scrolling-scenario"></a>

In this example, a simple HTML page simulates the content of a blog page\. As the reader scrolls the simulated blog post, the browser script uses the SDK for JavaScript to record the scroll distance down the page and send that data to Kinesis using the [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Kinesis.html#putRecords-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Kinesis.html#putRecords-property) method of the Kinesis client class\. The streaming data captured by Amazon Kinesis Data Streams can then be processed by Amazon EC2 instances and stored in any of several data stores including Amazon DynamoDB and Amazon Redshift\.

![\[JavaScript in a browser script sending scroll data to Kinesis.\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/kinesis-examples.png)