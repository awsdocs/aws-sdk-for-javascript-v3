--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Create the browser script<a name="cross-service-submitdata-browser-script"></a>

First, create the service client objects required for the example\. Create a `libs` directory, create `snsClient.js`, and paste the code below into it\. Replace *REGION* and *IDENTITY\_POOL\_ID* in each\. 

**Note**  
Use the ID of the Amazon Cognito identity pool you created in [Create the AWS resources ](s3-crossservices-adddata-provision-resources.md)\.

```
import { CognitoIdentityClient } from "@aws-sdk/client-cognito-identity";
import { fromCognitoIdentityPool } from "@aws-sdk/credential-provider-cognito-identity";
import { SNSClient } from "@aws-sdk/client-sns";

const REGION = "REGION";
const IDENTITY_POOL_ID = "IDENTITY_POOL_ID"; // An Amazon Cognito Identity Pool ID.

// Create an Amazon Comprehend service client object.
const snsClient = new SNSClient({
  region: REGION,
  credentials: fromCognitoIdentityPool({
    client: new CognitoIdentityClient({ region: REGION }),
    identityPoolId: IDENTITY_POOL_ID,
  }),
});

export { snsClient };
```

This code is available [here on GitHub\.](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/cross-services/submit-data-app/src/libs/snsClient.js)\.

To create the browser script for this example, in a folder named `DynamoDBApp`, create a Node\.js module with the file name `add_data.js` and paste the code below into it\. The `submitData` function submits data to a DynamoDB table, and sends an SMS text to the app administrator using Amazon SNS\. 

In the `submitData` function, declare variables for the target phone number, the values entered on the app interface, and for the name of the Amazon S3 bucket\. Next, create a parameters object for adding an item to the table\. If none of the values is empty, `submitData` adds the item to the table, and sends the message\. Remember to make the function available to the browser, with `window.submitData = submitData`\.

```
// Import required AWS SDK clients and commands for Node.js
import { PutItemCommand } from "@aws-sdk/client-dynamodb";
import { PublishCommand } from "@aws-sdk/client-sns";
import { snsClient } from "../libs/snsClient.js";
import { dynamoClient } from "../libs/dynamoClient.js";

export const submitData = async () => {
  //Set the parameters
  // Capture the values entered in each field in the browser (by id).
  const id = document.getElementById("id").value;
  const title = document.getElementById("title").value;
  const name = document.getElementById("name").value;
  const body = document.getElementById("body").value;
  //Set the table name.
  const tableName = "Items";

  //Set the parameters for the table
  const params = {
    TableName: tableName,
    // Define the attributes and values of the item to be added. Adding ' + "" ' converts a value to
    // a string.
    Item: {
      id: { N: id + "" },
      title: { S: title + "" },
      name: { S: name + "" },
      body: { S: body + "" },
    },
  };
  // Check that all the fields are completed.
  if (id != "" && title != "" && name != "" && body != "") {
    try {
      //Upload the item to the table
      await dynamoClient.send(new PutItemCommand(params));
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
        const data = await snsClient.send(new PublishCommand(messageParams));
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

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/cross-services/submit-data-app/src/dynamoApp/add_data.js)\.

Finally, run the following at the command prompt to bundle the JavaScript for this example in a file named `main.js`:

```
webpack add_data.js --mode development --target web --devtool false -o main.js
```

**Note**  
For information about installing webpack, see [Bundling applications with webpack](webpack.md)\.

To run the app, open `index.html` on your browser\.