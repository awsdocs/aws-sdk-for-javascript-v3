# Working with IAM Server Certificates<a name="iam-examples-server-certificates"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to carry out basic tasks in managing server certificates for HTTPS connections\.

## The Scenario<a name="iam-examples-server-certificates-scenario"></a>

To enable HTTPS connections to your website or application on AWS, you need an SSL/TLS *server certificate*\. To use a certificate that you obtained from an external provider with your website or application on AWS, you must upload the certificate to IAM or import it into AWS Certificate Manager\.

In this example, a series of Node\.js modules are used to handle server certificates in IAM\. The Node\.js modules use the SDK for JavaScript to manage server certificates using these methods of the `AWS.IAM` client class:
+ [listServerCertificates](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#listServerCertificates-property)
+ [getServerCertificate](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#getServerCertificate-property)
+ [updateServerCertificate](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#updateServerCertificate-property)
+ [deleteServerCertificate](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#deleteServerCertificate-property)

For more information about server certificates, see [Working with Server Certificates](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_server-certs.html) in the *IAM User Guide*\.

## Prerequisite Tasks<a name="iam-examples-server-certificates-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Listing Your Server Certificates<a name="iam-examples-server-certificates-listing"></a>

Create a Node\.js module with the file name `iam_listservercerts.js`\. Be sure to configure the SDK as previously shown\. To access IAM, create an `AWS.IAM` service object\. Call the `listServerCertificates` method of the `AWS.IAM` service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

iam.listServerCertificates({}, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node iam_listservercerts.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/iam/iam_listservercerts.js)\.

## Getting a Server Certificate<a name="iam-examples-server-certificates-getting"></a>

Create a Node\.js module with the file name `iam_getservercert.js`\. Be sure to configure the SDK as previously shown\. To access IAM, create an `AWS.IAM` service object\. Create a JSON object containing the parameters needed get a certificate, which consists of the name of the server certificate you want\. Call the `getServerCertificates` method of the `AWS.IAM` service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

iam.getServerCertificate({ServerCertificateName: 'CERTIFICATE_NAME'}, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node iam_getservercert.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/iam/iam_getservercert.js)\.

## Updating a Server Certificate<a name="iam-examples-server-certificates-updating"></a>

Create a Node\.js module with the file name `iam_updateservercert.js`\. Be sure to configure the SDK as previously shown\. To access IAM, create an `AWS.IAM` service object\. Create a JSON object containing the parameters needed to update a certificate, which consists of the name of the existing server certificate as well as the name of the new certificate\. Call the `updateServerCertificate` method of the `AWS.IAM` service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

var params = {
  ServerCertificateName: 'CERTIFICATE_NAME',
  NewServerCertificateName: 'NEW_CERTIFICATE_NAME'
};

iam.updateServerCertificate(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node iam_updateservercert.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/iam/iam_updateservercert.js)\.

## Deleting a Server Certificate<a name="iam-examples-server-certificates-deleting"></a>

Create a Node\.js module with the file name `iam_deleteservercert.js`\. Be sure to configure the SDK as previously shown\. To access IAM, create an `AWS.IAM` service object\. Create a JSON object containing the parameters needed to delete a server certificate, which consists of the name of the certificate you want to delete\. Call the `deleteServerCertificates` method of the `AWS.IAM` service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the IAM service object
var iam = new AWS.IAM({apiVersion: '2010-05-08'});

iam.deleteServerCertificate({ServerCertificateName: 'CERTIFICATE_NAME'}, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node iam_deleteservercert.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/iam/iam_deleteservercert.js)\.