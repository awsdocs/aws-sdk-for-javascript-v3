--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Creating and using tables in DynamoDB<a name="dynamodb-examples-using-tables"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create ad manage tables used to store and retrieve data from DynamoDB\.

## The scenario<a name="dynamodb-examples-using-tables-scenario"></a>

Similar to other database systems, DynamoDB stores data in tables\. A DynamoDB table is a collection of data that's organized into items that are analogous to rows\. To store or access data in DynamoDB, you create ad work with tables\.

In this example, you use a series of Node\.js modules to perform basic operations with a DynamoDB table\. The code uses the SDK for JavaScript to create ad work with tables by using these methods of the `DynamoDB` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#createTable-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#createTable-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#listTables-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#listTables-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#describeTable-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#describeTable-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#deleteTable-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#deleteTable-property)

## Prerequisite tasks<a name="dynamodb-examples-using-tables-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/dynamodb/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these example can also be run in JavaScript\.
+ Install SDK for JavaScript DynamoDB client\. For more information, see [What's new in Version 3](welcome.md#welcome_whats_new_v3)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Creating a table<a name="dynamodb-examples-using-tables-creating-a-table"></a>

Create a Node\.js module with the file name `ddb_createtable.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. Create a JSON object containing the parameters needed to create a table, which in this example includes the name and data type for each attribute, the key schema, the name of the table, and the units of throughput to provision\. Call the `CreateTableCommand` method of the DynamoDB service object\.

**Note**  
Replace *REGION* with your AWS Region, *TABLE\_NAME*with the name of the table, *ATTRIBUTE\_NAME\_1* with the name of the partition key, *ATTRIBUTE\_NAME\_2* with the name of the sort key \(optionally\), and *ATTRIBUTE\_TYPE* with the type of the attribute \(for example, N \[for a number\], S \[for a string\] etc\.\)\.

**Note**  
The primary key for the table is composed of the following attributes:  
`Season`
`Episode`

```
// Import required AWS SDK clients and commands for Node.js
const {
  DynamoDBClient,
  CreateTableCommand
} = require("@aws-sdk/client-dynamodb");

// Set the AWS Region
const REGION = "region"; //e.g. "us-east-1"

// Set the parameters
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

// Create DynamoDB service object
const dbclient = new DynamoDBClient(REGION);

const run = async () => {
  try {
    const data = await dbclient.send(new CreateTableCommand(params));
    console.log("Table Created", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ddb_createtable.ts // If you prefer JavaScript, enter 'node ddb_createtable.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_createtable.ts)\.

## Listing your tables<a name="dynamodb-examples-using-tables-listing-tables"></a>

Create a Node\.js module with the file name `ddb_listtables.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. Create a JSON object containing the parameters needed to list your tables, which in this example limits the number of tables listed to 10\. Call the `ListTablesCommand` method of the DynamoDB service object\.

**Note**  
Replace *REGION* with your AWS Region\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  DynamoDBClient,
  ListTablesCommand,
} = require("@aws-sdk/client-dynamodb");

// Set the AWS Region
const REGION = "region"; //e.g. "us-east-1"

// Create DynamoDB service object
const dbclient = new DynamoDBClient(REGION);

const run = async () => {
  try {
    const data = await dbclient.send(new ListTablesCommand({}));
    console.log(data.TableNames.join("\n"));
  } catch (err) {
    console.error(err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ddb_listtables.ts // If you prefer JavaScript, enter 'node ddb_listtables.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_listtables.ts)\.

## Describing a table<a name="dynamodb-examples-using-tables-describing-a-table"></a>

Create a Node\.js module with the file name `ddb_describetable.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. Create a JSON object containing the parameters needed to describe a `DescribeTableCommand` method of the DynamoDB service object\.

**Note**  
Replace *REGION* with your AWS Region, and *TABLE\_NAME* with the name of the table\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  DynamoDBClient,
  DescribeTableCommand,
} = require("@aws-sdk/client-dynamodb");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = { TableName: "TABLE_NAME" }; //TABLE_NAME

// Create DynamoDB service object
const dbclient = new DynamoDBClient(REGION);

const run = async () => {
  try {
    const data = await dbclient.send(new DescribeTableCommand(params));
    console.log("Success", data.Table.KeySchema);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ddb_describetable.ts // If you prefer JavaScript, enter 'node ddb_describetable.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_describetable.ts)\.

## Deleting a table<a name="dynamodb-examples-using-tables-deleting-a-table"></a>

Create a Node\.js module with the file name `ddb_deletetable.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. Create a JSON object containing the parameters needed to delete a table, which in this example includes the name of the table provided as a command\-line parameter\. Call the `DeleteTableCommand` method of the DynamoDB service object\.

**Note**  
Replace *REGION* with your AWS Region\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  DynamoDBClient,
  DeleteTableCommand
} = require("@aws-sdk/client-dynamodb");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
const params = {
  TableName: "TABLE_NAME",
};

// Create DynamoDB service object
const dbclient = new DynamoDBClient(REGION);

const run = async () => {
  try {
    const data = await dbclient.send(new DeleteTableCommand(params));
    console.log("Success, table deleted", data);
  } catch (err) {
    if (err && err.code === "ResourceNotFoundException") {
      console.log("Error: Table not found");
    } else if (err && err.code === "ResourceInUseException") {
      console.log("Error: Table in use");
    }
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ddb_deletetable.ts // If you prefer JavaScript, enter 'node ddb_deletetable.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_deletetable.ts)\.