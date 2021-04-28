--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Create a DynamoDB table<a name="s3-crossservices-adddata-create-table"></a>

This example demonstrates creating a table using the AWS SDK for JavaScript, and updating the table with the required attributes\. Alternatively you can create it through the AWS Management Console\. For more information, see [Step 1: Create a table in the Amazon DynamoDB Developer Guide](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/getting-started-step-1.html)\.

To create the table, in `DynamoDBAppHelperFiles`, create a Node\.js module with the file name `create_table.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\.

Create an object for the parameters for creating the table, replacing *REGION* with the AWS Region and *TABLE\_NAME* with a name of the table\. The primary key is `Id`\. Then create a `DynamoDB` client service object\.

To create the table, enter the following at the command prompt\.

```
ts-node create_table.ts
```

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  DynamoDBClient,
  CreateTableCommand,
  PutItemCommand,
} = require("@aws-sdk/client-dynamodb");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const tableParams = {
  AttributeDefinitions: [
    {
      AttributeName: "Id", //ATTRIBUTE_NAME_1
      AttributeType: "N", //ATTRIBUTE_TYPE
    },
  ],
  KeySchema: [
    {
      AttributeName: "Id", //ATTRIBUTE_NAME_1
      KeyType: "HASH",
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

// Create DynamoDB service object
const dbclient = new DynamoDBClient({ region: REGION });

const run = async () => {
  try {
    const data = await dbclient.send(new CreateTableCommand(tableParams));
    console.log("Table created.", data.TableDescription.TableName);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/submit-data-app/src/dynamoAppHelperFiles/create-table.ts)\.

To update the table, create a Node\.js module with the file name `create_table.ts`\.

In `DynamoDBAppHelperFiles`, create an object for the parameters for creating the identity pool, replacing *REGION* with the AWS Region and *TABLE\_NAME* with a name of the table\.

The primary key is `Id`\. The other items are `Title`, `Name`, and `Body`\.

Create a `DynamoDB` client service object\.

To update the table, enter the following at the command prompt\.

```
ts-node update_table.ts
```

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import required AWS SDK clients and commands for Node.js
const { DynamoDBClient, PutItemCommand } = require("@aws-sdk/client-dynamodb");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  TableName: "TABLE_NAME",
  Item: {
    Id: { N: "1" },
    Title: { S: "aTitle" },
    Name: { S: "aName" },
    Body: { S: "aBody" },
  },
};

// Create DynamoDB service object
const dbclient = new DynamoDBClient({ region: REGION });

const run = async () => {
  try {
    const data = await dbclient.send(new PutItemCommand(params));
    console.log("success");
    console.log(data);
  } catch (err) {
    console.error(err);
  }
};
run();
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/submit-data-app/src/dynamoAppHelperFiles/update-table.ts)\.