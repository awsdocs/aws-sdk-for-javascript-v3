--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Creating the AWS Lambda function<a name="scheduled-events-invoking-lambda-browser-script"></a>

## Configuring the SDK<a name="scheduled-events-invoking-lambda-configure-sdk"></a>

First import the required AWS SDK for JavaScript \(v3\) modules and commands: `DynamoDBClient` and the DynamoDB `ScanCommand`, and `SNSClient` and the Amazon SNS `PublishCommand` command\. Replace *REGION* with the AWS Region\. Then calculate today's date and assign it to a parameter\. Then create the parameters for the `ScanCommand`\.Replace *TABLE\_NAME* with the name of the table you created in the [Create the AWS resources ](scheduled-events-invoking-lambda-provision-resources.md) section of this example\.

The following code snippet shows this step\. \(See [Bundling the Lambda function](#scheduled-events-invoking-lambda-full) for the full example\.\)

```
"use strict";
// Load the required clients and commands.
const { DynamoDBClient, ScanCommand } = require("@aws-sdk/client-dynamodb");
const { SNSClient, PublishCommand } = require("@aws-sdk/client-sns");

//Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"

// Get today's date.
const today = new Date();
const dd = String(today.getDate()).padStart(2, "0");
const mm = String(today.getMonth() + 1).padStart(2, "0"); //January is 0!
const yyyy = today.getFullYear();
const date = yyyy + "-" + mm + "-" + dd;

// Set the parameters for the ScanCommand method.
const params = {
  // Specify which items in the results are returned.
  FilterExpression: "startDate = :topic",
  // Define the expression attribute value, which are substitutes for the values you want to compare.
  ExpressionAttributeValues: {
    ":topic": { S: date },
  },
  // Set the projection expression, which the the attributes that you want.
  ProjectionExpression: "firstName, phone",
  TableName: "TABLE_NAME",
};
```

## Scanning the DynamoDB table<a name="scheduled-events-invoking-lambda-scan-table"></a>

First create an async/await function called `sendText` to publish a text message using the Amazon SNS `PublishCommand`\. Then, add a `try` block pattern that scans the DynamoDB table for employees with their work anniversry today, and then calls the `sendText` function to send these employees a text message\. If an error occurs the `catch` block is called\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

The following code snippet shows this step\. \(See [Bundling the Lambda function](#scheduled-events-invoking-lambda-full) for the full example\.\)

```
exports.handler = async (event, context, callback) => {
  // Helper function to send message using Amazon SNS.
  async function sendText(textParams) {
    try {
      const data = await snsclient.send(new PublishCommand(textParams));
      console.log("Message sent");
    } catch (err) {
      console.log("Error, message not sent ", err);
    }
  }
  try {
    // Scan the table to check identify employees with work anniversary today.
    const data = await dbclient.send(new ScanCommand(params));
    data.Items.forEach(function (element, index, array) {
      const textParams = {
        PhoneNumber: element.phone.N,
        Message:
          "Hi " +
          element.firstName.S +
          "; congratulations on your work anniversary!",
      };
      // Send message using Amazon SNS.
      sendText(textParams);
    });
  } catch (err) {
    console.log("Error, could not scan table ", err);
  }
};
```

## Bundling the Lambda function<a name="scheduled-events-invoking-lambda-full"></a>

This topic describes how to bundle the `mylambdafunction.ts` and the required AWS SDK for JavaScript modules for this example into a bundled file called `index.js`\. 

1. If you haven't already, follow the [Prerequisite tasks](scheduled-events-invoking-lambda-prerequisites.md) for this example to install webpack\. 
**Note**  
For information about*webpack*, see [Bundling applications with webpack](webpack.md)\.

1. Run the the following in the command line to bundle the JavaScript for this example into a file called `<index.js>` :

   ```
   webpack mylambdafunction.ts --mode development --libraryTarget commonjs2 --target node --devtool false -o index.js
   ```
**Important**  
Notice the output is named `index.js`\. This is because Lambda functions must have an `index.js` handler to work\.

1. Compress the bundled output file, `index.js`, into a ZIP file named `my-lambda-function.zip`\.

1. Upload `mylambdafunction.zip` to the Amazon S3 bucket you created in the [Create the AWS resources ](scheduled-events-invoking-lambda-provision-resources.md) topic of this tutorial\. 

Here is the complete browser script code for `mylambdafunction.ts`\.

```
"use strict";
// Load the required clients and commands.
const { DynamoDBClient, ScanCommand } = require("@aws-sdk/client-dynamodb");
const { SNSClient, PublishCommand } = require("@aws-sdk/client-sns");

//Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"

// Get today's date.
const today = new Date();
const dd = String(today.getDate()).padStart(2, "0");
const mm = String(today.getMonth() + 1).padStart(2, "0"); //January is 0!
const yyyy = today.getFullYear();
const date = yyyy + "-" + mm + "-" + dd;

// Set the parameters for the ScanCommand method.
const params = {
  // Specify which items in the results are returned.
  FilterExpression: "startDate = :topic",
  // Define the expression attribute value, which are substitutes for the values you want to compare.
  ExpressionAttributeValues: {
    ":topic": { S: date },
  },
  // Set the projection expression, which the the attributes that you want.
  ProjectionExpression: "firstName, phone",
  TableName: "TABLE_NAME",
};

// Create the client service objects.
const dbclient = new DynamoDBClient({ region: REGION });
const snsclient = new SNSClient({ region: REGION });

exports.handler = async (event, context, callback) => {
  // Helper function to send message using Amazon SNS.
  async function sendText(textParams) {
    try {
      const data = await snsclient.send(new PublishCommand(textParams));
      console.log("Message sent");
    } catch (err) {
      console.log("Error, message not sent ", err);
    }
  }
  try {
    // Scan the table to check identify employees with work anniversary today.
    const data = await dbclient.send(new ScanCommand(params));
    data.Items.forEach(function (element, index, array) {
      const textParams = {
        PhoneNumber: element.phone.N,
        Message:
          "Hi " +
          element.firstName.S +
          "; congratulations on your work anniversary!",
      };
      // Send message using Amazon SNS.
      sendText(textParams);
    });
  } catch (err) {
    console.log("Error, could not scan table ", err);
  }
};
```