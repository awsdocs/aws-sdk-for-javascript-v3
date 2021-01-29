--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Reusing connections with keep\-alive in Node\.js<a name="node-reusing-connections"></a>

The default Node\.js HTTP/HTTPS agent creates a new TCP connection for every new request\. To avoid the cost of establishing a new connection, the SDK for JavaScript reuses TCP connections\. 

For short\-lived operations, such as Amazon DynamoDB queries, the latency overhead of setting up a TCP connection might be greater than the operation itself\. Additionally, since DynamoDB [encryption at rest](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/encryption.howitworks.html) is integrated with [AWS KMS](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/encryption.howitworks.html), you may experience latencies from the database having to re\-establish new AWS KMS cache entries for each operation\. 

To disable reusing TCP connections, set the `AWS_NODEJS_CONNECTION_REUSE_ENABLED` environment variable to `false` \(the default is `true`\)\.

You can also disable keeping these connections alive on a per\-service client basis, as shown in the following example for a DynamoDB client\.

```
const { NodeHttpHandler} = require("@aws-sdk/node-http-handler");
const { Agent } = require("http");
const dynamodbClient = new DynamoDBClient({
  requestHandler: new NodeHttpHandler({
    httpAgent: new Agent({keepAlive: false})
  })
});
```

If `keepAlive` is enabled, you can also set the initial delay for TCP Keep\-Alive packets with `keepAliveMsecs`, which by default is 1000 ms\. See the [Node\.js documentation](https://nodejs.org/api/http.html) for details\.