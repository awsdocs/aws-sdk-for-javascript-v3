--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Working with services in the SDK for JavaScript<a name="working-with-services"></a>

The AWS SDK for JavaScript provides access to services that it supports through a collection of client classes\. From these client classes, you create service interface objects, commonly called *service objects*\. Each supported AWS service has one or more client classes that offer low\-level APIs for using service features and resources\. For example, Amazon DynamoDB APIs are available through the `DynamoDB` class\.

The services exposed through the SDK for JavaScript follow the request\-response pattern to exchange messages with calling applications\. In this pattern, the code invoking a service submits an HTTP/HTTPS request to an endpoint for the service\. The request contains parameters needed to successfully invoke the specific feature being called\. The service that is invoked generates a response that is sent back to the requestor\. The response contains data if the operation was successful or error information if the operation was unsuccessful\. 



Invoking an AWS service includes the full request and response lifecycle of an operation on a service object, including any retries that are attempted\. A request contains zero or more properties as JSON parameters\. The response is encapsulated in an object related to the operation, and is returned to the requestor through one of several techniques, such as a callback function or a JavaScript promise\.

**Topics**
+ [Creating and calling service objects](creating-and-calling-service-objects.md)
+ [Calling services asychronously](calling-services-asynchronously.md)
+ [Creating service client requests](the-request-object.md)
+ [Handling service client responses](the-response-object.md)
+ [Working with JSON](working-with-json.md)