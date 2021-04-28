--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Registering certificate bundles in Node\.js<a name="node-registering-certs"></a>

The default trust stores for Node\.js include the certificates needed to access AWS services\. In some cases, it might be preferable to include only a specific set of certificates\.

In this example, a specific certificate on disk is used to create an `https.Agent` that rejects connections unless the designated certificate is provided\. The newly created `https.Agent` is then used by the DynamoDB client\.

```
const fs = require("fs"); 
const https = require("https");
const { DynamoDBClient } = require("@aws-sdk/client-dynamodb");

const certs = [
  fs.readFileSync("/path/to/cert.pem")
];
    
const dynamodbClient = new DynamoDBClient({
  httpOptions: {
    agent: new https.Agent({
      rejectUnauthorized: true,
      ca: certs
    })
  }
});
```