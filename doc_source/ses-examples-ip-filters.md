# Using IP Address Filters for Email Receipt in Amazon SES<a name="ses-examples-ip-filters"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ Create IP address filters to accept or reject mail that originates from an IP address or range of IP addresses\.
+ List your current IP address filters\.
+ Delete an IP address filter\.

In Amazon SES, a *filter* is a data structure that consists of a name, an IP address range, and whether to allow or block mail from it\. IP addresses you want to block or allow are specified as a single IP address or a range of IP addresses in Classless Inter\-Domain Routing \(CIDR\) notation\. For details on how Amazon SES receives email, see [Amazon SES Email\-Receiving Concepts](Amazon Simple Email Service Developer Guidereceiving-email-concepts.html) in the Amazon Simple Email Service Developer Guide\.

## The Scenario<a name="ses-examples-receiving-email-scenario"></a>

In this example, a series of Node\.js modules are used to send email in a variety of ways\. The Node\.js modules use the SDK for JavaScript to create and use email templates using these methods of the `AWS.SES` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#createReceiptFilter-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#createReceiptFilter-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#listReceiptFilters-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#listReceiptFilters-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#deleteReceiptFilter-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/SES.html#deleteReceiptFilter-property)

## Prerequisite Tasks<a name="ses-examples-ip-filters-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Install Node\.js\. For more information about installing Node\.js, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Configuring the SDK<a name="ses-examples-ip-filters-configure-sdk"></a>

Configure the SDK for JavaScript by creating a global configuration object then setting the Region for your code\. In this example, the Region is set to `us-west-2`\.

```
// Load the SDK for JavaScript
var AWS = require('aws-sdk');
// Set the Region 
AWS.config.update({region: 'us-west-2'});
```

## Creating an IP Address Filter<a name="ses-examples-ip-filters-creating"></a>

In this example, use a Node\.js module to send email with Amazon SES\. Create a Node\.js module with the file name `ses_createreceiptfilter.js`\. Configure the SDK as previously shown\.

Create an object to pass the parameter values that define the IP filter, including the filter name, an IP address or range of addresses to filter, and whether to allow or block email traffic from the filtered addresses\. To call the `createReceiptFilter` method, create a promise for invoking an Amazon SES service object, passing the parameters\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create createReceiptFilter params
var params = {
 Filter: {
  IpFilter: {
   Cidr: "IP_ADDRESS_OR_RANGE", 
   Policy: "Allow" | "Block"
  },
  Name: "NAME"
 }
};


// Create the promise and SES service object
var sendPromise = new AWS.SES({apiVersion: '2010-12-01'}).createReceiptFilter(params).promise();

// Handle promise's fulfilled/rejected states
sendPromise.then(
  function(data) {
    console.log(data);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\. The filter is created in Amazon SES\.

```
node ses_createreceiptfilter.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ses/ses_createreceiptfilter.js)\.

## Listing Your IP Address Filters<a name="ses-examples-ip-filters-listing"></a>

In this example, use a Node\.js module to send email with Amazon SES\. Create a Node\.js module with the file name `ses_listreceiptfilters.js`\. Configure the SDK as previously shown\.

Create an empty parameters object\. To call the `listReceiptFilters` method, create a promise for invoking an Amazon SES service object, passing the parameters\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the promise and SES service object
var sendPromise = new AWS.SES({apiVersion: '2010-12-01'}).listReceiptFilters({}).promise();

// Handle promise's fulfilled/rejected states
sendPromise.then(
  function(data) {
    console.log(data.Filters);
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\. Amazon SES returns the filter list\.

```
node ses_listreceiptfilters.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ses/ses_listreceiptfilters.js)\.

## Deleting an IP Address Filter<a name="ses-examples-ip-filters-deleting"></a>

In this example, use a Node\.js module to send email with Amazon SES\. Create a Node\.js module with the file name `ses_deletereceiptfilter.js`\. Configure the SDK as previously shown\.

Create an object to pass the name of the IP filter to delete\. To call the `deleteReceiptFilter` method, create a promise for invoking an Amazon SES service object, passing the parameters\. Then handle the `response` in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the promise and SES service object
var sendPromise = new AWS.SES({apiVersion: '2010-12-01'}).deleteReceiptFilter({FilterName: "NAME"}).promise();

// Handle promise's fulfilled/rejected states
sendPromise.then(
  function(data) {
    console.log("IP Filter deleted");
  }).catch(
    function(err) {
    console.error(err, err.stack);
  });
```

To run the example, type the following at the command line\. The filter is deleted from Amazon SES\.

```
node ses_deletereceiptfilter.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/ses/ses_deletereceiptfilter.js)\.