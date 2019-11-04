# Configuring maxSockets in Node\.js<a name="node-configuring-maxsockets"></a>

In Node\.js, you can set the maximum number of connections per origin\. If `maxSockets` is set, the low\-level HTTP client queues requests and assigns them to sockets as they become available\.

This lets you set an upper bound on the number of concurrent requests to a given origin at a time\. Lowering this value can reduce the number of throttling or timeout errors received\. However, it can also increase memory usage because requests are queued until a socket becomes available\.

The following example shows how to set `maxSockets` for a DynamoDB client\.

```
const dynamodb = require('aws-sdk/clients/dynamodb');
var https = require('https');
var agent = new https.Agent({
   maxSockets: 25
});

var dynamodbClient = new DynamoDB({
   apiVersion: '2012-08-10'
   httpOptions:{
      agent: agent
   }
});
```

When using the default of `https`, the SDK takes the `maxSockets` value from the `globalAgent`\. If the `maxSockets` value is not defined or is `Infinity`, the SDK assumes a `maxSockets` value of 50\.

For more information about setting `maxSockets` in Node\.js, see the [Node\.js online documentation](https://nodejs.org/dist/latest-v4.x/docs/api/http.html#http_agent_maxsockets)\.