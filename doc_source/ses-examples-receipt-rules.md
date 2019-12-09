# Using Receipt Rules in Amazon SES<a name="ses-examples-receipt-rules"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ Create and delete receipt rules\.
+ Organize receipt rules into receipt rule sets\.

Receipt rules in Amazon SES specify what to do with email received for email addresses or domains you own\. A *receipt rule* contains a condition and an ordered list of actions\. If the recipient of an incoming email matches a recipient specified in the conditions for the receipt rule, Amazon SES performs the actions that the receipt rule specifies\. 

To use Amazon SES as your email receiver, you must have at least one active *receipt rule set*\. A receipt rule set is an ordered collection of receipt rules that specify what Amazon SES should do with mail it receives across your verified domains\. For more information, see [Creating Receipt Rules for Amazon SES Email Receiving](Amazon Simple Email Service Developer Guidereceiving-email-receipt-rules.html) and [Creating a Receipt Rule Set for Amazon SES Email Receiving](Amazon Simple Email Service Developer Guidereceiving-email-receipt-rule-set.html) in the Amazon Simple Email Service Developer Guide\.

## The Scenario<a name="ses-examples-receipt-rules-scenario"></a>

In this example, a series of Node\.js modules are used to send email in a variety of ways\. The Node\.js modules use the SDK for JavaScript to create and use email templates using these methods of the `AWS.SES` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#createReceiptRule-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#createReceiptRule-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#deleteReceiptRule-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#deleteReceiptRule-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#createReceiptRuleSet-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#createReceiptRuleSet-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#deleteReceiptRuleSet-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#deleteReceiptRuleSet-property)

## Prerequisite Tasks<a name="ses-examples-receipt-rules-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Creating an Amazon S3 Receipt Rule<a name="ses-examples-creatingreceipt-rules"></a>

Each receipt rule for Amazon SES contains an ordered list of actions\. This example creates a receipt rule with an Amazon S3 action, which delivers the mail message to an Amazon S3 bucket\. For details on receipt rule actions, see [Action Options ](Amazon Simple Email Service Developer Guidereceiving-email-action.html) in the Amazon Simple Email Service Developer Guide\.

For Amazon SES to write email to an Amazon S3 bucket, create a bucket policy that gives `PutObject` permission to Amazon SES\. For information about creating this bucket policy, see [Give Amazon SES Permission to Write to Your Amazon S3 Bucket ](Amazon Simple Email Service Developer Guidereceiving-email-permissions.html#receiving-email-permissions-s3.html) in the Amazon Simple Email Service Developer Guide\.

In this example, use a Node\.js module to create a receipt rule in Amazon SES to save received messages in an Amazon S3 bucket\. Create a Node\.js module with the file name `ses_createreceiptrule.js`\. Configure the SDK as previously shown\.

Create a parameters object to pass the values needed to create for the receipt rule set\. To call the `createReceiptRuleSet` method, create a promise for invoking an Amazon SES service object, passing the parameters\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create createReceiptRule params 
var params = {
 Rule: {
  Actions: [
     {
    S3Action: {
     BucketName: "S3_BUCKET_NAME",
     ObjectKeyPrefix: "email"
    }
   }
  ],
    Recipients: [
      'DOMAIN | EMAIL_ADDRESS',
      /* more items */
    ],
  Enabled: true | false,
  Name: "RULE_NAME",
  ScanEnabled: true | false,
  TlsPolicy: "Optional"
 },
 RuleSetName: "RULE_SET_NAME"
};

// Create the promise and SES service object
var newRulePromise = new AWS.SES({apiVersion: '2010-12-01'}).createReceiptRule(params).promise();

// Handle promise's fulfilled/rejected states
newRulePromise.then(
  function(data) {
    console.log("Rule created");
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\. Amazon SES creates the receipt rule\.

```
node ses_createreceiptrule.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ses/ses_createreceiptrule.js)\.

## Deleting a Receipt Rule<a name="ses-examples-deletingreceipt-rules"></a>

In this example, use a Node\.js module to send email with Amazon SES\. Create a Node\.js module with the file name `ses_deletereceiptrule.js`\. Configure the SDK as previously shown\.

Create a parameters object to pass the name for the receipt rule to delete\. To call the `deleteReceiptRule` method, create a promise for invoking an Amazon SES service object, passing the parameters\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create deleteReceiptRule params
var params = {
  RuleName: 'RULE_NAME', /* required */
  RuleSetName: 'RULE_SET_NAME' /* required */
};

// Create the promise and SES service object
var newRulePromise = new AWS.SES({apiVersion: '2010-12-01'}).deleteReceiptRule(params).promise();

// Handle promise's fulfilled/rejected states
newRulePromise.then(
  function(data) {
    console.log("Receipt Rule Deleted");
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\. Amazon SES creates the receipt rule set list\.

```
node ses_deletereceiptrule.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ses/ses_deletereceiptrule.js)\.

## Creating a Receipt Rule Set<a name="ses-examples-creatingreceiptrulesets"></a>

In this example, use a Node\.js module to send email with Amazon SES\. Create a Node\.js module with the file name `ses_createreceiptruleset.js`\. Configure the SDK as previously shown\.

Create a parameters object to pass the name for the new receipt rule set\. To call the `createReceiptRuleSet` method, create a promise for invoking an Amazon SES service object, passing the parameters\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the promise and SES service object
var newRulePromise = new AWS.SES({apiVersion: '2010-12-01'}).createReceiptRuleSet({RuleSetName: "NAME"}).promise();

// Handle promise's fulfilled/rejected states
newRulePromise.then(
  function(data) {
    console.log(data);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\. Amazon SES creates the receipt rule set list\.

```
node ses_createreceiptruleset.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ses/ses_createreceiptruleset.js)\.

## Deleting a Receipt Rule Set<a name="ses-examples-deletingreceiptrulesets"></a>

In this example, use a Node\.js module to send email with Amazon SES\. Create a Node\.js module with the file name `ses_deletereceiptruleset.js`\. Configure the SDK as previously shown\.

Create an object to pass the name for the receipt rule set to delete\. To call the `deleeReceiptRuleSet` method, create a promise for invoking an Amazon SES service object, passing the parameters\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the promise and SES service object
var newRulePromise = new AWS.SES({apiVersion: '2010-12-01'}).deleteReceiptRuleSet({RuleSetName: "NAME"}).promise();

// Handle promise's fulfilled/rejected states
newRulePromise.then(
  function(data) {
    console.log(data);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\. Amazon SES creates the receipt rule set list\.

```
node ses_deletereceiptruleset.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ses/ses_deletereceiptruleset.js)\.