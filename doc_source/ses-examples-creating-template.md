--------

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
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/createtemplatecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/createtemplatecommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/gettemplatecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/gettemplatecommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/deletetemplatecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/deletetemplatecommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/updatetemplatecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-ses/classes/updatetemplatecommand.html)

## Prerequisite tasks<a name="ses-examples-creating-template-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/ses/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about creating a credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)\.

## Listing your email templates<a name="ses-examples-listing-templates"></a>

In this example, use a Node\.js module to create an email template to use with Amazon SES\. 

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

Create a Node\.js module with the file name `ses_listtemplates.js`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the parameters for the `ListTemplatesCommand` method of the `SES` client class\. To call the `ListTemplatesCommand` method, invoke an Amazon SES client service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *ITEMS\_COUNT* with the maximum number of templates to return\. The value must be a minimum of 1 and a maximum of 10\.

```
import { ListTemplatesCommand } from "@aws-sdk/client-ses";
import { sesClient } from "./libs/sesClient.js";

const createListTemplatesCommand = (maxItems) =>
  new ListTemplatesCommand({ MaxItems: maxItems });

const run = async () => {
  const listTemplatesCommand = createListTemplatesCommand(10);

  try {
    return await sesClient.send(listTemplatesCommand);
  } catch (err) {
    console.log("Failed to list templates.", err);
    return err;
  }
};
```

To run the example, enter the following at the command prompt\. Amazon SES returns the list of templates\.

```
node ses_listtemplates.js  
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_listtemplates.js)\.

## Getting an email template<a name="ses-examples-get-template"></a>

In this example, use a Node\.js module to get an email template to use with Amazon SES\.

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

Create a Node\.js module with the file name `ses_gettemplate.js`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the `TemplateName` parameter for the `GetTemplateCommand` method of the `SES` client class\. To call the `GetTemplateCommand` method, invoke an Amazon SES client service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *TEMPLATE\_NAME* with the name of the template to return\.

```
import { GetTemplateCommand } from "@aws-sdk/client-ses";
import { getUniqueName } from "../../libs/utils/util-string.js";
import { sesClient } from "./libs/sesClient.js";

const TEMPLATE_NAME = getUniqueName("TemplateName");

const createGetTemplateCommand = (templateName) =>
  new GetTemplateCommand({ TemplateName: templateName });

const run = async () => {
  const getTemplateCommand = createGetTemplateCommand(TEMPLATE_NAME);

  try {
    return await sesClient.send(getTemplateCommand);
  } catch (err) {
    console.log("Failed to get email template.", err);
    return err;
  }
};
```

To run the example, enter the following at the command prompt\. Amazon SES returns the template details\.

```
node ses_gettemplate.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_gettemplate.js)\.

## Creating an email template<a name="ses-examples-create-template"></a>

In this example, use a Node\.js module to create an email template to use with Amazon SES\. 

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

Create a Node\.js module with the file name `ses_createtemplate.js`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the parameters for the `CreateTemplateCommand` method of the `SES` client class, including `TemplateName`, `HtmlPart`, `SubjectPart`, and `TextPart`\. To call the `CreateTemplateCommand` method, invoke an Amazon SES client service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *TEMPLATE\_NAME* with a name for the new template, *HTML\_CONTENT* with the HTML tagged content of email, *SUBJECT* with the subject of the email, and *TEXT\_CONTENT* with the text of the email\.

```
import { CreateTemplateCommand } from "@aws-sdk/client-ses";
import { sesClient } from "./libs/sesClient.js";
import { getUniqueName } from "../../libs/utils/util-string.js";

const TEMPLATE_NAME = getUniqueName("TestTemplateName");

const createCreateTemplateCommand = () => {
  return new CreateTemplateCommand({
    /**
     * The template feature in Amazon SES is based on the Handlebars template system.
     */
    Template: {
      /**
       * The name of an existing template in Amazon SES.
       */
      TemplateName: TEMPLATE_NAME,
      HtmlPart: `
        <h1>Hello, {{contact.firstName}}!</h1>
        <p>
        Did you know Amazon has a mascot named Peccy?
        </p>
      `,
      SubjectPart: "Amazon Tip",
    },
  });
};

const run = async () => {
  const createTemplateCommand = createCreateTemplateCommand();

  try {
    return await sesClient.send(createTemplateCommand);
  } catch (err) {
    console.log("Failed to create template.", err);
    return err;
  }
};
```

To run the example, enter the following at the command prompt\. The template is added to Amazon SES\.

```
node ses_createtemplate.js  
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_createtemplate.js)\.

## Updating an email template<a name="ses-examples-update-template"></a>

In this example, use a Node\.js module to create an email template to use with Amazon SES\. 

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

Create a Node\.js module with the file name `ses_updatetemplate.js`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the `Template` parameter values you want to update in the template, with the required `TemplateName` parameter passed to the `UpdateTemplateCommand` method of the `SES` client class\. To call the `UpdateTemplateCommand` method, invoke an Amazon SES service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *TEMPLATE\_NAME* with a name of the template, *HTML\_CONTENT* with the HTML tagged content of email, *SUBJECT* with the subject of the email, and *TEXT\_CONTENT* with the text of the email\.

```
import { UpdateTemplateCommand } from "@aws-sdk/client-ses";
import { getUniqueName } from "../../libs/utils/util-string.js";
import { sesClient } from "./libs/sesClient.js";

const TEMPLATE_NAME = getUniqueName("TemplateName");
const HTML_PART = "<h1>Hello, World!</h1>";

const createUpdateTemplateCommand = () => {
  return new UpdateTemplateCommand({
    Template: {
      TemplateName: TEMPLATE_NAME,
      HtmlPart: HTML_PART,
      SubjectPart: "Example",
      TextPart: "Updated template text.",
    },
  });
};

const run = async () => {
  const updateTemplateCommand = createUpdateTemplateCommand();

  try {
    return await sesClient.send(updateTemplateCommand);
  } catch (err) {
    console.log("Failed to update template.", err);
    return err;
  }
};
```

To run the example, enter the following at the command prompt\. Amazon SES returns the template details\.

```
node ses_updatetemplate.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_updatetemplate.js)\.

## Deleting an email template<a name="ses-examples-delete-template"></a>

In this example, use a Node\.js module to create an email template to use with Amazon SES\. 

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

Create a Node\.js module with the file name `ses_deletetemplate.js`\. Configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the required`TemplateName` parameter to the `DeleteTemplateCommand` method of the `SES` client class\. To call the `DeleteTemplateCommand` method, invoke an Amazon SES service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *TEMPLATE\_NAME* with the name of the template to be deleted\.

```
import { DeleteTemplateCommand } from "@aws-sdk/client-ses";
import { getUniqueName } from "../../libs/utils/util-string.js";
import { sesClient } from "./libs/sesClient.js";

const TEMPLATE_NAME = getUniqueName("TemplateName");

const createDeleteTemplateCommand = (templateName) =>
  new DeleteTemplateCommand({ TemplateName: templateName });

const run = async () => {
  const deleteTemplateCommand = createDeleteTemplateCommand(TEMPLATE_NAME);

  try {
    return await sesClient.send(deleteTemplateCommand);
  } catch (err) {
    console.log("Failed to delete template.", err);
    return err;
  }
};
```

To run the example, enter the following at the command prompt\. Amazon SES returns the template details\.

```
node ses_deletetemplate.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/ses/src/ses_deletetemplate.js)\.