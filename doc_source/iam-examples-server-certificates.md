--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Working with IAM server certificates<a name="iam-examples-server-certificates"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to carry out basic tasks in managing server certificates for HTTPS connections\.

## The scenario<a name="iam-examples-server-certificates-scenario"></a>

To enable HTTPS connections to your website or application on AWS, you need an SSL/TLS *server certificate*\. To use a certificate that you obtained from an external provider with your website or application onAWS, you must upload the certificate to IAM or import it into AWS Certificate Manager\.

In this example, a series of Node\.js modules are used to handle server certificates in IAM\. The Node\.js modules use the SDK for JavaScript to manage server certificates using these methods of the `IAM` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/listservercertificatescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/listservercertificatescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/getservercertificatecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/getservercertificatecommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/updateservercertificatecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/updateservercertificatecommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/deleteservercertificatecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-iam/classes/deleteservercertificatecommand.html)

For more information about server certificates, see [Working with server certificates](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_server-certs.html) in the *IAM User Guide*\.

## Prerequisite tasks<a name="iam-examples-server-certificates-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Listing your server certificates<a name="iam-examples-server-certificates-listing"></a>

Create a `libs` directory, and create a Node\.js module with the file name `iamClient.js`\. Copy and paste the code below into it, which creates the IAM client object\. Replace *REGION* with your AWS Region\.

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/libs/iamClient.js)\.

Create a Node\.js module with the file name `iam_listservercerts.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. Call the `ListServerCertificatesCommand` method of the `IAM` client service object\.

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { ListServerCertificatesCommand } from "@aws-sdk/client-iam";

export const run = async () => {
  try {
    const data = await iamClient.send(new ListServerCertificatesCommand({}));
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node iam_listservercers.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_listservercerts.js)\.

## Getting a server certificate<a name="iam-examples-server-certificates-getting"></a>

Create a `libs` directory, and create a Node\.js module with the file name `iamClient.js`\. Copy and paste the code below into it, which creates the IAM client object\. Replace *REGION* with your AWS Region\.

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/libs/iamClient.js)\.

Create a Node\.js module with the file name `iam_getservercert.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. Create a JSON object containing the parameters needed get a certificate, which consists of the name of the server certificate\. Call the `GetServerCertificatesCommand` method of the `IAM` client service object\.

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { GetServerCertificateCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = { ServerCertificateName: "CERTIFICATE_NAME" }; //CERTIFICATE_NAME

export const run = async () => {
  try {
    const data = await iamClient.send(new GetServerCertificateCommand(params));
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
```

To run the example, enter the following at the command prompt\.

```
node iam_getservercert.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_getservercert.js)\.

## Updating a server certificate<a name="iam-examples-server-certificates-updating"></a>

Create a `libs` directory, and create a Node\.js module with the file name `iamClient.js`\. Copy and paste the code below into it, which creates the IAM client object\. Replace *REGION* with your AWS Region\.

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/libs/iamClient.js)\.

Create a Node\.js module with the file name `iam_updateservercert.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. Create a JSON object containing the parameters needed to update a certificate, which consists of the name of the existing server certificate as well as the name of the new certificate\. Call the `UpdateServerCertificateCommand` method of the `IAM` client service object\.

**Note**  
Replace *CERTIFICATE\_NAME* with the service certificate name to update, and *NEW\_CERTIFICATE\_NAME* with the new certificate name\.

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { UpdateServerCertificateCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = {
  ServerCertificateName: "CERTIFICATE_NAME", //CERTIFICATE_NAME
  NewServerCertificateName: "NEW_CERTIFICATE_NAME", //NEW_CERTIFICATE_NAME
};

export const run = async () => {
  try {
    const data = await iamClient.send(
      new UpdateServerCertificateCommand(params)
    );
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
 node iam_updateservercert.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_updateservercert.js)\.

## Deleting a server certificate<a name="iam-examples-server-certificates-deleting"></a>

Create a `libs` directory, and create a Node\.js module with the file name `iamClient.js`\. Copy and paste the code below into it, which creates the IAM client object\. Replace *REGION* with your AWS Region\.

```
import { IAMClient } from "@aws-sdk/client-iam";
// Set the AWS Region.
const REGION = "REGION"; // For example, "us-east-1".
// Create an IAM service client object.
const iamClient = new IAMClient({ region: REGION });
export { iamClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/libs/iamClient.js)\.

Create a Node\.js module with the file name `iam_deleteservercert.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. Create a JSON object containing the parameters needed to delete a server certificate, which consists of the name of the certificate to delete\. Call the `DeleteServerCertificatesCommand` method of the `IAM` client service object\.

**Note**  
Replace *CERTIFICATE\_NAME* with the name of the server certificate to delete\.

```
// Import required AWS SDK clients and commands for Node.js.
import { iamClient } from "./libs/iamClient.js";
import { DeleteServerCertificateCommand } from "@aws-sdk/client-iam";

// Set the parameters.
export const params = { ServerCertificateName: "CERTIFICATE_NAME" }; // CERTIFICATE_NAME

export const run = async () => {
  try {
    const data = await iamClient.send(
      new DeleteServerCertificateCommand(params)
    );
    console.log("Success", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node iam_deleteservercert.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/iam/src/iam_deleteservercert.js)\.