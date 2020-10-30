--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Reading and writing a single item in DynamoDB<a name="dynamodb-example-table-read-write"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to add an item in a DynamoDB table\.
+ How to retrieve, an item in a DynamoDB table\.
+ How to delete an item in a DynamoDB table\.

## The scenario<a name="dynamodb-example-table-read-write-scenario"></a>

In this example, you use a series of Node\.js modules to read and write one item in a DynamoDB table by using these methods of the `DynamoDB` client class:
+ [PutItemCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#putItem-property)
+ [GetItemCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#getItem-property)
+ [DeleteItemCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#deleteItem-property)

## Prerequisite tasks<a name="dynamodb-example-table-read-write-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/dynamodb/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so with minor adjustments these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create a DynamoDB table whose items you can access\. For more information about creating a DynamoDB table, see [Creating and using tables in DynamoDB](dynamodb-examples-using-tables.md)\.

## Writing an item<a name="dynamodb-example-table-read-write-writing-an-item"></a>

Create a Node\.js module with the file name `ddb_putitem.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. Create a JSON object containing the parameters needed to add an item, which in this example includes the name of the table and a map that defines the attributes to set and the values for each attribute\. Call the `PutItemCommand` method of the DynamoDB client service object\.

**Note**  
Replace *REGION* with your AWS Region, and *TABLE\_NAME* with the name of the table\.

**Note**  
The following code example writes to a table with a primary key composed of only a partion key \- `CUSTOMER_ID` \- and a sort key \- `CUSTOMER_NAME`\. If the table's primary key is composed of only a partition key, you only specify the partition key\. 

```
// Import required AWS SDK clients and commands for Node.js
const { DynamoDBClient, PutItemCommand } = require("@aws-sdk/client-dynamodb");

// Set the AWS Region
const REGION = "region"; //e.g. "us-east-1"

// Set the parameters
const params = {
  TableName: "TABLE_NAME",
  Item: {
    CUSTOMER_ID: { N: "001" },
    CUSTOMER_NAME: { S: "Richard Roe" },
  },
};

// Create DynamoDB service object
const dbclient = new DynamoDBClient(REGION);

const run = async () => {
  try {
    const data = await dbclient.send(new PutItemCommand(params));
    console.log(data);
  } catch (err) {
    console.error(err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ddb_putitem.ts // If you prefer JavaScript, enter 'node ddb_putitem.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_putitem.ts)\.

## Getting an item<a name="dynamodb-example-table-read-write-getting-an-item"></a>

Create a Node\.js module with the file name `ddb_getitem.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. To identify the item to get, you must provide the value of the primary key for that item in the table\. By default, the `GetItemCommand` method returns all the attribute values defined for the item\. To get only a subset of all possible attribute values, specify a projection expression\.

Create a JSON object containing the parameters needed to get an item, which in this example includes the name of the table, the name and value of the key for the item you're getting, and a projection expression that identifies the item attribute you want to retrieve\. Call the `GetItemCommand` method of the DynamoDB client service object\.

**Note**  
Replace *REGION* with your AWS Region, *TABLE\_NAME* with the name of the table, *KEY\_NAME* with the primary key of the table, *KEY\_NAME\_VALUE* with the value of the primary key row containing the attribute value, and *ATTRIBUTE\_NAME* the name of the attribute column containing the attribute value\.

The following code example retrieves an item from a table with a primary key composed of only a partion key \- `KEY_NAME` \- and not of both a partion and sort key\. If the table has a primary key composed of a partition key and a sort key, you must also specify the sort key name and attribute\.

```
// Import required AWS SDK clients and commands for Node.js
const { DynamoDBClient, GetItemCommand } = require("@aws-sdk/client-dynamodb");

// Set the AWS Region
const REGION = "region"; //e.g. "us-east-1"

// Set the parameters
const params = {
  TableName: "TABLE_NAME", //TABLE_NAME
  Key: {
    KEY_NAME: { N: "KEY_VALUE" },
  },
  ProjectionExpression: "ATTRIBUTE_NAME",
};

// Create DynamoDB service object
const dbclient = new DynamoDBClient(REGION);

const run = async () => {
  const data = await dbclient.send(new GetItemCommand(params));
  console.log("Success", data.Item);
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ddb_getitem.ts // If you prefer JavaScript, enter 'node ddb_getitem.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_getitem.ts)\.

## Deleting an item<a name="dynamodb-example-table-read-write-deleting-an-item"></a>

Create a Node\.js module with the file name `ddb_deleteitem.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. Create a JSON object containing the parameters needed to delete an item, which in this example includes the name of the table and both the key name and value for the item you're deleting\. Call the `DeleDteItemCommand` method of the DynamoDB client service object\.

**Note**  
Replace *REGION* with your AWS Region, and *TABLE\_NAME* with the name of the table\.

**Note**  
The following code example below deletes a item with a primary key composed of only a partion key \- `KEY_NAME` \- and not of both a partion and sort key\. If the table has a primary key composed of a partition key and a sort key, you must also specify the sort key name and attribute\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  DynamoDBClient,
  DeleteItemCommand
} = require("@aws-sdk/client-dynamodb");

// Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Set the parameters
var params = {
  TableName: 'TABLE_NAME',
  Key: {
    'KEY_NAME': {N: 'VALUE'}
  }
};

// Create DynamoDB service object
const dbclient = new DynamoDBClient(REGION);

const run = async () => {
  try {
    const data = await dbclient.send(new DeleteItemCommand(params));
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
ts-node ddb_deleteitem.ts // If you prefer JavaScript, enter 'node ddb_deleteitem.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_deleteitem.ts)\.