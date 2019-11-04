# Configuring Proxies for Node\.js<a name="node-configuring-proxies"></a>

If you can't directly connect to the internet, the SDK for JavaScript supports use of HTTP or HTTPS proxies through a third\-party HTTP agent\.

To find a third\-party HTTP agent, search for "HTTP proxy" at [npm](https://www.npmjs.com/)\.

To install a third\-party HTTP agent proxy, type the following at the command line, where *PROXY* is the name of the NPM package\. 

```
npm install PROXY --save
```

To use a proxy in your application, use the `httpOptions` property, as shown in the following example for a DynamoDB client\. 

```
var proxy = require('proxy-agent');
const dynamodb = require('aws-sdk/clients/dynamodb')
// http or https
const https = require('https');
const agent = new https.Agent({
  keepAlive: true
});

var dynamodbClient = new DynamoDB({
  httpOptions: {
    agent: proxy('http://internal.proxy.com')
  }
});
```

\.