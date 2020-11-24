--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Using receipt rules in Amazon SES<a name="ses-examples-receipt-rules"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create and delete receipt rules\.
+ How to organize receipt rules into receipt rule sets\.

Receipt rules in Amazon SES specify what to do with email received for email addresses or domains you own\. A *receipt rule* contains a condition and an ordered list of actions\. If the recipient of an incoming email matches a recipient specified in the conditions for the receipt rule, Amazon SES performs the actions that the receipt rule specifies\. 

To use Amazon SES as your email receiver, you must have at least one active *receipt rule set*\. A receipt rule set is an ordered collection of receipt rules that specify what Amazon SES should do with mail it receives across your verified domains\. For more information, see [Creating receipt rules for Amazon SES email receiving](Amazon Simple Email Service Developer Guidereceiving-email-receipt-rules.html) and [Creating a receipt rule set for Amazon SES email receiving](Amazon Simple Email Service Developer Guidereceiving-email-receipt-rule-set.html) in the Amazon Simple Email Service Developer Guide\.

## The scenario<a name="ses-examples-receipt-rules-scenario"></a>

In this example, a series of Node\.js modules are used to send email in a variety of ways\. The Node\.js modules use the SDK for JavaScript to create and use email templates using these methods of the `SES` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#createReceiptRule-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#createReceiptRule-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#deleteReceiptRule-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#deleteReceiptRule-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#createReceiptRuleSet-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#createReceiptRuleSet-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#deleteReceiptRuleSet-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#deleteReceiptRuleSet-property)

## Prerequisite tasks<a name="ses-examples-receipt-rules-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/ses/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Creating an Amazon S3 receipt rule<a name="ses-examples-creatingreceipt-rules"></a>

Each receipt rule for Amazon SES contains an ordered list of actions\. This example creates a receipt rule with an Amazon S3 action, which delivers the mail message to an Amazon S3 bucket\. For details on receipt rule actions, see [Action options ](Amazon Simple Email Service Developer Guidereceiving-email-action.html) in the Amazon Simple Email Service Developer Guide\.

For Amazon SES to write email to an Amazon S3 bucket, create a bucket policy that gives `PutObject` permission to Amazon SES\. For information about creating this bucket policy, see [Give Amazon SES permission to write to your Amazon S3 bucket ](Amazon Simple Email Service Developer Guidereceiving-email-permissions.html#receiving-email-permissions-s3.html) in the Amazon Simple Email Service Developer Guide\.

In this example, use a Node\.js module to create a receipt rule in Amazon SES to save received messages in an Amazon S3 bucket\. Create a Node\.js module with the file name `ses_createreceiptrule.ts`\. Configure the SDK as previously shown\.

Create a parameters object to pass the values needed to create for the receipt rule set\. To call the `CreateReceiptRuleSetCommand` method, invoke an Amazon SES service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *S3\_BUCKET\_NAME* with the name of the Amazon S3 bucket, *EMAIL\_ADDRESS \| DOMAIN* with the email, or domain, *RULE\_NAME* with the name of the rule, and *RULE\_SET\_NAME* with the name of the ruleset\.

```
// Import required AWS SDK clients and commands for Node.js
const { SESClient, CreateReceiptRuleCommand } = require("@aws-sdk/client-ses");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1" // REGION

// Set the parameters
const params = {
  Rule: {
    Actions: [
      {
        S3Action: {
          BucketName: "BUCKET_NAME", // S3_BUCKET_NAME
          ObjectKeyPrefix: "email",
        },
      },
    ],
    Recipients: [
      "EMAIL_ADDRESS", // The email addresses, or domain
      /* more items */
    ],
    Enabled: true | false,
    Name: "RULE_NAME", // RULE_NAME
    ScanEnabled: true | false,
    TlsPolicy: "Optional",
  },
  RuleSetName: "RULE_SET_NAME", // RULE_SET_NAME
};

// Create SES service object
const ses = new SESClient(REGION);

const run = async () => {
  try {
    const data = await ses.send(new CreateReceiptRuleCommand(params));
    console.log("Rule created; requestId:", data.$metadata.requestId);
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\. Amazon SES creates the receipt rule\.

```
ts-node ses_createreceiptrule.ts // If you prefer JavaScript, enter 'node ses_createreceiptrule.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_createreceiptrule.ts)\.

## Deleting a receipt rule<a name="ses-examples-deletingreceipt-rules"></a>

In this example, use a Node\.js module to send email with Amazon SES\. Create a Node\.js module with the file name `ses_deletereceiptrule.ts`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create a parameters object to pass the name for the receipt rule to delete\. To call the `DeleteReceiptRuleCommand` method, invoke an Amazon SES service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *RULE\_NAME* with the name of the rule, and *RULE\_SET\_NAME* with the rule set name\.

```
// Import required AWS SDK clients and commands for Node.js
const { SESClient, DeleteReceiptRuleCommand } = require("@aws-sdk/client-ses");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the deleteReceiptRule params
var params = {
  RuleName: "RULE_NAME", // RULE_NAME
  RuleSetName: "RULE_SET_NAME", // RULE_SET_NAME
};

// Create SES service object
const ses = new SESClient(REGION);

const run = async () => {
  try {
    const data = await ses.send(new DeleteReceiptRuleCommand(params));
    console.log("Receipt Rule Deleted");
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\. Amazon SES creates the receipt rule set list\.

```
ts-node ses_deletereceiptrule.ts // If you prefer JavaScript, enter 'node ses_deletereceiptrule.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_deletereceiptrule.ts)\.

## Creating a receipt rule set<a name="ses-examples-creatingreceiptrulesets"></a>

In this example, use a Node\.js module to send email with Amazon SES\. Create a Node\.js module with the file name `ses_createreceiptruleset.ts`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create a parameters object to pass the name for the new receipt rule set\. To call the `CreateReceiptRuleSetCommand` method, invoke an Amazon SES client service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *RULE\_SET\_NAME* with the rule set name\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  SESClient,
  CreateReceiptRuleSetCommand
} = require("@aws-sdk/client-ses");

// Set the AWS Region
const REGION = "region"; //e.g. "us-east-1" // REGION

// Set the parameters
const params = { RuleSetName: "RULE_SET_NAME" }; //RULE_SET_NAME

// Create SES service object
const ses = new SESClient(REGION);

const run = async () => {
  try {
    const data = await ses.send(new CreateReceiptRuleSetCommand(params));
    console.log(
      "Success, receipt rule created; requestId",
      data.$metadata.requestId
    );
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\. Amazon SES creates the receipt rule set list\.

```
ts-node ses_createreceiptruleset.ts // If you prefer JavaScript, enter 'node ses_createreceiptruleset.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_createreceiptruleset.ts)\.

## Deleting a receipt rule set<a name="ses-examples-deletingreceiptrulesets"></a>

In this example, use a Node\.js module to send email with Amazon SES\. Create a Node\.js module with the file name `ses_deletereceiptruleset.ts`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the name for the receipt rule set to delete\. To call the `DeleteReceiptRuleSetCommand` method, invoke an Amazon SES client service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *RULE\_SET\_NAME* with the rule set name\.

```
// Import required AWS SDK clients and commands for Node.js
const { SESClient, DeleteReceiptRuleSetCommand } = require("@aws-sdk/client-ses");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { RuleSetName: "RULE_SET_NAME" }; //RULE_SET_NAME

// Create SES service object
const ses = new SESClient(REGION);

const run = async () => {
  try {
    const data = await ses.send(new DeleteReceiptRuleSetCommand(params));
    console.log("Success, rule set deleted", data);
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\. Amazon SES creates the receipt rule set list\.

```
ts-node ses_deletereceiptruleset.ts // If you prefer JavaScript, enter 'node ses_deletereceiptruleset.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_deletereceiptruleset.ts)\.