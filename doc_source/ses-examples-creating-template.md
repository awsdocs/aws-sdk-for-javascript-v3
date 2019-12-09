# Working with Email Templates in Amazon SES<a name="ses-examples-creating-template"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ Get a list of all of your email templates\.
+ Retrieve and update email templates\.
+ Create and delete email templates\.

Amazon SES lets you send personalized email messages using email templates\. For details on how to create and use email templates in Amazon Simple Email Service, see [Sending Personalized Email Using the Amazon SES API](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/send-personalized-email-api.html) in the Amazon Simple Email Service Developer Guide\.

## The Scenario<a name="ses-examples-creating-template-scenario"></a>

In this example, you use a series of Node\.js modules to work with email templates\. The Node\.js modules use the SDK for JavaScript to create and use email templates using these methods of the `AWS.SES` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#listTemplates-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#listTemplates-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#createTemplate-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#createTemplate-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#getTemplate-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#getTemplate-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#deleteTemplate-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#deleteTemplate-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#updateTemplate-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#updateTemplate-property)

## Prerequisite Tasks<a name="ses-examples-creating-template-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about creating a credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Listing Your Email Templates<a name="ses-examples-listing-templates"></a>

In this example, use a Node\.js module to create an email template to use with Amazon SES\. Create a Node\.js module with the file name `ses_listtemplates.js`\. Configure the SDK as previously shown\.

Create an object to pass the parameters for the `listTemplates` method of the `AWS.SES` client class\. To call the `listTemplates` method, create a promise for invoking an Amazon SES service object, passing the parameters\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the promise and SES service object
var templatePromise = new AWS.SES({apiVersion: '2010-12-01'}).listTemplates({MaxItems: ITEMS_COUNT}).promise();

// Handle promise's fulfilled/rejected states
templatePromise.then(
  function(data) {
    console.log(data);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\. Amazon SES returns the list of templates\.

```
node ses_listtemplates.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ses/ses_listtemplates.js)\.

## Getting an Email Template<a name="ses-examples-get-template"></a>

In this example, use a Node\.js module to get an email template to use with Amazon SES\. Create a Node\.js module with the file name `ses_gettemplate.js`\. Configure the SDK as previously shown\.

Create an object to pass the `TemplateName` parameter for the `getTemplate` method of the `AWS.SES` client class\. To call the `getTemplate` method, create a promise for invoking an Amazon SES service object, passing the parameters\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the promise and SES service object
var templatePromise = new AWS.SES({apiVersion: '2010-12-01'}).getTemplate({TemplateName: 'TEMPLATE_NAME'}).promise();

// Handle promise's fulfilled/rejected states
templatePromise.then(
  function(data) {
    console.log(data.SubjectPart);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\. Amazon SES returns the template details\.

```
node ses_gettemplate.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ses/ses_gettemplate.js)\.

## Creating an Email Template<a name="ses-examples-create-template"></a>

In this example, use a Node\.js module to create an email template to use with Amazon SES\. Create a Node\.js module with the file name `ses_createtemplate.js`\. Configure the SDK as previously shown\.

Create an object to pass the parameters for the `createTemplate` method of the `AWS.SES` client class, including `TemplateName`, `HtmlPart`, `SubjectPart`, and `TextPart`\. To call the `createTemplate` method, create a promise for invoking an Amazon SES service object, passing the parameters\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create createTemplate params
var params = {
  Template: { 
    TemplateName: 'TEMPLATE_NAME', /* required */
    HtmlPart: 'HTML_CONTENT',
    SubjectPart: 'SUBJECT_LINE',
    TextPart: 'TEXT_CONTENT'
  }
};

// Create the promise and SES service object
var templatePromise = new AWS.SES({apiVersion: '2010-12-01'}).createTemplate(params).promise();

// Handle promise's fulfilled/rejected states
templatePromise.then(
  function(data) {
    console.log(data);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\. The template is added to Amazon SES\.

```
node ses_createtemplate.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ses/ses_createtemplate.js)\.

## Updating an Email Template<a name="ses-examples-update-template"></a>

In this example, use a Node\.js module to create an email template to use with Amazon SES\. Create a Node\.js module with the file name `ses_updatetemplate.js`\. Configure the SDK as previously shown\.

Create an object to pass the `Template` parameter values you want to update in the template, with the required `TemplateName` parameter passed to the `updateTemplate` method of the `AWS.SES` client class\. To call the `updateTemplate` method, create a promise for invoking an Amazon SES service object, passing the parameters\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create updateTemplate parameters 
var params = {
  Template: { 
    TemplateName: 'TEMPLATE_NAME', /* required */
    HtmlPart: 'HTML_CONTENT',
    SubjectPart: 'SUBJECT_LINE',
    TextPart: 'TEXT_CONTENT'
  }
};

// Create the promise and SES service object
var templatePromise = new AWS.SES({apiVersion: '2010-12-01'}).updateTemplate(params).promise();

// Handle promise's fulfilled/rejected states
templatePromise.then(
  function(data) {
    console.log("Template Updated");
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\. Amazon SES returns the template details\.

```
node ses_updatetemplate.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ses/ses_updatetemplate.js)\.

## Deleting an Email Template<a name="ses-examples-delete-template"></a>

In this example, use a Node\.js module to create an email template to use with Amazon SES\. Create a Node\.js module with the file name `ses_deletetemplate.js`\. Configure the SDK as previously shown\.

Create an object to pass the required`TemplateName` parameter to the `deleteTemplate` method of the `AWS.SES` client class\. To call the `deleteTemplate` method, create a promise for invoking an Amazon SES service object, passing the parameters\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the promise and SES service object
var templatePromise = new AWS.SES({apiVersion: '2010-12-01'}).deleteTemplate({TemplateName: 'TEMPLATE_NAME'}).promise();

// Handle promise's fulfilled/rejected states
templatePromise.then(
  function(data) {
    console.log("Template Deleted");
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\. Amazon SES returns the template details\.

```
node ses_deletetemplate.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ses/ses_deletetemplate.js)\.