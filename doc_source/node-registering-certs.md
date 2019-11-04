# Registering Certificate Bundles in Node\.js<a name="node-registering-certs"></a>

The default trust stores for Node\.js include the certificates needed to access AWS services\. In some cases, it might be preferable to include only a specific set of certificates\.

In this example, a specific certificate on disk is used to create an `https.Agent` that rejects connections unless the designated certificate is provided\. The newly created `https.Agent` is then used by the DynamoDB client\.

```
var fs = require('fs'); 
var https = require('https');
const dynamodb = require('aws-sdk/clients/dynamodb')

var certs = [
  fs.readFileSync('/path/to/cert.pem')
];
    
var dynamodbClient = new DynamoDB({
  httpOptions: {
    agent: new https.Agent({
      rejectUnauthorized: true,
      ca: certs
    })
  }
});
```