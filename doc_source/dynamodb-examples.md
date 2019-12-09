# Amazon DynamoDB Examples<a name="dynamodb-examples"></a>

Amazon DynamoDB is a fully managed NoSQL cloud database that supports both document and key\-value store models\. You create schemaless tables for data without the need to provision or maintain dedicated database servers\.

![\[Relationship between JavaScript environments, the SDK, and DynamoDB\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/code-samples-dynamodb.png)

The JavaScript API for DynamoDB is exposed through the `AWS.DynamoDB`, `AWS.DynamoDBStreams`, and `AWS.DynamoDB.DocumentClient` client classes\. For more information about using the DynamoDB client classes, see [Class: AWS\.DynamoDB](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html), [Class: AWS\.DynamoDBStreams](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDBStreams.html), and [Class: AWS\.DynamoDB\.DocumentClient](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html) in the API reference\.

**Topics**
+ [Creating and Using Tables in DynamoDB](dynamodb-examples-using-tables.md)
+ [Reading and Writing a Single Item in DynamoDB](dynamodb-example-table-read-write.md)
+ [Reading and Writing Items in Batch in DynamoDB](dynamodb-example-table-read-write-batch.md)
+ [Querying and Scanning a DynamoDB Table](dynamodb-example-query-scan.md)
+ [Using the DynamoDB Document Client](dynamodb-example-document-client.md)