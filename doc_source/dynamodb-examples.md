--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Amazon DynamoDB examples<a name="dynamodb-examples"></a>

Amazon DynamoDB is a fully managed NoSQL cloud database that supports both document and key\-value store models\. You create schemaless tables for data without the need to provision or maintain dedicated database servers\.

![\[Relationship between JavaScript environments, the SDK, and DynamoDB\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/code-samples-dynamodb.png)

The JavaScript API for DynamoDB is exposed through the `DynamoDB`, `DynamoDBStreams`, and `DynamoDB.DocumentClient` client classes\. For more information about using the DynamoDB client classes, see [Class: DynamoDB](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/dynamodb.html), [Class: DynamoDBStreams](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb-streams/classes/dynamodbstreams.html), and [Class: DynamoDB utility](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/modules/_aws_sdk_util_dynamodb.html) in the API Reference\.

**Topics**
+ [Creating and using tables in DynamoDB](dynamodb-examples-using-tables.md)
+ [Reading and writing a single item in DynamoDB](dynamodb-example-table-read-write.md)
+ [Reading and writing items in batch in DynamoDB](dynamodb-example-table-read-write-batch.md)
+ [Querying and scanning a DynamoDB table](dynamodb-example-query-scan.md)
+ [Using the DynamoDB Utilities](dynamodb-example-dynamodb-utilities.md)