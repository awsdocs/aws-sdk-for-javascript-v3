--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Working with email templates in Amazon SES<a name="ses-examples-creating-template"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to get a list of all of your email templates\.
+ How to retrieve and update email templates\.
+ How to create and delete email templates\.

Amazon SES enables you ro send personalized email messages using email templates\. For details on how to create and use email templates in Amazon SES, see [Sending personalized email using the Amazon SES API](https://docs.aws.amazon.com/ses/latest/DeveloperGuide/send-personalized-email-api.html) in the Amazon Simple Email Service Developer Guide\.

## The scenario<a name="ses-examples-creating-template-scenario"></a>

In this example, you use a series of Node\.js modules to work with email templates\. The Node\.js modules use the SDK for JavaScript to create and use email templates using these methods of the `SES` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/listtemplatescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/listtemplatescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/createtemplatescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/createtemplatescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/gettemplatescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/gettemplatescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/deletetemplatescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/deletetemplatescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/updatetemplatescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/updatetemplatescommand.html)

## Prerequisite tasks<a name="ses-examples-creating-template-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/ses/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypeScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these examples can also be run in JavaScript\. For more information, see [this article](https://aws.amazon.com/blogs/developer/first-class-typescript-support-in-modular-aws-sdk-for-javascript/) in the AWS Developer Blog\.
+ Create a shared configurations file with your user credentials\. For more information about creating a credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Listing your email templates<a name="ses-examples-listing-templates"></a>

In this example, use a Node\.js module to create an email template to use with Amazon SES\. Create a Node\.js module with the file name `ses_listtemplates.ts`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the parameters for the `ListTemplatesCommand` method of the `SES` client class\. To call the `ListTemplatesCommand` method, invoke an Amazon SES client service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *ITEMS\_COUNT* with the maximum number of templates to return\. The value must be a minimum of 1 and a maximum of 10\.

```
// Import required AWS SDK clients and commands for Node.js
const { SESClient, ListTemplatesCommand } = require("@aws-sdk/client-ses");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { MaxItems: "ITEMS_COUNT" }; //ITEMS_COUNT

// Create SES service object
const ses = new SESClient({ region: REGION });

const run = async () => {
  try {
    const data = await ses.send(new ListTemplatesCommand({ params }));
    console.log(data);
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\. Amazon SES returns the list of templates\.

```
ts-node ses_listtemplates.ts  // If you prefer JavaScript, enter 'node ses_listtemplates.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_listtemplates.ts)\.

## Getting an email template<a name="ses-examples-get-template"></a>

In this example, use a Node\.js module to get an email template to use with Amazon SES\. Create a Node\.js module with the file name `ses_gettemplate.ts`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the `TemplateName` parameter for the `GetTemplateCommand` method of the `SES` client class\. To call the `GetTemplateCommand` method, invoke an Amazon SES client service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *TEMPLATE\_NAME* with the name of the template to return\.

```
// Import required AWS SDK clients and commands for Node.js
const { SESClient, GetTemplateCommand } = require("@aws-sdk/client-ses");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { TemplateName: "TEMPLATE_NAME" };

// Create SES service object
const ses = new SESClient({ region: REGION });

const run = async () => {
  try {
    const data = await ses.send(new GetTemplateCommand(params));
    console.log("Success. Template:", data.Template.SubjectPart);
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\. Amazon SES returns the template details\.

```
ts-node ses_gettemplate.ts // If you prefer JavaScript, enter 'node ses_gettemplate.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_gettemplate.ts)\.

## Creating an email template<a name="ses-examples-create-template"></a>

In this example, use a Node\.js module to create an email template to use with Amazon SES\. Create a Node\.js module with the file name `ses_createtemplate.ts`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the parameters for the `CreateTemplateCommand` method of the `SES` client class, including `TemplateName`, `HtmlPart`, `SubjectPart`, and `TextPart`\. To call the `CreateTemplateCommand` method, invoke an Amazon SES client service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *TEMPLATE\_NAME* with a name for the new template, *HTML\_CONTENT* with the HTML tagged content of email, *SUBJECT* with the subject of the email, and *TEXT\_CONTENT* with the text of the email\.

```
// Import required AWS SDK clients and commands for Node.js
const { SESClient, CreateTemplateCommand } = require("@aws-sdk/client-ses");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Create createTemplate params
const params = {
  Template: {
    TemplateName: "TEMPLATE_NAME", //TEMPLATE_NAME
    HtmlPart: "HTML_CONTENT",
    SubjectPart: "SUBJECT",
    TextPart: "TEXT_CONTENT",
  },
};

// Create SES service object
const ses = new SESClient({ region: REGION });

const run = async () => {
  try {
    const data = await ses.send(new CreateTemplateCommand(params));
    console.log(
      "Success, template created; requestID",
      data.$metadata.requestId
    );
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\. The template is added to Amazon SES\.

```
ts-node ses_createtemplate.ts  // If you prefer JavaScript, enter 'node ses_createtemplate.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_createtemplate.ts)\.

## Updating an email template<a name="ses-examples-update-template"></a>

In this example, use a Node\.js module to create an email template to use with Amazon SES\. Create a Node\.js module with the file name `ses_updatetemplate.ts`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the `Template` parameter values you want to update in the template, with the required `TemplateName` parameter passed to the `UpdateTemplateCommand` method of the `SES` client class\. To call the `UpdateTemplateCommand` method, invoke an Amazon SES service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *TEMPLATE\_NAME* with a name of the template, *HTML\_CONTENT* with the HTML tagged content of email, *SUBJECT* with the subject of the email, and *TEXT\_CONTENT* with the text of the email\.

```
// Import required AWS SDK clients and commands for Node.js
const { SESClient, UpdateTemplateCommand } = require("@aws-sdk/client-ses");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  Template: {
    TemplateName: "TEMPLATE_NAME", //TEMPLATE_NAME
    HtmlPart: "HTML_CONTENT", //HTML_CONTENT; i.e., HTML content in the email
    SubjectPart: "SUBJECT_LINE", //SUBJECT_LINE; i.e., email subject line
    TextPart: "TEXT_CONTENT", //TEXT_CONTENT; i.e., body of email
  },
};

// Create SES service object
const ses = new SESClient({ region: REGION });

const run = async () => {
  try {
    const data = await ses.send(new UpdateTemplateCommand(params));
    console.log("Template Updated");
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\. Amazon SES returns the template details\.

```
ts-node ses_updatetemplate.ts  // If you prefer JavaScript, enter 'node ses_updatetemplate.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_updatetemplate.ts)\.

## Deleting an email template<a name="ses-examples-delete-template"></a>

In this example, use a Node\.js module to create an email template to use with Amazon SES\. Create a Node\.js module with the file name `ses_deletetemplate.ts`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the required`TemplateName` parameter to the `DeleteTemplateCommand` method of the `SES` client class\. To call the `DeleteTemplateCommand` method, invoke an Amazon SES service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *TEMPLATE\_NAME* with the name of the template to be deleted\.

```
// Import required AWS SDK clients and commands for Node.js
const { SESClient, DeleteTemplateCommand } = require("@aws-sdk/client-ses");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { TemplateName: "TEMPLATE_NAME" };

// Create SES service object
const ses = new SESClient({ region: REGION });

const run = async () => {
  try {
    const data = await ses.send(new DeleteTemplateCommand(params));
    console.log("Template deleted.");
  } catch (err) {
    console.error(err, err.stack);
  }
};
run();
```

To run the example, enter the following at the command prompt\. Amazon SES returns the template details\.

```
ts-node ses_deletetemplate.ts // If you prefer JavaScript, enter 'node ses_deletetemplate.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_deletetemplate.ts)\.