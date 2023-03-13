--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Amazon Personalize Events examples using SDK for JavaScript \(v3\)<a name="javascript_personalize-events_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for JavaScript \(v3\) with Amazon Personalize Events\.

*Actions* are code excerpts that show you how to call individual service functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple functions within the same service\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#actions)

## Actions<a name="actions"></a>

### Import real\-time interaction event data<a name="personalize-events_putEvents_javascript_topic"></a>

The following code example shows how to import real\-time interaction event data into Amazon Personalize Events\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/personalize#code-examples)\. 
  

```
// Get service clients module and commands using ES6 syntax.
import { PutEventsCommand } from
  "@aws-sdk/client-personalize-events";
import { personalizeEventsClient } from "./libs/personalizeClients.js";
// Or, create the client here.
// const personalizeEventsClient = new PersonalizeEventsClient({ region: "REGION"});

// Convert your UNIX timestamp to a Date.
const sentAtDate = new Date(1613443801 * 1000)  // 1613443801 is a testing value. Replace it with your sentAt timestamp in UNIX format.

// Set put events parameters.
var putEventsParam = {
  eventList: [          /* required */
    {
      eventType: 'EVENT_TYPE',     /* required */
      sentAt: sentAtDate,           /* required, must be a Date with js */
      eventId: 'EVENT_ID',    /* optional */
      itemId: 'ITEM_ID'         /* optional */
    }
  ],
  sessionId: 'SESSION_ID',      /* required */
  trackingId: 'TRACKING_ID', /* required */
  userId: 'USER_ID'  /* required */
};
export const run = async () => {
  try {
    const response = await personalizeEventsClient.send(new PutEventsCommand(putEventsParam));
    console.log("Success!", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  For API details, see [PutEvents](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-personalize-events/classes/puteventscommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Incrementally import a user<a name="personalize-events_putUsers_javascript_topic"></a>

The following code example shows how to incrementally import a user into Amazon Personalize Events Events\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/personalize#code-examples)\. 
  

```
// Get service clients module and commands using ES6 syntax.
import { PutUsersCommand } from
  "@aws-sdk/client-personalize-events";
import { personalizeEventsClient } from "./libs/personalizeClients.js";
// Or, create the client here.
// const personalizeEventsClient = new PersonalizeEventsClient({ region: "REGION"});

// Set the put users parameters. For string properties and values, use the \ character to escape quotes.
var putUsersParam = {
    datasetArn: "DATASET_ARN",
    users: [ 
      {
        'userId': 'USER_ID',
        'properties': "{\"PROPERTY1_NAME\": \"PROPERTY1_VALUE\"}"   
      }
    ]
};
export const run = async () => {
  try {
    const response = await personalizeEventsClient.send(new PutUsersCommand(putUsersParam));
    console.log("Success!", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  For API details, see [PutUsers](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-personalize-events/classes/putuserscommand.html) in *AWS SDK for JavaScript API Reference*\. 