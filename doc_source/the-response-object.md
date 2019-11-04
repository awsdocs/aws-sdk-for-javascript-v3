# Handling Service Client Responses<a name="the-response-object"></a>

After a service client method has been called, it returns a response object instance of an interface with the name associated with the client method\. For example, if you use the *AbcCommand* client method, the response object is of *AbcOutput* \(interface\) type\.

## Accessing Data Returned in the Response<a name="response-data-property"></a>

The response object contains the data, as properties, returned by the service request\.

In the previous example, the `DescribeTableCommand` command returned the number of items in the table in the `ItemCount` property of the response\.

## Accessing Error Information<a name="response-error-property"></a>

If a command fails,it throws an exception\. You can handle the exception as you desire\.