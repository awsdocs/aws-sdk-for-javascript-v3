--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Create the Lambda function<a name="lambda-create-table-create-lambda-function"></a>

This topic is part of a tutorial that demonstrates how to create, deploy, and run a Lambda function using the AWS SDK for JavaScript\. To start at the beginning of the tutorial, see [Tutorial: Create and using Lambda functions](lambda-create-table-example.md)\.

In the root of your project, create an `mylambdafunction.ts` file, and copy and paste the content below into it\. Replace *"REGION"* with the AWS region\.

Run the following command to bundle the Lambda function and the required Amazon Web Services modules\.

```
webpack mylambdafunction.ts --mode development --target web --devtool false -o index.js
```

**Important**  
Notice the output is named `index.js`\. This is because Lambda functions must have an `index.js` handler to work\.

Compress the bundled output file, `index.js`, into a ZIP file named `my-lambda-function.zip`\.

Upload `my-lambda-function.zip` to the Amazon S3 bucket you created in the [Create the AWS resources ](lambda-create-table-provision-resources.md) topic of this tutorial\. 

```
"use strict";
// Load the required clients and packages.
const { DynamoDBClient, CreateTableCommand } = require("@aws-sdk/client-dynamodb");

//Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters.
const params = {
    AttributeDefinitions: [
        {
            AttributeName: "Season", //ATTRIBUTE_NAME_1
            AttributeType: "N", //ATTRIBUTE_TYPE
        },
        {
            AttributeName: "Episode", //ATTRIBUTE_NAME_2
            AttributeType: "N", //ATTRIBUTE_TYPE
        },
    ],
    KeySchema: [
        {
            AttributeName: "Season", //ATTRIBUTE_NAME_1
            KeyType: "HASH",
        },
        {
            AttributeName: "Episode", //ATTRIBUTE_NAME_2
            KeyType: "RANGE",
        },
    ],
    ProvisionedThroughput: {
        ReadCapacityUnits: 1,
        WriteCapacityUnits: 1,
    },
    TableName: "TABLE_NAME", //TABLE_NAME
    StreamSpecification: {
        StreamEnabled: false,
    },
};

// Instantiate an Amazon DynamoDB client object.
const ddb = new DynamoDBClient({ region: REGION });

exports.handler = async(event, context, callback) => {
    try {
        const data = await ddb.send(new CreateTableCommand(params));
        console.log("Table Created", data);
    } catch (err) {
        console.log("Error", err);
    }
};
```

This code example is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/lambda/lambda_create_function/src/mylamdbafunction.ts)\.