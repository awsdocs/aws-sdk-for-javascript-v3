--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Create the browser script<a name="cross-service-submitdata-browser-script"></a>

This topic is part of a larger tutorial about using the AWS SDK for JavaScript with AWS Lambda functions\. To start at the beginning of the tutorial, see [Tutorial: Creating and using Lambda functions](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/sdk-cross-service-example-submitting-data.html)\. 

In this task, you will create the browser script\. 

To create the browser script for this example, in `DynamoDBApp`, create a Node\.js module with the file name `create_cognito_id_pool.ts`, complete it, and bundle the JavaScript using webpack\.

First, make sure to configure the SDK as previously shown, including installing the required clients and packages\. In `create_cognito_id_pool.ts`, load the required clients modules⁠— `CognitoIdentityClient`, `fromCognitoIdentityPool`, `DynamoDB`, and `SNSClient`⁠ — and commands\. Create an `S3` client service object, and initialize the Amazon Cognito credentials provider to provide the required credentials\. Then create a `Cognito` client service object, and initialize the Amazon Cognito credentials provider to provide the required credentials\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import required AWS SDK clients and commands for Node.js
const { CognitoIdentityClient } = require("@aws-sdk/client-cognito-identity");
const {
  fromCognitoIdentityPool,
} = require("@aws-sdk/credential-provider-cognito-identity");
const { DynamoDB, PutItemCommand } = require("@aws-sdk/client-dynamodb");
const { SNSClient, PublishCommand } = require("@aws-sdk/client-sns");

// Set the AWS Region
const REGION = "REGION"; //REGION

// Initialize the Amazon Cognito credentials provider
const IDENTITY_POOL_ID = "IDENTITY_POOL_ID";

const dbclient = new DynamoDB({
  region: REGION,
  credentials: fromCognitoIdentityPool({
    client: new CognitoIdentityClient({ region }),
    identityPoolId: IDENTITY_POOL_ID,
  }),
});

const sns = new SNSClient({
  region: REGION,
  credentials: fromCognitoIdentityPool({
    client: new CognitoIdentityClient({ region }),
    identityPoolId: IDENTITY_POOL_ID,
  }),
});
```

Most of the remainder of the code in this example is in a single function named `submitData`\. This function submits data to a DynamoDB table, and sends an SMS text to the app administrator using Amazon SNS\. 

In the `submitData` function, declare variables for the target phone number, the values entered on the app interface, and for the name of the Amazon S3 bucket\. Replace *BUCKET\_NAME* with the name of the `S3` bucket you created\. Next, create a parameters object for adding an item to the table\. If none of the values is empty, `submitData` adds the item to the table, and sends the message\. Remember to make the function available to the browser\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
const submitData = async () => {
  //Set the parameters
  // Capture the values entered in each field in the browser (by id).
  const id = document.getElementById("id").value;
  const title = document.getElementById("title").value;
  const name = document.getElementById("name").value;
  const body = document.getElementById("body").value;
  //Set the table name.
  const tableName = "TABLE_NAME";

  //Set the parameters for the table
  const params = {
    TableName: tableName,
    // Define the attributes and values of the item to be added. Adding ' + "" ' converts a value to
    // a string.
    Item: {
      Id: { N: id + "" },
      Title: { S: title + "" },
      Name: { N: name + "" },
      Body: { S: body + "" },
    },
  };
  // Check that all the fields are completed.
  if (id != "" && title != "" && name != "" && body != "") {
    try {
      //Upload the item to the table
      const data = await dbclient.send(new PutItemCommand(params));
      alert("Data added to table.");
      try {
        // Create the message parameters object.
        const messageParams = {
          Message: "A new item with ID value was added to the DynamoDB",
          PhoneNumber: "PHONE_NUMBER", //PHONE_NUMBER, in the E.164 phone number structure.
          // For example, ak standard local formatted number, such as (415) 555-2671, is +14155552671 in E.164
          // format, where '1' in the country code.
        };
        // Send the SNS message
        const data = await sns.send(new PublishCommand(messageParams));
        console.log(
          "Success, message published. MessageID is " + data.MessageId
        );
      } catch (err) {
        // Display error message if error is not sent
        console.error(err, err.stack);
      }
    } catch (err) {
      // Display error message if item is no added to table
      console.error(
        "An error occurred. Check the console for further information",
        err
      );
    }
    // Display alert if all field are not completed.
  } else {
    alert("Enter data in each field.");
  }
};
// Expose the function to the browser
window.submitData = submitData;
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/submit-data-app/src/dynamoApp/add_data.ts)\.

Finally, run the following at the command prompt to bundle the JavaScript for this example in a file named `main.js`:

```
webpack add_data.ts --mode development --target web --devtool false -o main.js
```

**Note**  
For information about installing webpack, see [Bundling applications with webpack](webpack.md)\.