# Managing Amazon SES Identities<a name="ses-examples-managing-identities"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to verify email addresses and domains used with Amazon SES\.
+ How to assign IAM policy to your Amazon SES identities\.
+ How to list all Amazon SES identities for your AWS account\.
+ How to delete identities used with Amazon SES\.

An Amazon SES *identity* is an email address or domain that Amazon SES uses to send email\. Amazon SES requires you to verify your email identities, confirming that you own them and preventing others from using them\.

For details on how to verify email addresses and domains in Amazon SES, see [Verifying Email Addresses and Domains in Amazon SES](Amazon Simple Email Service Developer Guideverify-addresses-and-domains.html) in the Amazon Simple Email Service Developer Guide\. For information about sending authorization in Amazon SES, see [Overview of Amazon SES Sending Authorization](Amazon Simple Email Service Developer Guidesending-authorization-overview.html)\.

## The Scenario<a name="ses-examples-verifying-identities-scenario"></a>

In this example, you use a series of Node\.js modules to verify and manage Amazon SES identities\. The Node\.js modules use the SDK for JavaScript to verify email addresses and domains, using these methods of the `AWS.SES` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#listIdentities-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#listIdentities-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#deleteIdentity-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#deleteIdentity-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#verifyEmailIdentity-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#verifyEmailIdentity-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#verifyDomainIdentity-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#verifyDomainIdentity-property)

## Prerequisite Tasks<a name="ses-examples-verifying-identities-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Configuring the SDK<a name="ses-examples-verifying-identities-configure-sdk"></a>

Configure the SDK for JavaScript by creating a global configuration object then setting the Region for your code\. In this example, the Region is set to `us-west-2`\.

```
// Load the SDK for JavaScript
var AWS = require('aws-sdk');

// Set the Region 
AWS.config.update({region: 'us-west-2'});
```

## Listing Your Identities<a name="ses-examples-listing-identities"></a>

In this example, use a Node\.js module to list email addresses and domains to use with Amazon SES\. Create a Node\.js module with the file name `ses_listidentities.js`\. Configure the SDK as previously shown\.

Create an object to pass the `IdentityType` and other parameters for the `listIdentities` method of the `AWS.SES` client class\. To call the `listIdentities` method, create a promise for invoking an Amazon SES service object, passing the parameters object\. 

Then handle the `response` in the promise callback\. The `data` returned by the promise contains an array of domain identities as specified by the `IdentityType` parameter\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set region 
AWS.config.update({region: 'REGION'});

// Create listIdentities params 
var params = {
 IdentityType: "Domain",
 MaxItems: 10
};

// Create the promise and SES service object
var listIDsPromise = new AWS.SES({apiVersion: '2010-12-01'}).listIdentities(params).promise();

// Handle promise's fulfilled/rejected states
listIDsPromise.then(
  function(data) {
    console.log(data.Identities);
  }).catch(
  function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\.

```
node ses_listidentities.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ses/ses_listidentities.js)\.

## Verifying an Email Address Identity<a name="ses-examples-verifying-email"></a>

In this example, use a Node\.js module to verify email senders to use with Amazon SES\. Create a Node\.js module with the file name `ses_verifyemailidentity.js`\. Configure the SDK as previously shown\. To access Amazon SES, create an `AWS.SES` service object\.

Create an object to pass the `EmailAddress` parameter for the `verifyEmailIdentity` method of the `AWS.SES` client class\. To call the `verifyEmailIdentity` method, create a promise for invoking an Amazon SES service object, passing the parameters\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set region 
AWS.config.update({region: 'REGION'});

// Create promise and SES service object
var verifyEmailPromise = new AWS.SES({apiVersion: '2010-12-01'}).verifyEmailIdentity({EmailAddress: "ADDRESS@DOMAIN.EXT"}).promise();

// Handle promise's fulfilled/rejected states
verifyEmailPromise.then(
  function(data) {
    console.log("Email verification initiated");
   }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\. The domain is added to Amazon SES to be verified\.

```
node ses_verifyemailidentity.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ses/ses_verifyemailidentity.js)\.

## Verifying a Domain Identity<a name="ses-examples-verifying-domains"></a>

In this example, use a Node\.js module to verify email domains to use with Amazon SES\. Create a Node\.js module with the file name `ses_verifydomainidentity.js`\. Configure the SDK as previously shown\.

Create an object to pass the `Domain` parameter for the `verifyDomainIdentity` method of the `AWS.SES` client class\. To call the `verifyDomainIdentity` method, create a promise for invoking an Amazon SES service object, passing the parameters object\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set region 
AWS.config.update({region: 'REGION'});

// Create the promise and SES service object
var verifyDomainPromise = new AWS.SES({apiVersion: '2010-12-01'}).verifyDomainIdentity({Domain: "DOMAIN_NAME"}).promise();

// Handle promise's fulfilled/rejected states
verifyDomainPromise.then(
  function(data) {
    console.log("Verification Token: " + data.VerificationToken);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\. The domain is added to Amazon SES to be verified\.

```
node ses_verifydomainidentity.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ses/ses_verifydomainidentity.js)\.

## Deleting Identities<a name="ses-examples-deleting-identities"></a>

In this example, use a Node\.js module to delete email addresses or domains used with Amazon SES\. Create a Node\.js module with the file name `ses_deleteidentity.js`\. Configure the SDK as previously shown\.

Create an object to pass the `Identity` parameter for the `deleteIdentity` method of the `AWS.SES` client class\. To call the `deleteIdentity` method, create a `request` for invoking an Amazon SES service object, passing the parameters\. Then handle the `response` in the promise callback\.\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set region 
AWS.config.update({region: 'REGION'});

// Create the promise and SES service object
var deletePromise = new AWS.SES({apiVersion: '2010-12-01'}).deleteIdentity({Identity: "DOMAIN_NAME"}).promise();

// Handle promise's fulfilled/rejected states
deletePromise.then(
  function(data) {
    console.log("Identity Deleted");
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\.

```
node ses_deleteidentity.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ses/ses_deleteidentity.js)\.