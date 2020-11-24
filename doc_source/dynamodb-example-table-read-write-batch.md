--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Reading and writing items in batch in DynamoDB<a name="dynamodb-example-table-read-write-batch"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to read and write batches of items in a DynamoDB table\.

## The scenario<a name="dynamodb-example-table-read-write-batch-scenario"></a>

In this example, you use a series of Node\.js modules to put a batch of items in a DynamoDB table and read a batch of items\. The code uses the SDK for JavaScript to perform batch read and write operations using these methods of the DynamoDB client class:
+ [BatchGetItemCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#batchGetItem-property)
+ [BatchWriteItemCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#batchWriteItem-property)

## Prerequisite tasks<a name="dynamodb-example-table-read-write-batch-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/dynamodb/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so with minor adjustments these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create a DynamoDB table whose items you can access\. For more information about creating a DynamoDB table, see [Creating and using tables in DynamoDB](dynamodb-examples-using-tables.md)\.

## Reading items in Batch<a name="dynamodb-example-table-read-write-batch-reading"></a>

Create a Node\.js module with the file name `ddb_batchgetitem.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. Create a JSON object containing the parameters needed to get a batch of items, which in this example includes the name of one or more tables from which to read, the values of keys to read in each table, and the projection expression that specifies the attributes to return\. Call the `BatchGetItemCommand` method of the DynamoDB service object\.

**Note**  
Replace *REGION* with your AWS Region, *TABLE\_NAME* with the name of the table, *KEY\_NAME* with the primary key of the table, *KEY\_VALUE* with the value of the primary key row containing the attribute value, and *ATTRIBUTE\_NAME* the name of the attribute column containing the attribute value\.

**Note**  
This the following code below batch retrieves items from a table with a primary key composed of only a partion key \- `KEY_NAME` \- and not of both a partion and sort key\. If the table has a primary key composed of a partition key and a sort key, you must also specify the sort key name and attribute for each item\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  DynamoDBClient,
  BatchGetItemCommand
} = require("@aws-sdk/client-dynamodb");

// Set the AWS Region
const REGION = "region"; //e.g. "us-east-1"

// Set the parameters
const params = {
  RequestItems: {
    TABLE_NAME: {
      Keys: [
        {
          KEY_NAME: { N: "KEY_VALUE" },
          KEY_NAME: { N: "KEY_VALUE" },
          KEY_NAME: { N: "KEY_VALUE" },
        },
      ],
      ProjectionExpression: "ATTRIBUTE_NAME",
    },
  },
};

// Create DynamoDB service object
const dbclient = new DynamoDBClient(REGION);

const run = async () => {
  try {
    const data = await dbclient.send(new BatchGetItemCommand(params));
    console.log("Success, items retrieved", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ddb_batchgetitem.ts // If you prefer JavaScript, enter 'node ddb_batchgetitem.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_batchgetitem.ts)\.

## Writing items in Batch<a name="dynamodb-example-table-read-write-batch-writing"></a>

Create a Node\.js module with the file name `ddb_batchwriteitem.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. Create a JSON object containing the parameters needed to get a batch of items, which in this example includes the table into which you want to write items, the keys you want to write for each item, and the attributes along with their values\. Call the `BatchWriteItemCommand` method of the DynamoDB service object\.

**Note**  
Replace *REGION* with your AWS Region, *TABLE\_NAME* with the name of the table, *KEYS* with the primary key of the item, *KEY\_VALUE* with the value of the primary key row containing the attribute value, and *ATTRIBUTE\_NAME* the name of the attribute column containing the attribute value\.

The following code example batch writes items to a table with a primary key composed of only a partion key \- `KEY_NAME` \- and not of both a partion and sort key\. If the table has a primary key composed of a partition key and a sort key, you must also specify the sort key name and attribute for each item\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  DynamoDBClient,
  BatchWriteItemCommand
} = require("@aws-sdk/client-dynamodb");

// Set the AWS Region
const REGION = "region"; //e.g. "us-east-1"

// Set the parameters
const params = {
  RequestItems: {
    TABLE_NAME: [
      {
        PutRequest: {
          Item: {
            KEY: { N: "KEY_VALUE" },
            ATTRIBUTE_1: { S: "ATTRIBUTE_1_VALUE" },
            ATTRIBUTE_2: { N: "ATTRIBUTE_2_VALUE" },
          },
        },
      },
      {
        PutRequest: {
          Item: {
            KEY: { N: "KEY_VALUE" },
            ATTRIBUTE_1: { S: "ATTRIBUTE_1_VALUE" },
            ATTRIBUTE_2: { N: "ATTRIBUTE_2_VALUE" },
          },
        },
      },
    ],
  },
};

// Create DynamoDB service object
const dbclient = new DynamoDBClient(REGION);

const run = async () => {
  try {
    const data = await dbclient.send(new BatchWriteItemCommand(params));
    console.log("Success, items inserted", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ddb_batchwriteitem.ts // If you prefer JavaScript, enter 'node ddb_batchwriteitem.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_batchwriteitem.ts)\.