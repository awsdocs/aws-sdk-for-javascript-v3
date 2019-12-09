# Handling Service Client Responses<a name="the-response-object"></a>

After a service client method has been called, it returns a response object instance of an interface with the name associated with the client method\. For example, if you use the *AbcCommand* client method, the response object is of *AbcResponse* \(interface\) type\.

## Accessing Data Returned in the Response<a name="response-data-property"></a>

The response object contains the data, as properties, returned by the service request\.

In [Creating Service Client Requests](the-request-object.md), the `ListTablesCommand` command returned the table names in the `TableNames` property of the response\.

## Accessing Error Information<a name="response-error-property"></a>

If a command fails, it throws an exception\. You can handle the exception as you need\.