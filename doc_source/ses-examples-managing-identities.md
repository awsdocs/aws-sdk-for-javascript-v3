--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Managing Amazon SES identities<a name="ses-examples-managing-identities"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to verify email addresses and domains used with Amazon SES\.
+ How to assign an AWS Identity and Access Management \(IAM\) policy to your Amazon SES identities\.
+ How to list all Amazon SES identities for your AWS account\.
+ How to delete identities used with Amazon SES\.

An Amazon SES *identity* is an email address or domain that Amazon SES uses to send email\. Amazon SES requires you to verify your email identities, confirming that you own them and preventing others from using them\.

For details on how to verify email addresses and domains in Amazon SES, see [Verifying email addresses and domains in Amazon SES](Amazon Simple Email Service Developer Guideverify-addresses-and-domains.html) in the Amazon Simple Email Service Developer Guide\. For information about sending authorization in Amazon SES, see [Overview of Amazon SES sending authorization](Amazon Simple Email Service Developer Guidesending-authorization-overview.html)\.

## The scenario<a name="ses-examples-verifying-identities-scenario"></a>

In this example, you use a series of Node\.js modules to verify and manage Amazon SES identities\. The Node\.js modules use the SDK for JavaScript to verify email addresses and domains, using these methods of the `SES` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#listIdentities-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#listIdentities-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#deleteIdentity-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#deleteIdentity-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#verifyEmailIdentity-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#verifyEmailIdentity-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#verifyDomainIdentity-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#verifyDomainIdentity-property)

## Prerequisite tasks<a name="ses-examples-verifying-identities-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up a project environment to run Node TypeScript examples\. For more information, see[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/ses/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Listing your identities<a name="ses-examples-listing-identities"></a>

In this example, use a Node\.js module to list email addresses and domains to use with Amazon SES\. Create a Node\.js module with the file name `ses_listidentities.ts`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the `IdentityType` and other parameters for the `ListIdentitiesCommand` method of the `SES` client class\. To call the `ListIdentitiesCommand` method, invoke an Amazon SES service object, passing the parameters object\. 

 The `data` returned contains an array of domain identities as specified by the `IdentityType` parameter\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *IDENTITY\_TYPE* with the identity type, which can be "EmailAddress" or "Domain"\.

```
// Import required AWS SDK clients and commands for Node.js
const { SESClient, ListIdentitiesCommand } = require("@aws-sdk/client-ses");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
var params = {
  IdentityType: "IDENTITY_TYPE", // IDENTITY_TYPE: "EmailAddress' or 'Domain'
  MaxItems: 10,
};

// Create SES service object
const ses = new SESClient(REGION);

const run = async () => {
  try {
    const data = await ses.send(new ListIdentitiesCommand(params));
    console.log("Success.", data);
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ses_listidentities.ts // If you prefer JavaScript, enter 'node ses_listidentities.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_listidentities.ts)\.

## Verifying an email address identity<a name="ses-examples-verifying-email"></a>

In this example, use a Node\.js module to verify email senders to use with Amazon SES\. Create a Node\.js module with the file name `ses_verifyemailidentity.ts`\. Configure the SDK as previously shown, including downloading the required clients and packages\. To access Amazon SES, create an `SES` client service object\.

Create an object to pass the `EmailAddress` parameter for the `VerifyEmailIdentityCommand` method of the `SES` client class\. To call the `VerifyEmailIdentityCommand` method, invoke an Amazon SES client service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *ADDRESS@DOMAIN\.EXT* with the email address, such as name@example\.com\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  SESClient,
  VerifyEmailIdentityCommand
} = require("@aws-sdk/client-ses");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { EmailAddress: "ADDRESS@DOMAIN.EXT" }; //ADDRESS@DOMAIN.EXT; e.g., name@example.com

// Create SES service object
const ses = new SESClient(REGION);

const run = async () => {
  try {
    const data = await ses.send(new VerifyEmailIdentityCommand(params));
    console.log("Email verification initiated");
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\. The domain is added to Amazon SES to be verified\.

```
ts-node ses_verifyemailidentity.ts // If you prefer JavaScript, enter 'node ses_verifyemailidentity.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/ses_verifyemailidentity.ts)\.

## Verifying a Domain identity<a name="ses-examples-verifying-domains"></a>

In this example, use a Node\.js module to verify email domains to use with Amazon SES\. Create a Node\.js module with the file name `ses_verifydomainidentity.ts`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the `Domain` parameter for the `VerifyDomainIdentityCommand` method of the `SES` client class\. To call the `VerifyDomainIdentityCommand` method, invoke an Amazon SES client service object, passing the parameters object\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *AMI\_ID* with the ID of the Amazon Machine Image \(AMI\) to run, and *KEY\_PAIR\_NAME* of the key pair to assign to the AMI ID\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  SESClient,
  VerifyDomainIdentityCommand,
} = require("@aws-sdk/client-ses");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { Domain: "DOMAIN_NAME" }; //DOMAIN_NAME

// Create SES service object
const ses = new SESClient(REGION);

const run = async () => {
  try {
    const data = await ses.send(new VerifyDomainIdentityCommand(params));
    console.log("Verification Token: " + data.VerificationToken);
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\. The domain is added to Amazon SES to be verified\.

```
ts-node ses_verifydomainidentity.ts  // If you prefer JavaScript, enter 'node ses_verifydomainidentity.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/ses_verifydomainidentity.ts)\.

## Deleting identities<a name="ses-examples-deleting-identities"></a>

In this example, use a Node\.js module to delete email addresses or domains used with Amazon SES\. Create a Node\.js module with the file name `ses_deleteidentity.ts`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the `Identity` parameter for the `DeleteIdentityCommand` method of the `SES` client class\. To call the `DeleteIdentityCommand` method, create a `request` for invoking an Amazon SES client service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *IDENTITY\_TYPE* with the identity type to be deleted, and *IDENTITY\_NAME* with the name of the identity to be deleted\.

```
// Import required AWS SDK clients and commands for Node.js
const { SESClient, DeleteIdentityCommand } = require("@aws-sdk/client-ses");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  IdentityType: "IDENTITY_TYPE", // IDENTITY_TYPE - i.e., 'EmailAddress' or 'Domain'
  Identity: "IDENTITY_NAME",
}; // IDENTITY_NAME

// Create SES service object
const ses = new SESClient(REGION);

const run = async () => {
  try {
    const data = await ses.send(new DeleteIdentityCommand(params));
    console.log("Identity Deleted");
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ses_deleteidentity.ts // If you prefer JavaScript, enter 'node ses_deleteidentity.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_deleteidentity.ts)\.