--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Handling service client responses<a name="the-response-object"></a>

After a service client method has been called, it returns a response object instance of an interface with the name associated with the client method\. For example, if you use the *AbcCommand* client method, the response object is of *AbcResponse* \(interface\) type\.

## Accessing data returned in the response<a name="response-data-property"></a>

The response object contains the data, as properties, returned by the service request\.

In [Creating service client requests](the-request-object.md), the `ListTablesCommand` command returned the table names in the `TableNames` property of the response\.

## Accessing error information<a name="response-error-property"></a>

If a command fails, it throws an exception\. You can handle the exception as you need\.