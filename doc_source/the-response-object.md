--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Handling service client responses<a name="the-response-object"></a>

After a service client method has been called, it returns a response object instance of an interface with the name associated with the client method\. For example, if you use the *AbcCommand* client method, the response object is of *AbcResponse* \(interface\) type\.

## Accessing data returned in the response<a name="response-data-property"></a>

The response object contains the data, as properties, returned by the service request\.

In [Creating service client requests](the-request-object.md), the `ListTablesCommand` command returned the table names in the `TableNames` property of the response\.

## Accessing error information<a name="response-error-property"></a>

If a command fails, it throws an exception\. You can handle the exception as you need\.