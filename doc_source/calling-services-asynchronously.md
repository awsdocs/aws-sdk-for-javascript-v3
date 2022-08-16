--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Calling services asynchronously<a name="calling-services-asynchronously"></a>

All requests made through the SDK are asynchronous\. This is important to keep in mind when writing browser scripts\. JavaScript running in a web browser typically has just a single execution thread\. After making an asynchronous call to an AWS service, the browser script continues running and in the process can try to execute code that depends on that asynchronous result before it returns\.

Making asynchronous calls to an AWS service includes managing those calls so your code doesn't try to use data before the data is available\. The topics in this section explain the need to manage asynchronous calls and detail different techniques you can use to manage them\.

Although you can use any of these techniques to manage asynchronous calls, we recommend that you use async/await for all new code\.

async/await  
We recommend that you use this technique as it is the default behavior in V3\.

promise  
Use this technique in browsers that do not support async/await\.

callback  
Avoid using callbacks except in very simple cases\. However, you might find it useful for migration scenarios\.

**Topics**
+ [Managing asynchronous calls](making-asynchronous-calls.md)
+ [Using async/await](using-async-await.md)
+ [Using JavaScript promises](using-promises.md)
+ [Using an anonymous callback function](using-a-callback-function.md)