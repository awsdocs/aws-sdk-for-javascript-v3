# Configuring Proxies for Node\.js<a name="node-configuring-proxies"></a>

If you can't directly connect to the internet, the SDK for JavaScript supports use of HTTP or HTTPS proxies through a third\-party HTTP agent\.

To find a third\-party HTTP agent, search for "HTTP proxy" at [npm](https://www.npmjs.com/)\.

To install a third\-party HTTP agent proxy, type the following at the command line, where *PROXY* is the name of the `npm` package\. 

```
npm install PROXY --save
```

To use a proxy in your application, use the `httpOptions` property, as shown in the following example for a DynamoDB client\. 

```
const proxy = require('proxy-agent')
const NodeHttpHandler = require("@aws-sdk/node-http-handler")

const dynamodbClient = new DynamoDBClient({
  requestHandler: new NodeHttpHandler({
    httpAgent: new Agent({proxy: proxy('http://internal.proxy.com')})
  })
})
```

\.