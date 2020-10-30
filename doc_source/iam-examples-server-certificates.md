--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Working with IAM server certificates<a name="iam-examples-server-certificates"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to carry out basic tasks in managing server certificates for HTTPS connections\.

## The scenario<a name="iam-examples-server-certificates-scenario"></a>

To enable HTTPS connections to your website or application on AWS, you need an SSL/TLS *server certificate*\. To use a certificate that you obtained from an external provider with your website or application on AWS, you must upload the certificate to IAM or import it into AWS Certificate Manager\.

In this example, a series of Node\.js modules are used to handle server certificates in IAM\. The Node\.js modules use the SDK for JavaScript to manage server certificates using these methods of the `IAM` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#listServerCertificates-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#listServerCertificates-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#getServerCertificate-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#getServerCertificate-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#updateServerCertificate-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#updateServerCertificate-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#deleteServerCertificate-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/IAM.html#deleteServerCertificate-property)

For more information about server certificates, see [Working with server certificates](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_server-certs.html) in the *IAM User Guide*\.

## Prerequisite tasks<a name="iam-examples-server-certificates-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript is a super\-set of JavaScript so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Listing your server certificates<a name="iam-examples-server-certificates-listing"></a>

Create a Node\.js module with the file name `iam_listservercerts.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access IAM, create an `IAM` client service object\. Call the `ListServerCertificatesCommand` method of the `IAM` client service object\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  IAMClient,
  ListServerCertificatesCommand
} = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Create IAM service object
const iam = new IAMClient(REGION);

const run = async () => {
  try {
    const data = await iam.send(new ListServerCertificatesCommand({}));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node iam_listservercers.ts// If you prefer JavaScript, enter 'node ddb_batchgetitem.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/iam_listservercerts.ts)\.

## Getting a server certificate<a name="iam-examples-server-certificates-getting"></a>

Create a Node\.js module with the file name `iam_getservercert.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access IAM, create an `IAM` client service object\. Create a JSON object containing the parameters needed get a certificate, which consists of the name of the server certificate\. Call the `GetServerCertificatesCommand` method of the `IAM` client service object\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  IAMClient,
  GetServerCertificateCommand
} = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { ServerCertificateName: "CERTIFICATE_NAME" }; //CERTIFICATE_NAME

// Create IAM service object
const iam = new IAMClient(REGION);

const run = async () => {
  try {
    const data = await iam.send(new GetServerCertificateCommand(params));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
```

To run the example, enter the following at the command prompt\.

```
ts-node iam_getservercert.ts // If you prefer JavaScript, enter 'node iam_getservercert.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/iam_getservercert.ts)\.

## Updating a server certificate<a name="iam-examples-server-certificates-updating"></a>

Create a Node\.js module with the file name `iam_updateservercert.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access IAM, create an `IAM` client service object\. Create a JSON object containing the parameters needed to update a certificate, which consists of the name of the existing server certificate as well as the name of the new certificate\. Call the `UpdateServerCertificateCommand` method of the `IAM` client service object\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *CERTIFICATE\_NAME* with the service certicate name to update, and *NEW\_CERTIFICATE\_NAME* with the new certificate name\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  IAMClient,
  UpdateServerCertificateCommand,
} = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  ServerCertificateName: "CERTIFICATE_NAME", //CERTIFICATE_NAME
  NewServerCertificateName: "NEW_CERTIFICATE_NAME", //NEW_CERTIFICATE_NAME
};

// Create IAM service object
const iam = new IAMClient(REGION);

const run = async () => {
  try {
    const data = await iam.send(new UpdateServerCertificateCommand(params));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
 ts-node iam_updateservercert.ts // If you prefer JavaScript, enter 'node ddb_batchgetitem.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/iam_updateservercert.ts)\.

## Deleting a server certificate<a name="iam-examples-server-certificates-deleting"></a>

Create a Node\.js module with the file name `iam_deleteservercert.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access IAM, create an `IAM` client service object\. Create a JSON object containing the parameters needed to delete a server certificate, which consists of the name of the certificate to delete\. Call the `DeleteServerCertificatesCommand` method of the `IAM` client service object\.

**Note**  
Replace *REGION* with your AWS Region, and *CERTIFICATE\_NAME* with the name of the server certificate to delete\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  IAMClient,
  DeleteServerCertificateCommand,
} = require("@aws-sdk/client-iam");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { ServerCertificateName: "CERTIFICATE_NAME" }; // CERTIFICATE_NAME

// Create IAM service object
const iam = new IAMClient(REGION);

const run = async () => {
  try {
    const data = await iam.send(new DeleteServerCertificateCommand(params));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node iam_deleteservercert.ts // If you prefer JavaScript, enter 'node ddb_batchgetitem.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/iam_deleteservercert.ts)\.