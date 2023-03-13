--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# AWS Support examples using SDK for JavaScript \(v3\)<a name="javascript_support_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for JavaScript \(v3\) with AWS Support\.

*Actions* are code excerpts that show you how to call individual service functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple functions within the same service\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Get started**

## Hello AWS Support<a name="example_support_Hello_section"></a>

The following code examples show how to get started using AWS Support\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/support#code-examples)\. 
Invoke `main\(\)` to run the example\.  

```
import {
  DescribeServicesCommand,
  SupportClient,
} from "@aws-sdk/client-support";

// Change the value of 'region' to your preferred AWS Region.
const client = new SupportClient({ region: "us-east-1" });

const getServiceCount = async () => {
  try {
    const { services } = await client.send(new DescribeServicesCommand({}));
    return services.length;
  } catch (err) {
    if (err.name === "SubscriptionRequiredException") {
      throw new Error(
        "You must be subscribed to the AWS Support plan to use this feature."
      );
    } else {
      throw err;
    }
  }
};

export const main = async () => {
  try {
    const count = await getServiceCount();
    console.log(`Hello, AWS Support! There are ${count} services available.`);
  } catch (err) {
    console.error("Failed to get service count: ", err.message);
  }
};
```
+  For API details, see [DescribeServices](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-support/classes/describeservicescommand.html) in *AWS SDK for JavaScript API Reference*\. 

**Topics**
+ [Actions](#actions)
+ [Scenarios](#scenarios)

## Actions<a name="actions"></a>

### Add a communication to a case<a name="support_AddCommunication_javascript_topic"></a>

The following code example shows how to add an AWS Support communication with an attachment to a support case\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/support#code-examples)\. 
  

```
import { AddCommunicationToCaseCommand } from "@aws-sdk/client-support";

import { client } from "../libs/client.js";

export const main = async () => {
  let attachmentSetId;

  try {
    // Add a communication to a case.
    const response = await client.send(
      new AddCommunicationToCaseCommand({
        communicationBody: "Adding an attachment.",
        // Set value to an existing support case id.
        caseId: "CASE_ID",
        // Optional. Set value to an existing attachment set id to add attachments to the case.
        attachmentSetId,
      })
    );
    console.log(response);
    return response;
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [AddCommunicationToCase](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-support/classes/addcommunicationtocasecommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Add an attachment to a set<a name="support_AddAttachment_javascript_topic"></a>

The following code example shows how to add an AWS Support attachment to an attachment set\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/support#code-examples)\. 
  

```
import { AddAttachmentsToSetCommand } from "@aws-sdk/client-support";

import { client } from "../libs/client.js";

export const main = async () => {
  try {
    // Create a new attachment set or add attachments to an existing set.
    // Provide an 'attachmentSetId' value to add attachments to an existing set.
    // Use AddCommunicationToCase or CreateCase to associate an attachment set with a support case.
    const response = await client.send(
      new AddAttachmentsToSetCommand({
        // You can add up to three attachments per set. The size limit is 5 MB per attachment.
        attachments: [
          {
            fileName: "example.txt",
            data: new TextEncoder().encode("some example text"),
          },
        ],
      })
    );
    // Use this ID in AddCommunicationToCase or CreateCase.
    console.log(response.attachmentSetId);
    return response;
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [AddAttachmentsToSet](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-support/classes/addattachmentstosetcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create a case<a name="support_CreateCase_javascript_topic"></a>

The following code example shows how to create a new AWS Support case\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/support#code-examples)\. 
  

```
import { CreateCaseCommand } from "@aws-sdk/client-support";

import { client } from "../libs/client.js";

export const main = async () => {
  try {
    // Create a new case and log the case id.
    // Important: This creates a real support case in your account.
    const response = await client.send(
      new CreateCaseCommand({
        // The subject line of the case.
        subject: "IGNORE: Test case",
        // Use DescribeServices to find available service codes for each service.
        serviceCode: "service-quicksight-end-user",
        // Use DescribeSecurityLevels to find available severity codes for your support plan.
        severityCode: "low",
        // Use DescribeServices to find available category codes for each service.
        categoryCode: "end-user-support",
        // The main description of the support case. 
        communicationBody: "This is a test. Please ignore."
      })
    );
    console.log(response.caseId);
    return response;
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [CreateCase](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-support/classes/createcasecommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Describe an attachment<a name="support_DescribeAttachment_javascript_topic"></a>

The following code example shows how to describe an attachment for an AWS Support case\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/support#code-examples)\. 
  

```
import { DescribeAttachmentCommand } from "@aws-sdk/client-support";

import { client } from "../libs/client.js";

export const main = async () => {
  try {
    // Get the metadata and content of an attachment.
    const response = await client.send(
      new DescribeAttachmentCommand({
        // Set value to an existing attachment id.
        // Use DescribeCommunications or DescribeCases to find an attachment id.
        attachmentId: "ATTACHMENT_ID",
      })
    );
    console.log(response.attachment?.fileName);
    return response;
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [DescribeAttachment](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-support/classes/describeattachmentcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Describe cases<a name="support_DescribeCases_javascript_topic"></a>

The following code example shows how to describe AWS Support cases\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/support#code-examples)\. 
  

```
import { DescribeCasesCommand } from "@aws-sdk/client-support";

import { client } from "../libs/client.js";

export const main = async () => {
  try {
    // Get all of the unresolved cases in your account.
    // Filter or expand results by providing parameters to the DescribeCasesCommand. Refer
    // to the TypeScript definition and the API doc for more information on possible parameters.
    // https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-support/interfaces/describecasescommandinput.html
    const response = await client.send(new DescribeCasesCommand({}));
    const caseIds = response.cases.map((supportCase) => supportCase.caseId);
    console.log(caseIds);
    return response;
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [DescribeCases](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-support/classes/describecasescommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Describe communications<a name="support_DescribeCommunications_javascript_topic"></a>

The following code example shows how to describe AWS Support communications for a case\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/support#code-examples)\. 
  

```
import { DescribeCommunicationsCommand } from "@aws-sdk/client-support";

import { client } from "../libs/client.js";

export const main = async () => {
  try {
    // Get all communications for the support case.
    // Filter results by providing parameters to the DescribeCommunicationsCommand. Refer
    // to the TypeScript definition and the API doc for more information on possible parameters.
    // https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-support/interfaces/describecommunicationscommandinput.html
    const response = await client.send(
      new DescribeCommunicationsCommand({
        // Set value to an existing case id.
        caseId: "CASE_ID",
      })
    );
    const text = response.communications.map((item) => item.body).join("\n");
    console.log(text);
    return response;
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [DescribeCommunications](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-support/classes/describecommunicationscommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Describe severity levels<a name="support_DescribeSeverityLevels_javascript_topic"></a>

The following code example shows how to describe AWS Support severity levels\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/support#code-examples)\. 
  

```
import { DescribeSeverityLevelsCommand } from "@aws-sdk/client-support";

import { client } from "../libs/client.js";

export const main = async () => {
  try {
    // Get the list of severity levels.
    // The available values depend on the support plan for the account.
    const response = await client.send(new DescribeSeverityLevelsCommand({}));
    console.log(response.severityLevels)
    return response;
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [DescribeSeverityLevels](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-support/classes/describeseveritylevelscommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Resolve case<a name="support_ResolveCase_javascript_topic"></a>

The following code example shows how to resolve an AWS Support case\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/support#code-examples)\. 
  

```
import { ResolveCaseCommand } from "@aws-sdk/client-support";

import { client } from "../libs/client.js";

const main = async () => {
  try {
    const response = await client.send(
      new ResolveCaseCommand({
        caseId: "CASE_ID",
      })
    );

    console.log(response.finalCaseStatus);
    return response;
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [ResolveCase](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-support/classes/resolvecasecommand.html) in *AWS SDK for JavaScript API Reference*\. 

## Scenarios<a name="scenarios"></a>

### Get started with cases<a name="support_Scenario_GetStartedSupportCases_javascript_topic"></a>

The following code example shows how to:
+ Get and display available services and severity levels for cases\.
+ Create a support case using a selected service, category, and severity level\.
+ Get and display a list of open cases for the current day\.
+ Add an attachment set and a communication to the new case\.
+ Describe the new attachment and communication for the case\.
+ Resolve the case\.
+ Get and display a list of resolved cases for the current day\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/support#code-examples)\. 
Run an interactive scenario in the terminal\.  

```
import {
  AddAttachmentsToSetCommand,
  AddCommunicationToCaseCommand,
  CreateCaseCommand,
  DescribeAttachmentCommand,
  DescribeCasesCommand,
  DescribeCommunicationsCommand,
  DescribeServicesCommand,
  DescribeSeverityLevelsCommand,
  ResolveCaseCommand,
  SupportClient,
} from "@aws-sdk/client-support";
import inquirer from "inquirer";

// Retry an asynchronous function on failure.
const retry = async ({ intervalInMs = 500, maxRetries = 10 }, fn) => {
  try {
    return await fn();
  } catch (err) {
    console.log(`Function call failed. Retrying.`);
    console.error(err.message);
    if (maxRetries === 0) throw err;
    await new Promise((resolve) => setTimeout(resolve, intervalInMs));
    return retry({ intervalInMs, maxRetries: maxRetries - 1 }, fn);
  }
};

const wrapText = (text, char = "=") => {
  const rule = char.repeat(80);
  return `${rule}\n    ${text}\n${rule}\n`;
};

const client = new SupportClient({ region: "us-east-1" });

// Verify that the account has a Support plan.
export const verifyAccount = async () => {
  const command = new DescribeServicesCommand({});

  try {
    await client.send(command);
  } catch (err) {
    if (err.name === "SubscriptionRequiredException") {
      throw new Error(
        "You must be subscribed to the AWS Support plan to use this feature."
      );
    } else {
      throw err;
    }
  }
};

// Get the list of available services.
export const getService = async () => {
  const { services } = await client.send(new DescribeServicesCommand({}));
  const { selectedService } = await inquirer.prompt({
    name: "selectedService",
    type: "list",
    message:
      "Select a service. Your support case will be created for this service. The list of services is truncated for readability.",
    choices: services.slice(0, 10).map((s) => ({ name: s.name, value: s })),
  });
  return selectedService;
};

// Get the list of available support case categories for a service.
export const getCategory = async (service) => {
  const { selectedCategory } = await inquirer.prompt({
    name: "selectedCategory",
    type: "list",
    message: "Select a category.",
    choices: service.categories.map((c) => ({ name: c.name, value: c })),
  });
  return selectedCategory;
};

// Get the available severity levels for the account.
export const getSeverityLevel = async () => {
  const command = new DescribeSeverityLevelsCommand({});
  const { severityLevels } = await client.send(command);
  const { selectedSeverityLevel } = await inquirer.prompt({
    name: "selectedSeverityLevel",
    type: "list",
    message: "Select a severity level.",
    choices: severityLevels.map((s) => ({ name: s.name, value: s })),
  });
  return selectedSeverityLevel;
};

// Create a new support case and return the caseId.
export const createCase = async ({
  selectedService,
  selectedCategory,
  selectedSeverityLevel,
}) => {
  const command = new CreateCaseCommand({
    subject: "IGNORE: Test case",
    communicationBody: "This is a test. Please ignore.",
    serviceCode: selectedService.code,
    categoryCode: selectedCategory.code,
    severityCode: selectedSeverityLevel.code,
  });
  const { caseId } = await client.send(command);
  return caseId;
};

// Get a list of open support cases created today.
export const getTodaysOpenCases = async () => {
  const d = new Date();
  const startOfToday = new Date(d.getFullYear(), d.getMonth(), d.getDate());
  const command = new DescribeCasesCommand({
    includeCommunications: false,
    afterTime: startOfToday.toISOString(),
  });

  const { cases } = await client.send(command);

  if (cases.length === 0) {
    throw new Error(
      "Unexpected number of cases. Expected more than 0 open cases."
    );
  }
  return cases;
};

// Create an attachment set.
export const createAttachmentSet = async () => {
  const command = new AddAttachmentsToSetCommand({
    attachments: [
      {
        fileName: "example.txt",
        data: new TextEncoder().encode("some example text"),
      },
    ],
  });
  const { attachmentSetId } = await client.send(command);
  return attachmentSetId;
};

export const linkAttachmentSetToCase = async (attachmentSetId, caseId) => {
  const command = new AddCommunicationToCaseCommand({
    attachmentSetId,
    caseId,
    communicationBody: "Adding attachment set to case.",
  });
  await client.send(command);
};

// Get all communications for a support case.
export const getCommunications = async (caseId) => {
  const command = new DescribeCommunicationsCommand({
    caseId,
  });
  const { communications } = await client.send(command);
  return communications;
};

// Get an attachment set.
export const getFirstAttachment = (communications) => {
  const firstCommWithAttachment = communications.find(
    (c) => c.attachmentSet.length > 0
  );
  return firstCommWithAttachment?.attachmentSet[0].attachmentId;
};

// Get an attachment.
export const getAttachment = async (attachmentId) => {
  const command = new DescribeAttachmentCommand({
    attachmentId,
  });
  const { attachment } = await client.send(command);
  return attachment;
};

// Resolve the case matching the given case ID.
export const resolveCase = async (caseId) => {
  const { shouldResolve } = await inquirer.prompt({
    name: "shouldResolve",
    type: "confirm",
    message: `Do you want to resolve ${caseId}?`,
  });

  if (shouldResolve) {
    const command = new ResolveCaseCommand({
      caseId: caseId,
    });

    await client.send(command);
    return true;
  }
  return false;
};

// Find a specific case in the list of provided cases by case ID.
// If the case is not found, and the results are paginated, continue
// paging through the results.
export const findCase = async ({ caseId, cases, nextToken }) => {
  const foundCase = cases.find((c) => c.caseId === caseId);

  if (foundCase) {
    return foundCase;
  }

  if (nextToken) {
    const response = await client.send(
      new DescribeCasesCommand({
        nextToken,
        includeResolvedCases: true,
      })
    );
    return findCase({
      caseId,
      cases: response.cases,
      nextToken: response.nextToken,
    });
  }

  throw new Error(`${caseId} not found.`);
};

// Get all cases created today.
export const getTodaysResolvedCases = async (caseIdToWaitFor) => {
  const d = new Date("2023-01-18");
  const startOfToday = new Date(d.getFullYear(), d.getMonth(), d.getDate());
  const command = new DescribeCasesCommand({
    includeCommunications: false,
    afterTime: startOfToday.toISOString(),
    includeResolvedCases: true,
  });
  const { cases, nextToken } = await client.send(command);
  await findCase({ cases, caseId: caseIdToWaitFor, nextToken });
  return cases.filter((c) => c.status === "resolved");
};

const main = async () => {
  let caseId;
  try {
    console.log(wrapText("Welcome to the AWS Support basic usage scenario."));

    // Verify that the account is subscribed to support.
    await verifyAccount();

    // Provided a truncated list of services and prompt the user to select one.
    const selectedService = await getService();

    // Provided the categories for the selected service and prompt the user to select one.
    const selectedCategory = await getCategory(selectedService);

    // Provide the severity available severity levels for the account and prompt the user to select one.
    const selectedSeverityLevel = await getSeverityLevel();

    // Create a support case.
    console.log("\nCreating a support case.");
    caseId = await createCase({
      selectedService,
      selectedCategory,
      selectedSeverityLevel,
    });
    console.log(`Support case created: ${caseId}`);

    // Display a list of open support cases created today.
    const todaysOpenCases = await retry(
      { intervalInMs: 1000, maxRetries: 15 },
      getTodaysOpenCases
    );
    console.log(
      `\nOpen support cases created today: ${todaysOpenCases.length}`
    );
    console.log(todaysOpenCases.map((c) => `${c.caseId}`).join("\n"));

    // Create an attachment set.
    console.log("\nCreating an attachment set.");
    const attachmentSetId = await createAttachmentSet();
    console.log(`Attachment set created: ${attachmentSetId}`);

    // Add the attachment set to the support case.
    console.log(`\nAdding attachment set to ${caseId}`);
    await linkAttachmentSetToCase(attachmentSetId, caseId);
    console.log(`Attachment set added to ${caseId}`);

    // List the communications for a support case.
    console.log(`\nListing communications for ${caseId}`);
    const communications = await getCommunications(caseId);
    console.log(
      communications
        .map(
          (c) =>
            `Communication created on ${c.timeCreated}. Has ${c.attachmentSet.length} attachments.`
        )
        .join("\n")
    );

    // Describe the first attachment.
    console.log(`\nDescribing attachment ${attachmentSetId}`);
    const attachmentId = getFirstAttachment(communications);
    const attachment = await getAttachment(attachmentId);
    console.log(
      `Attachment is the file '${
        attachment.fileName
      }' with data: \n${new TextDecoder().decode(attachment.data)}`
    );

    // Confirm that the support case should be resolved.
    const isResolved = await resolveCase(caseId);
    if (isResolved) {
      // List the resolved cases and include the one previously created.
      // Resolved cases can take a while to appear.
      console.log(
        "\nWaiting for case status to be marked as resolved. This can take some time."
      );
      const resolvedCases = await retry(
        { intervalInMs: 20000, maxRetries: 15 },
        () => getTodaysResolvedCases(caseId)
      );
      console.log("Resolved cases:");
      console.log(resolvedCases.map((c) => c.caseId).join("\n"));
    }
  } catch (err) {
    console.error(err);
  }
};
```
+ For API details, see the following topics in *AWS SDK for JavaScript API Reference*\.
  + [AddAttachmentsToSet](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-support/classes/addattachmentstosetcommand.html)
  + [AddCommunicationToCase](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-support/classes/addcommunicationtocasecommand.html)
  + [CreateCase](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-support/classes/createcasecommand.html)
  + [DescribeAttachment](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-support/classes/describeattachmentcommand.html)
  + [DescribeCases](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-support/classes/describecasescommand.html)
  + [DescribeCommunications](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-support/classes/describecommunicationscommand.html)
  + [DescribeServices](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-support/classes/describeservicescommand.html)
  + [DescribeSeverityLevels](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-support/classes/describeseveritylevelscommand.html)
  + [ResolveCase](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-support/classes/resolvecasecommand.html)