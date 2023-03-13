--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Managing Amazon SES identities<a name="ses-examples-managing-identities"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to verify email addresses and domains used with Amazon SES\.
+ How to assign an AWS Identity and Access Management \(IAM\) policy to your Amazon SES identities\.
+ How to list all Amazon SES identities for your AWS account\.
+ How to delete identities used with Amazon SES\.

An Amazon SES *identity* is an email address or domain that Amazon SES uses to send email\. Amazon SES requires you to verify your email identities, confirming that you own them and preventing others from using them\.

For details on how to verify email addresses and domains in Amazon SES, see [Verifying email addresses and domains in Amazon SES](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/verify-addresses-and-domains.html) in the Amazon Simple Email Service Developer Guide\. For information about sending authorization in Amazon SES, see [Overview of Amazon SES sending authorization](Amazon Simple Email Service Developer Guidesending-authorization-overview.html)\.

## The scenario<a name="ses-examples-verifying-identities-scenario"></a>

In this example, you use a series of Node\.js modules to verify and manage Amazon SES identities\. The Node\.js modules use the SDK for JavaScript to verify email addresses and domains, using these methods of the `SES` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/listidentitiescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/listidentitiescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/deleteidentitycommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/deleteidentitycommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/verifyemailidentitycommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/verifyemailidentitycommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/verifydomainidentitycommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/verifydomainidentitycommand.html)

## Prerequisite tasks<a name="ses-examples-verifying-identities-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/ses/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Listing your identities<a name="ses-examples-listing-identities"></a>

In this example, use a Node\.js module to list email addresses and domains to use with Amazon SES\.

Create a `libs` directory, and create a Node\.js module with the file name `sesClient.js`\. Copy and paste the code below into it, which creates the Amazon SES client object\. Replace *REGION* with your AWS Region\.

```
import { SESClient } from "@aws-sdk/client-ses";
// Set the AWS Region.
const REGION = "us-east-1";
// Create SES service object.
const sesClient = new SESClient({ region: REGION });
export { sesClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/libs/sesClient.js)\.

Create a Node\.js module with the file name `ses_listidentities.js`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the `IdentityType` and other parameters for the `ListIdentitiesCommand` method of the `SES` client class\. To call the `ListIdentitiesCommand` method, invoke an Amazon SES service object, passing the parameters object\. 

 The `data` returned contains an array of domain identities as specified by the `IdentityType` parameter\.

**Note**  
Replace *IDENTITY\_TYPE* with the identity type, which can be "EmailAddress" or "Domain"\.

```
import { ListIdentitiesCommand } from "@aws-sdk/client-ses";
import { sesClient } from "./libs/sesClient.js";

const createListIdentitiesCommand = () =>
  new ListIdentitiesCommand({ IdentityType: "EmailAddress", MaxItems: 10 });

const run = async () => {
  const listIdentitiesCommand = createListIdentitiesCommand();

  try {
    return await sesClient.send(listIdentitiesCommand);
  } catch (err) {
    console.log("Failed to list identities.", err);
    return err;
  }
};
```

To run the example, enter the following at the command prompt\.

```
node ses_listidentities.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_listidentities.js)\.

## Verifying an email address identity<a name="ses-examples-verifying-email"></a>

In this example, use a Node\.js module to verify email senders to use with Amazon SES\.

Create a `libs` directory, and create a Node\.js module with the file name `sesClient.js`\. Copy and paste the code below into it, which creates the Amazon SES client object\. Replace *REGION* with your AWS Region\.

```
import { SESClient } from "@aws-sdk/client-ses";
// Set the AWS Region.
const REGION = "us-east-1";
// Create SES service object.
const sesClient = new SESClient({ region: REGION });
export { sesClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/libs/sesClient.js)\.

Create a Node\.js module with the file name `ses_verifyemailidentity.js`\. Configure the SDK as previously shown, including downloading the required clients and packages\. 

Create an object to pass the `EmailAddress` parameter for the `VerifyEmailIdentityCommand` method of the `SES` client class\. To call the `VerifyEmailIdentityCommand` method, invoke an Amazon SES client service object, passing the parameters\. 

**Note**  
Replace *ADDRESS@DOMAIN\.EXT* with the email address, such as name@example\.com\.

```
// Import required AWS SDK clients and commands for Node.js
import { VerifyEmailIdentityCommand } from "@aws-sdk/client-ses";
import { sesClient } from "./libs/sesClient.js";

const EMAIL_ADDRESS = "name@example.com";

const createVerifyEmailIdentityCommand = (emailAddress) => {
  return new VerifyEmailIdentityCommand({ EmailAddress: emailAddress });
};

const run = async () => {
  const verifyEmailIdentityCommand =
    createVerifyEmailIdentityCommand(EMAIL_ADDRESS);
  try {
    return await sesClient.send(verifyEmailIdentityCommand);
  } catch (err) {
    console.log("Failed to verify email identity.", err);
    return err;
  }
};
```

To run the example, enter the following at the command prompt\. The domain is added to Amazon SES to be verified\.

```
node ses_verifyemailidentity.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_verifyemailidentity.js)\.

## Verifying a Domain identity<a name="ses-examples-verifying-domains"></a>

In this example, use a Node\.js module to verify email domains to use with Amazon SES\.

Create a `libs` directory, and create a Node\.js module with the file name `sesClient.js`\. Copy and paste the code below into it, which creates the Amazon SES client object\. Replace *REGION* with your AWS Region\.

```
import { SESClient } from "@aws-sdk/client-ses";
// Set the AWS Region.
const REGION = "us-east-1";
// Create SES service object.
const sesClient = new SESClient({ region: REGION });
export { sesClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/libs/sesClient.js)\.

Create a Node\.js module with the file name `ses_verifydomainidentity.js`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the `Domain` parameter for the `VerifyDomainIdentityCommand` method of the `SES` client class\. To call the `VerifyDomainIdentityCommand` method, invoke an Amazon SES client service object, passing the parameters object\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *AMI\_ID* with the ID of the Amazon Machine Image \(AMI\) to run, and *KEY\_PAIR\_NAME* of the key pair to assign to the AMI ID\.

```
import { VerifyDomainIdentityCommand } from "@aws-sdk/client-ses";
import { getUniqueName, postfix } from "../../libs/utils/util-string.js";
import { sesClient } from "./libs/sesClient.js";

/**
 * You must have access to the domain's DNS settings to complete the
 * domain verification process.
 */
const DOMAIN_NAME = postfix(getUniqueName("Domain"), ".example.com");

const createVerifyDomainIdentityCommand = () => {
  return new VerifyDomainIdentityCommand({ Domain: DOMAIN_NAME });
};

const run = async () => {
  const VerifyDomainIdentityCommand = createVerifyDomainIdentityCommand();

  try {
    return await sesClient.send(VerifyDomainIdentityCommand);
  } catch (err) {
    console.log("Failed to verify domain.", err);
    return err;
  }
};
```

To run the example, enter the following at the command prompt\. The domain is added to Amazon SES to be verified\.

```
node ses_verifydomainidentity.js  
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_verifydomainidentity.js)\.

## Deleting identities<a name="ses-examples-deleting-identities"></a>

In this example, use a Node\.js module to delete email addresses or domains used with Amazon SES\.

Create a `libs` directory, and create a Node\.js module with the file name `sesClient.js`\. Copy and paste the code below into it, which creates the Amazon SES client object\. Replace *REGION* with your AWS Region\.

```
import { SESClient } from "@aws-sdk/client-ses";
// Set the AWS Region.
const REGION = "us-east-1";
// Create SES service object.
const sesClient = new SESClient({ region: REGION });
export { sesClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/libs/sesClient.js)\.

Create a Node\.js module with the file name `ses_deleteidentity.js`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the `Identity` parameter for the `DeleteIdentityCommand` method of the `SES` client class\. To call the `DeleteIdentityCommand` method, create a `request` for invoking an Amazon SES client service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *IDENTITY\_TYPE* with the identity type to be deleted, and *IDENTITY\_NAME* with the name of the identity to be deleted\.

```
import { DeleteIdentityCommand } from "@aws-sdk/client-ses";
import { sesClient } from "./libs/sesClient.js";

const IDENTITY_EMAIL = "fake@example.com";

const createDeleteIdentityCommand = (identityName) => {
  return new DeleteIdentityCommand({
    Identity: identityName,
  });
};

const run = async () => {
  const deleteIdentityCommand = createDeleteIdentityCommand(IDENTITY_EMAIL);

  try {
    return await sesClient.send(deleteIdentityCommand);
  } catch (err) {
    console.log("Failed to delete identity.", err);
    return err;
  }
};
```

To run the example, enter the following at the command prompt\.

```
node ses_deleteidentity.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_deleteidentity.js)\.