--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Configuring maxSockets in Node\.js<a name="node-configuring-maxsockets"></a>

In Node\.js, you can set the maximum number of connections per origin\. If `maxSockets` is set, the low\-level HTTP client queues requests and assigns them to sockets as they become available\.

This lets you set an upper bound on the number of concurrent requests to a given origin at a time\. Lowering this value can reduce the number of throttling or timeout errors received\. However, it can also increase memory usage because requests are queued until a socket becomes available\.

The following example shows how to set `maxSockets` for a DynamoDB client\.

```
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
import { NodeHttpHandler } from "@aws-sdk/node-http-handler";
import  https from "https";    
var agent = new https.Agent({
  maxSockets: 25
});

var dynamodbClient = new DynamoDBClient({
  requestHandler: new NodeHttpHandler({
    httpsAgent: agent
 })
});
```

When using the default of `https`, the SDK takes the `maxSockets` value from the `globalAgent`\. If the `maxSockets` value is not defined, the SDK assumes a `maxSockets` value of 50\.

For more information about setting `maxSockets` in Node\.js, see the [Node\.js online documentation](https://nodejs.org/dist/latest-v4.x/docs/api/http.html#http_agent_maxsockets)\.