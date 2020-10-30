--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Configuring proxies for Node\.js<a name="node-configuring-proxies"></a>

If you can't directly connect to the internet, the SDK for JavaScript supports use of HTTP or HTTPS proxies through a third\-party HTTP agent\.

To find a third\-party HTTP agent, search for "HTTP proxy" at [npm](https://www.npmjs.com/)\.

To install a third\-party HTTP agent proxy, enter the following at the command prompt, where *PROXY* is the name of the `npm` package\. 

```
npm install PROXY --save
```

To use a proxy in your application, use the `httpOptions` property, as shown in the following example for a DynamoDB client\. 

```
const proxy = require("proxy-agent");
const { NodeHttpHandler } = require("@aws-sdk/node-http-handler");

const dynamodbClient = new DynamoDBClient({
  requestHandler: new NodeHttpHandler({
    httpAgent: new Agent({ proxy: proxy("http://internal.proxy.com") }),
  }),
});
```