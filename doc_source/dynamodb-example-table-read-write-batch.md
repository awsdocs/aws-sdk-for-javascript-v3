--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Reading and writing items in batch in DynamoDB<a name="dynamodb-example-table-read-write-batch"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to read and write batches of items in a DynamoDB table\.

## The scenario<a name="dynamodb-example-table-read-write-batch-scenario"></a>

In this example, you use a series of Node\.js modules to put a batch of items in a DynamoDB table and read a batch of items\. The code uses the SDK for JavaScript to perform batch read and write operations using these methods of the DynamoDB client class:
+ [BatchGetItemCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/batchgetitemcommand.html)
+ [BatchWriteItemCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/batchwriteitemcommand.html)

## Prerequisite tasks<a name="dynamodb-example-table-read-write-batch-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/dynamodb/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create a DynamoDB table whose items you can access\. For more information about creating a DynamoDB table, see [Creating and using tables in DynamoDB](dynamodb-examples-using-tables.md)\.

**Important**  
These examples use ECMAScript6 \(ES6\)\. This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.  
However, if you prefer to use CommonJS syntax, please refer to [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)

## Reading items in Batch<a name="dynamodb-example-table-read-write-batch-reading"></a>

Create a `libs` directory, and create a Node\.js module with the file name `ddbClient.js`\. Copy and paste the code below into it, which creates the DynamoDB client object\. Replace *REGION* with your AWS region\.

```
// Create service client module using ES6 syntax.
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon DynamoDB service client object.
const ddbClient = new DynamoDBClient({ region: REGION });
export { ddbClient };
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/libs/ddbClient.js)\.

Create a Node\.js module with the file name `ddb_batchgetitem.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. Create a JSON object containing the parameters needed to get a batch of items, which in this example includes the name of one or more tables from which to read, the values of keys to read in each table, and the projection expression that specifies the attributes to return\. Call the `BatchGetItemCommand` method of the DynamoDB service object\.

**Note**  
Replace *TABLE\_NAME* with the name of the table, *KEY\_NAME* with the primary key of the table, *KEY\_VALUE* with the value of the primary key row containing the attribute value, and *ATTRIBUTE\_NAME* the name of the attribute column containing the attribute value\.

**Note**  
This the following code below batch retrieves items from a table with a primary key composed of only a partition key \- `KEY_NAME` \- and not of both a partion and sort key\. If the table has a primary key composed of a partition key and a sort key, you must also specify the sort key name and attribute for each item\.

```
// Import required AWS SDK clients and commands for Node.js
import { BatchGetItemCommand } from "@aws-sdk/client-dynamodb";
import { ddbClient } from "./libs/ddbClient.js";

// Set the parameters
export const params = {
  RequestItems: {
    TABLE_NAME: {
      Keys: [
        {
          KEY_NAME_1: { N: "KEY_VALUE" },
          KEY_NAME_2: { N: "KEY_VALUE" },
          KEY_NAME_3: { N: "KEY_VALUE" },
        },
      ],
      ProjectionExpression: "ATTRIBUTE_NAME",
    },
  },
};

export const run = async () => {
  try {
    const data = await ddbClient.send(new BatchGetItemCommand(params));
    console.log("Success, items retrieved", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ddb_batchgetitem.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_batchgetitem.js)\.

## Writing items in Batch<a name="dynamodb-example-table-read-write-batch-writing"></a>

Create a `libs` directory, and create a Node\.js module with the file name `ddbClient.js`\. Copy and paste the code below into it, which creates the DynamoDB client object\. Replace *REGION* with your AWS region\.

```
// Create service client module using ES6 syntax.
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon DynamoDB service client object.
const ddbClient = new DynamoDBClient({ region: REGION });
export { ddbClient };
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/libs/ddbClient.js)\.

Create a Node\.js module with the file name `ddb_batchwriteitem.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. Create a JSON object containing the parameters needed to get a batch of items, which in this example includes the table into which you want to write items, the keys you want to write for each item, and the attributes along with their values\. Call the `BatchWriteItemCommand` method of the DynamoDB service object\.

**Note**  
Replace *TABLE\_NAME* with the name of the table, *KEYS* with the primary key of the item, *KEY\_VALUE* with the value of the primary key row containing the attribute value, and *ATTRIBUTE\_NAME* the name of the attribute column containing the attribute value\.

The following code example batch writes items to a table with a primary key composed of only a partition key \- `KEY_NAME` \- and not of both a partion and sort key\. If the table has a primary key composed of a partition key and a sort key, you must also specify the sort key name and attribute for each item\.

```
// Import required AWS SDK clients and commands for Node.js
import { BatchWriteItemCommand } from "@aws-sdk/client-dynamodb";
import { ddbClient } from "./libs/ddbClient.js";

// Set the parameters
export const params = {
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

export const run = async () => {
  try {
    const data = await ddbClient.send(new BatchWriteItemCommand(params));
    console.log("Success, items inserted", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ddb_batchwriteitem.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_batchwriteitem.js)\.