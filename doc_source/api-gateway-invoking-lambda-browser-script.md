--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Creating the AWS Lambda function<a name="api-gateway-invoking-lambda-browser-script"></a>

## Configuring the SDK<a name="api-gateway-invoking-lambda-configure-sdk"></a>

In the `libs` directory create a files named `snsClient.js` and `lambdaClient.js`, and paste the content below into them respectively\. 

```
const { SNSClient } = require("@aws-sdk/client-sns");
// Set the AWS Region.
const REGION = "REGION"; // e.g. "us-east-1"
// Create an Amazon Simple Notification Service client object.
const snsClient = new SNSClient({region:REGION});
module.exports = { snsClient };
```

 Replace *REGION* with the AWS Region\. This code is available [ here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/lambda-api-gateway/src/libs/snsClient.js)\.

```
const { LambadaClient } = require( "@aws-sdk/client-lambda" );
// Set the AWS Region.
const REGION = "eu-west-1"; // e.g. "us-east-1"
// Create an Amazon Lambda service client object.
const lambdaClient = new LambdaClient({region:REGION});
module.exports = { lambdaClient };
```

Replace *REGION* with the AWS Region\. This code is available [ here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/lambda-api-gateway/src/libs/lambdaClient.js)\.

First import the required AWS SDK for JavaScript \(v3\) modules and commands\. Then calculate today's date and assign it to a parameter\. Then create the parameters for the `ScanCommand`\.Replace *TABLE\_NAME* with the name of the table you created in the [Create the AWS resources ](api-gateway-invoking-lambda-provision-resources.md) section of this example\.

The following code snippet shows this step\. \(See [Bundling the Lambda function](#api-gateway-invoking-lambda-full) for the full example\.\)

```
"use strict";
// Load the required clients and commands.
const { ScanCommand }  = require ( "@aws-sdk/client-dynamodb" );
const { PublishCommand } = require ( "@aws-sdk/client-sns" );
const {lambdaClient}  = require ( "./libs/lambdaClient" );
const {snsClient} = require ( "./libs/snsClient" );

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

## Scanning the DynamoDB table<a name="api-gateway-invoking-lambda-scan-table"></a>

First create an async/await function called `sendText` to publish a text message using the Amazon SNS `PublishCommand`\. Then, add a `try` block pattern that scans the DynamoDB table for employees with their work anniversary today, and then calls the `sendText` function to send these employees a text message\. If an error occurs the `catch` block is called\.

The following code snippet shows this step\. \(See [Bundling the Lambda function](#api-gateway-invoking-lambda-full) for the full example\.\)

```
exports.handler = async (event, context, callback) => {
  // Helper function to send message using Amazon SNS.
  async function sendText(textParams) {
    try {
      const data = await snsClient.send(new PublishCommand(textParams));
      console.log("Message sent");
    } catch (err) {
      console.log("Error, message not sent ", err);
    }
  }
  try {
    // Scan the table to check identify employees with work anniversary today.
    const data = await dynamoClient.send(new ScanCommand(params));
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

## Bundling the Lambda function<a name="api-gateway-invoking-lambda-full"></a>

This topic describes how to bundle the `mylambdafunction.ts` and the required AWS SDK for JavaScript modules for this example into a bundled file called `index.js`\. 

1. If you haven't already, follow the [Prerequisite tasks](api-gateway-invoking-lambda-prerequisites.md) for this example to install webpack\. 
**Note**  
For information about*webpack*, see [Bundling applications with webpack](webpack.md)\.

1. Run the the following in the command line to bundle the JavaScript for this example into a file called `<index.js>` :

   ```
   webpack mylambdafunction.ts --mode development --target node --devtool false --output-library-target umd -o index.js
   ```
**Important**  
Notice the output is named `index.js`\. This is because Lambda functions must have an `index.js` handler to work\.

1. Compress the bundled output file, `index.js`, into a ZIP file named `mylambdafunction.zip`\.

1. Upload `mylambdafunction.zip` to the Amazon S3 bucket you created in the [Create the AWS resources ](api-gateway-invoking-lambda-provision-resources.md) topic of this tutorial\. 