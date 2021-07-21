--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Using IP address filters for email receipt in Amazon SES<a name="ses-examples-ip-filters"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create IP address filters to accept or reject mail that originates from an IP address or range of IP addresses\.
+ How to list your current IP address filters\.
+ How to delete an IP address filter\.

In Amazon SES, a *filter* is a data structure that consists of a name, an IP address range, and the functionality to allow or block mail from it\. IP addresses you want to block or allow are specified as a single IP address or a range of IP addresses in Classless Inter\-Domain Routing \(CIDR\) notation\. For details on how Amazon SES receives email, see [Amazon SES email\-receiving concepts](Amazon Simple Email Service Developer Guidereceiving-email-concepts.html) in the Amazon Simple Email Service Developer Guide\.

## The scenario<a name="ses-examples-receiving-email-scenario"></a>

In this example, a series of Node\.js modules are used to send email in a variety of ways\. The Node\.js modules use the SDK for JavaScript to create and use email templates using these methods of the `SES` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/createreceiptfiltercommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/createreceiptfiltercommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/listreceiptfilterscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/listreceiptfilterscommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/deletereceiptfiltercommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/deletereceiptfiltercommand.html)

## Prerequisite tasks<a name="ses-examples-ip-filters-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/ses/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Creating an IP address filter<a name="ses-examples-ip-filters-creating"></a>

In this example, use a Node\.js module to send email with Amazon SES\.

Create a `libs` directory, and create a Node\.js module with the file name `sesClient.js`\. Copy and paste the code below into it, which creates the Amazon SES client object\. Replace *REGION* with your AWS Region\.

```
import  { SESClient }  from  "@aws-sdk/client-ses";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SES service object.
const sesClient = new SESClient({ region: REGION });
export  { sesClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/libs/sesClient.js)\.

Create a Node\.js module with the file name `ses_createreceiptfilter.js`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the parameter values that define the IP filter, including the filter name, an IP address or range of addresses to filter, and whether to allow or block email traffic from the filtered addresses\. To call the `CreateReceiptFilterCommand` method, invoke an Amazon SES service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *IP\_ADDRESS\_OR\_RANGE* with the IP address or range of addresses to filter, *POLICY* with with `ALLOW` or `BLOCK`, and *NAME* with the filter name\.

```
// Import required AWS SDK clients and commands for Node.js
import { CreateReceiptFilterCommand } from "@aws-sdk/client-ses";
import { sesClient } from "./libs/sesClient.js";
// Set the parameters
const params = {
  Filter: {
    IpFilter: {
      Cidr: "IP_ADDRESS_OR_RANGE", // (in code; either a single IP address (10.0.0.1) or an IP address range in CIDR notation (10.0.0.1/24)),
      Policy: "POLICY", // 'ALLOW' or 'BLOCK' email traffic from the filtered addressesOptions.
    },
    Name: "NAME" // NAME (the filter name)
  },
};

const run = async () => {
  try {
    const data = await sesClient.send(new CreateReceiptFilterCommand(params));
    console.log("Success", data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\. The filter is created in Amazon SES\.

```
node ses_createreceiptfilter.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_createreceiptfilter.js)\.

## Listing your IP address filters<a name="ses-examples-ip-filters-listing"></a>

In this example, use a Node\.js module to send email with Amazon SES\. 

Create a `libs` directory, and create a Node\.js module with the file name `sesClient.js`\. Copy and paste the code below into it, which creates the Amazon SES client object\. Replace *REGION* with your AWS Region\.

```
import  { SESClient }  from  "@aws-sdk/client-ses";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SES service object.
const sesClient = new SESClient({ region: REGION });
export  { sesClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/libs/sesClient.js)\.

Create a Node\.js module with the file name `ses_listreceiptfilters.js`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an empty parameters object\. To call the `ListReceiptFiltersCommand` method, invoking an Amazon SES service object, passing the parameters\. 

```
// Import required AWS SDK clients and commands for Node.js
import { ListReceiptFiltersCommand }  from "@aws-sdk/client-ses";
import { sesClient } from "./libs/sesClient.js";

const run = async () => {
  try {
    const data = await sesClient.send(new ListReceiptFiltersCommand({}));
    console.log("Success.", data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\. Amazon SES returns the filter list\.

```
node ses_listreceiptfilters.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_listreceiptfilters.js)\.

## Deleting an IP address filter<a name="ses-examples-ip-filters-deleting"></a>

In this example, use a Node\.js module to send email with Amazon SES\. 

Create a `libs` directory, and create a Node\.js module with the file name `sesClient.js`\. Copy and paste the code below into it, which creates the Amazon SES client object\. Replace *REGION* with your AWS Region\.

```
import  { SESClient }  from  "@aws-sdk/client-ses";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create SES service object.
const sesClient = new SESClient({ region: REGION });
export  { sesClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/libs/sesClient.js)\.

Create a Node\.js module with the file name `ses_deletereceiptfilter.js`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the name of the IP filter to delete\. To call the `DeleteReceiptFilterCommand` method, invoke an Amazon SES client service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *FILTER\_NAME* with the name of the IP filter to delete\.

```
// Import required AWS SDK clients and commands for Node.js
import {
  DeleteReceiptFilterCommand
}  from "@aws-sdk/client-ses";
import { sesClient } from "./libs/sesClient.js";
// Set the parameters
const params = { FilterName: "FILTER_NAME" }; //FILTER_NAME

const run = async () => {
  try {
    const data = await sesClient.send(new DeleteReceiptFilterCommand(params));
    console.log("Success", data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\. The filter is deleted from Amazon SES\.

```
node ses_deletereceiptfilter.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_deletereceiptfilter.js)\.