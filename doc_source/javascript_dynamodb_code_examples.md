--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# DynamoDB examples using SDK for JavaScript \(v3\)<a name="javascript_dynamodb_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for JavaScript \(v3\) with DynamoDB\.

*Actions* are code excerpts that show you how to call individual service functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple functions within the same service\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#actions)
+ [Scenarios](#scenarios)

## Actions<a name="actions"></a>

### Create a table<a name="dynamodb_CreateTable_javascript_topic"></a>

The following code example shows how to create a DynamoDB table\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/dynamodb#code-examples)\. 
Create the client\.  

```
// Create service client module using ES6 syntax.
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon DynamoDB service client object.
const ddbClient = new DynamoDBClient({ region: REGION });
export { ddbClient };
```
Create the table\.  

```
// Import required AWS SDK clients and commands for Node.js
import { CreateTableCommand } from "@aws-sdk/client-dynamodb";
import { ddbClient } from "./libs/ddbClient.js";

// Set the parameters
export const params = {
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
  TableName: "TEST_TABLE", //TABLE_NAME
  StreamSpecification: {
    StreamEnabled: false,
  },
};

export const run = async () => {
  try {
    const data = await ddbClient.send(new CreateTableCommand(params));
    console.log("Table Created", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/dynamodb-examples-using-tables.html#dynamodb-examples-using-tables-creating-a-table)\. 
+  For API details, see [CreateTable](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/createtablecommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript \(v2\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/dynamodb#code-examples)\. 
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the DynamoDB service object
var ddb = new AWS.DynamoDB({apiVersion: '2012-08-10'});

var params = {
  AttributeDefinitions: [
    {
      AttributeName: 'CUSTOMER_ID',
      AttributeType: 'N'
    },
    {
      AttributeName: 'CUSTOMER_NAME',
      AttributeType: 'S'
    }
  ],
  KeySchema: [
    {
      AttributeName: 'CUSTOMER_ID',
      KeyType: 'HASH'
    },
    {
      AttributeName: 'CUSTOMER_NAME',
      KeyType: 'RANGE'
    }
  ],
  ProvisionedThroughput: {
    ReadCapacityUnits: 1,
    WriteCapacityUnits: 1
  },
  TableName: 'CUSTOMER_LIST',
  StreamSpecification: {
    StreamEnabled: false
  }
};

// Call DynamoDB to create the table
ddb.createTable(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Table Created", data);
  }
});
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/dynamodb-examples-using-tables.html#dynamodb-examples-using-tables-creating-a-table)\. 
+  For API details, see [CreateTable](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/dynamodb-2012-08-10/CreateTable) in *AWS SDK for JavaScript API Reference*\. 

### Delete a table<a name="dynamodb_DeleteTable_javascript_topic"></a>

The following code example shows how to delete a DynamoDB table\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/dynamodb#code-examples)\. 
Create the client\.  

```
// Create the DynamoDB service client module using ES6 syntax.
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
import { DEFAULT_REGION } from "../../../../libs/utils/util-aws-sdk.js";
// Create an Amazon DynamoDB service client object.
export const ddbClient = new DynamoDBClient({ region: DEFAULT_REGION });
```
Delete the table\.  

```
// Import required AWS SDK clients and commands for Node.js
import { DeleteTableCommand } from "@aws-sdk/client-dynamodb";
import { ddbClient } from "./libs/ddbClient.js";

// Set the parameters
export const params = {
  TableName: "CUSTOMER_LIST_NEW",
};

export const run = async () => {
  try {
    const data = await ddbClient.send(new DeleteTableCommand(params));
    console.log("Success, table deleted", data);
    return data;
  } catch (err) {
      console.log("Error", err);
    }
};
run();
```
+  For API details, see [DeleteTable](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/deletetablecommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript \(v2\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/dynamodb#code-examples)\. 
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the DynamoDB service object
var ddb = new AWS.DynamoDB({apiVersion: '2012-08-10'});

var params = {
  TableName: process.argv[2]
};

// Call DynamoDB to delete the specified table
ddb.deleteTable(params, function(err, data) {
  if (err && err.code === 'ResourceNotFoundException') {
    console.log("Error: Table not found");
  } else if (err && err.code === 'ResourceInUseException') {
    console.log("Error: Table in use");
  } else {
    console.log("Success", data);
  }
});
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/dynamodb-examples-using-tables.html#dynamodb-examples-using-tables-deleting-a-table)\. 
+  For API details, see [DeleteTable](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/dynamodb-2012-08-10/DeleteTable) in *AWS SDK for JavaScript API Reference*\. 

### Delete an item from a table<a name="dynamodb_DeleteItem_javascript_topic"></a>

The following code example shows how to delete an item from a DynamoDB table\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/dynamodb#code-examples)\. 
Create the client\.  

```
// Create the DynamoDB service client module using ES6 syntax.
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
import { DEFAULT_REGION } from "../../../../libs/utils/util-aws-sdk.js";
// Create an Amazon DynamoDB service client object.
export const ddbClient = new DynamoDBClient({ region: DEFAULT_REGION });
```
Create the document client\.  

```
// Create a service client module using ES6 syntax.
import { DynamoDBDocumentClient } from "@aws-sdk/lib-dynamodb";
import { ddbClient } from "./ddbClient.js";

const marshallOptions = {
  // Whether to automatically convert empty strings, blobs, and sets to `null`.
  convertEmptyValues: false, // false, by default.
  // Whether to remove undefined values while marshalling.
  removeUndefinedValues: true, // false, by default.
  // Whether to convert typeof object to map attribute.
  convertClassInstanceToMap: false, // false, by default.
};

const unmarshallOptions = {
  // Whether to return numbers as a string instead of converting them to native JavaScript numbers.
  wrapNumbers: false, // false, by default.
};

// Create the DynamoDB document client.
const ddbDocClient = DynamoDBDocumentClient.from(ddbClient, {
  marshallOptions,
  unmarshallOptions,
});

export { ddbDocClient };
```
Delete an item from a table using the DynamoDB document client\.  

```
import { DeleteCommand } from "@aws-sdk/lib-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient.js";

// Set the parameters.
export const params = {
  TableName: "TABLE_NAME",
  Key: {
    primaryKey: "VALUE_1",
    sortKey: "VALUE_2",
  },
};

export const deleteItem = async () => {
  try {
    await ddbDocClient.send(new DeleteCommand(params));
    console.log("Success - item deleted");
  } catch (err) {
    console.log("Error", err);
  }
};
deleteItem();
```
Delete an item from a table using PartiQL\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { ExecuteStatementCommand } from "@aws-sdk/client-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient.js";

const tableName = process.argv[2];
const movieYear1 = process.argv[3];
const movieTitle1 = process.argv[4];

export const run = async (tableName, movieYear1, movieTitle1) => {
  const params = {
    Statement: "DELETE FROM " + tableName + " where title=? and year=?",
    Parameters: [{ S: movieTitle1 }, { N: movieYear1 }],
  };
  try {
    await ddbDocClient.send(new ExecuteStatementCommand(params));
    console.log("Success. Item deleted.");
    return "Run successfully"; // For unit tests.
  } catch (err) {
    console.error(err);
  }
};
run(tableName, movieYear1, movieTitle1);
```
Delete an item from a table by batch using PartiQL\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { BatchExecuteStatementCommand } from "@aws-sdk/client-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient.js";

const tableName = process.argv[2];
const movieYear1 = process.argv[3];
const movieTitle1 = process.argv[4];
const movieYear2 = process.argv[5];
const movieTitle2 = process.argv[6];

export const run = async (
  tableName,
  movieYear1,
  movieTitle1,
  movieYear2,
  movieTitle2
) => {
  try {
    const params = {
      Statements: [
        {
          Statement: "DELETE FROM " + tableName + "  where year=? and title=?",
          Parameters: [{ N: movieYear1 }, { S: movieTitle1 }],
        },
        {
          Statement: "DELETE FROM " + tableName + "  where year=? and title=?",
          Parameters: [{ N: movieYear2 }, { S: movieTitle2 }],
        },
      ],
    };
    const data = await ddbDocClient.send(
      new BatchExecuteStatementCommand(params)
    );
    console.log("Success. Items deleted.", data);
    return "Run successfully"; // For unit tests.
  } catch (err) {
    console.error(err);
  }
};
run(tableName, movieYear1, movieTitle1, movieYear2, movieTitle2);
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/dynamodb-example-table-read-write.html#dynamodb-example-table-read-write-deleting-an-item)\. 
+  For API details, see [DeleteItem](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/deleteitemcommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript \(v2\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/dynamodb#code-examples)\. 
Delete an item from a table\.  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the DynamoDB service object
var ddb = new AWS.DynamoDB({apiVersion: '2012-08-10'});

var params = {
  TableName: 'TABLE',
  Key: {
    'KEY_NAME': {N: 'VALUE'}
  }
};

// Call DynamoDB to delete the item from the table
ddb.deleteItem(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```
Delete an item from a table using the DynamoDB document client\.  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create DynamoDB document client
var docClient = new AWS.DynamoDB.DocumentClient({apiVersion: '2012-08-10'});

var params = {
  Key: {
    'HASH_KEY': VALUE
  },
  TableName: 'TABLE'
};

docClient.delete(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/dynamodb-example-table-read-write.html#dynamodb-example-table-read-write-deleting-an-item)\. 
+  For API details, see [DeleteItem](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/dynamodb-2012-08-10/DeleteItem) in *AWS SDK for JavaScript API Reference*\. 

### Get a batch of items<a name="dynamodb_BatchGetItem_javascript_topic"></a>

The following code example shows how to get a batch of DynamoDB items\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/dynamodb#code-examples)\. 
Create the client\.  

```
// Create service client module using ES6 syntax.
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon DynamoDB service client object.
const ddbClient = new DynamoDBClient({ region: REGION });
export { ddbClient };
```
Get the items\.  

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
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/dynamodb-example-table-read-write-batch.html#dynamodb-example-table-read-write-batch-reading)\. 
+  For API details, see [BatchGetItem](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/batchgetitemcommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript \(v2\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/dynamodb#code-examples)\. 
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create DynamoDB service object
var ddb = new AWS.DynamoDB({apiVersion: '2012-08-10'});

var params = {
  RequestItems: {
    'TABLE_NAME': {
      Keys: [
        {'KEY_NAME': {N: 'KEY_VALUE_1'}},
        {'KEY_NAME': {N: 'KEY_VALUE_2'}},
        {'KEY_NAME': {N: 'KEY_VALUE_3'}}
      ],
      ProjectionExpression: 'KEY_NAME, ATTRIBUTE'
    }
  }
};

ddb.batchGetItem(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    data.Responses.TABLE_NAME.forEach(function(element, index, array) {
      console.log(element);
    });
  }
});
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/dynamodb-example-table-read-write-batch.html#dynamodb-example-table-read-write-batch-reading)\. 
+  For API details, see [BatchGetItem](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/dynamodb-2012-08-10/BatchGetItem) in *AWS SDK for JavaScript API Reference*\. 

### Get an item from a table<a name="dynamodb_GetItem_javascript_topic"></a>

The following code example shows how to get an item from a DynamoDB table\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/dynamodb#code-examples)\. 
Create the client\.  

```
// Create the DynamoDB service client module using ES6 syntax.
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
import { DEFAULT_REGION } from "../../../../libs/utils/util-aws-sdk.js";
// Create an Amazon DynamoDB service client object.
export const ddbClient = new DynamoDBClient({ region: DEFAULT_REGION });
```
Create the DynamoDB document client\.  

```
// Create a service client module using ES6 syntax.
import { DynamoDBDocumentClient } from "@aws-sdk/lib-dynamodb";
import { ddbClient } from "./ddbClient.js";

const marshallOptions = {
  // Whether to automatically convert empty strings, blobs, and sets to `null`.
  convertEmptyValues: false, // false, by default.
  // Whether to remove undefined values while marshalling.
  removeUndefinedValues: true, // false, by default.
  // Whether to convert typeof object to map attribute.
  convertClassInstanceToMap: false, // false, by default.
};

const unmarshallOptions = {
  // Whether to return numbers as a string instead of converting them to native JavaScript numbers.
  wrapNumbers: false, // false, by default.
};

// Create the DynamoDB document client.
const ddbDocClient = DynamoDBDocumentClient.from(ddbClient, {
  marshallOptions,
  unmarshallOptions,
});

export { ddbDocClient };
```
Get an item from a table\.  

```
import { GetCommand } from "@aws-sdk/lib-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient.js";

// Set the parameters.
export const params = {
  TableName: "TABLE_NAME",
  Key: {
    primaryKey: "VALUE_1",
    sortKey: "VALUE_2",
  },
};

export const getItem = async () => {
  try {
    const data = await ddbDocClient.send(new GetCommand(params));
    console.log("Success :", data.Item);
  } catch (err) {
    console.log("Error", err);
  }
};
getItem();
```
Get an item from a table using PartiQL\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { ExecuteStatementCommand } from "@aws-sdk/client-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient";

const tableName = process.argv[2];
const movieTitle1 = process.argv[3];

export const run = async (tableName, movieTitle1) => {
  const params = {
    Statement: "SELECT * FROM " + tableName + " where title=?",
    Parameters: [{ S: movieTitle1 }],
  };
  try {
    const data = await ddbDocClient.send(new ExecuteStatementCommand(params));
    for (let i = 0; i < data.Items.length; i++) {
      console.log(
        "Success. The query return the following data. Item " + i,
        data.Items[i].year,
        data.Items[i].title,
        data.Items[i].info
      );
    }
    return "Run successfully"; // For unit tests.
  } catch (err) {
    console.error(err);
  }
};
run(tableName, movieTitle1);
```
Get items by batch from a table using PartiQL\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { BatchExecuteStatementCommand } from "@aws-sdk/client-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient.js";

const tableName = process.argv[2];
const movieYear1 = process.argv[3];
const movieTitle1 = process.argv[4];
const movieYear2 = process.argv[5];
const movieTitle2 = process.argv[6];

export const run = async (
  tableName,
  movieYear1,
  movieTitle1,
  movieYear2,
  movieTitle2
) => {
  const params = {
    Statements: [
      {
        Statement: "SELECT * FROM " + tableName + " where title=? and year=?",
        Parameters: [{ S: movieTitle1 }, { N: movieYear1 }],
      },
      {
        Statement: "SELECT * FROM " + tableName + " where title=? and year=?",
        Parameters: [{ S: movieTitle2 }, { N: movieYear2 }],
      },
    ],
  };
  try {
    const data = await ddbDocClient.send(
      new BatchExecuteStatementCommand(params)
    );
    for (let i = 0; i < data.Responses.length; i++) {
      console.log(data.Responses[i].Item.year);
      console.log(data.Responses[i].Item.title);
    }
    return "Run successfully"; // For unit tests.
  } catch (err) {
    console.error(err);
  }
};
run(tableName, movieYear1, movieTitle1, movieYear2, movieTitle2);
```
+  For API details, see [GetItem](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/getitemcommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript \(v2\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/dynamodb#code-examples)\. 
Get an item from a table\.  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the DynamoDB service object
var ddb = new AWS.DynamoDB({apiVersion: '2012-08-10'});

var params = {
  TableName: 'TABLE',
  Key: {
    'KEY_NAME': {N: '001'}
  },
  ProjectionExpression: 'ATTRIBUTE_NAME'
};

// Call DynamoDB to read the item from the table
ddb.getItem(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.Item);
  }
});
```
Get an item from a table using the DynamoDB document client\.  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create DynamoDB document client
var docClient = new AWS.DynamoDB.DocumentClient({apiVersion: '2012-08-10'});

var params = {
 TableName: 'EPISODES_TABLE',
 Key: {'KEY_NAME': VALUE}
};

docClient.get(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.Item);
  }
});
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/dynamodb-example-dynamodb-utilities.html#dynamodb-example-document-client-get)\. 
+  For API details, see [GetItem](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/dynamodb-2012-08-10/GetItem) in *AWS SDK for JavaScript API Reference*\. 

### Get information about a table<a name="dynamodb_DescribeTable_javascript_topic"></a>

The following code example shows how to get information about a DynamoDB table\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/dynamodb#code-examples)\. 
Create the client\.  

```
// Create service client module using ES6 syntax.
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon DynamoDB service client object.
const ddbClient = new DynamoDBClient({ region: REGION });
export { ddbClient };
```
Describe the table\.  

```
// Import required AWS SDK clients and commands for Node.js
import { DescribeTableCommand } from "@aws-sdk/client-dynamodb";
import { ddbClient } from "./libs/ddbClient.js";

// Set the parameters
export const params = { TableName: "TABLE_NAME" }; //TABLE_NAME

export const run = async () => {
  try {
    const data = await ddbClient.send(new DescribeTableCommand(params));
    console.log("Success", data);
    // console.log("Success", data.Table.KeySchema);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/dynamodb-examples-using-tables.html#dynamodb-examples-using-tables-describing-a-table)\. 
+  For API details, see [DescribeTable](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/describetablecommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript \(v2\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/dynamodb#code-examples)\. 
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the DynamoDB service object
var ddb = new AWS.DynamoDB({apiVersion: '2012-08-10'});

var params = {
  TableName: process.argv[2]
};

// Call DynamoDB to retrieve the selected table descriptions
ddb.describeTable(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.Table.KeySchema);
  }
});
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/dynamodb-examples-using-tables.html#dynamodb-examples-using-tables-describing-a-table)\. 
+  For API details, see [DescribeTable](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/dynamodb-2012-08-10/DescribeTable) in *AWS SDK for JavaScript API Reference*\. 

### List tables<a name="dynamodb_ListTables_javascript_topic"></a>

The following code example shows how to list DynamoDB tables\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/dynamodb#code-examples)\. 
Create the client\.  

```
// Create service client module using ES6 syntax.
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon DynamoDB service client object.
const ddbClient = new DynamoDBClient({ region: REGION });
export { ddbClient };
```
List the tables\.  

```
// Import required AWS SDK clients and commands for Node.js
import { ListTablesCommand } from "@aws-sdk/client-dynamodb";
import { ddbClient } from "./libs/ddbClient.js";

export const run = async () => {
  try {
    const data = await ddbClient.send(new ListTablesCommand({}));
    console.log(data);
    // console.log(data.TableNames.join("\n"));
    return data;
  } catch (err) {
    console.error(err);
  }
};
run();
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/dynamodb-examples-using-tables.html#dynamodb-examples-using-tables-listing-tables)\. 
+  For API details, see [ListTables](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/listtablescommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript \(v2\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/dynamodb#code-examples)\. 
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the DynamoDB service object
var ddb = new AWS.DynamoDB({apiVersion: '2012-08-10'});

// Call DynamoDB to retrieve the list of tables
ddb.listTables({Limit: 10}, function(err, data) {
  if (err) {
    console.log("Error", err.code);
  } else {
    console.log("Table names are ", data.TableNames);
  }
});
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/dynamodb-examples-using-tables.html#dynamodb-examples-using-tables-listing-tables)\. 
+  For API details, see [ListTables](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/dynamodb-2012-08-10/ListTables) in *AWS SDK for JavaScript API Reference*\. 

### Put an item in a table<a name="dynamodb_PutItem_javascript_topic"></a>

The following code example shows how to put an item in a DynamoDB table\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/dynamodb#code-examples)\. 
Create the client\.  

```
// Create the DynamoDB service client module using ES6 syntax.
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
import { DEFAULT_REGION } from "../../../../libs/utils/util-aws-sdk.js";
// Create an Amazon DynamoDB service client object.
export const ddbClient = new DynamoDBClient({ region: DEFAULT_REGION });
```
Create the DynamoDB document client\.  

```
// Create a service client module using ES6 syntax.
import { DynamoDBDocumentClient } from "@aws-sdk/lib-dynamodb";
import { ddbClient } from "./ddbClient.js";

const marshallOptions = {
  // Whether to automatically convert empty strings, blobs, and sets to `null`.
  convertEmptyValues: false, // false, by default.
  // Whether to remove undefined values while marshalling.
  removeUndefinedValues: true, // false, by default.
  // Whether to convert typeof object to map attribute.
  convertClassInstanceToMap: false, // false, by default.
};

const unmarshallOptions = {
  // Whether to return numbers as a string instead of converting them to native JavaScript numbers.
  wrapNumbers: false, // false, by default.
};

// Create the DynamoDB document client.
const ddbDocClient = DynamoDBDocumentClient.from(ddbClient, {
  marshallOptions,
  unmarshallOptions,
});

export { ddbDocClient };
```
Put an item in a table using the DynamoDB document client\.  

```
import { PutCommand } from "@aws-sdk/lib-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient.js";

export const putItem = async () => {
  // Set the parameters.
  export const params = {
    TableName: "TABLE_NAME",
    Item: {
      primaryKey: "VALUE_1",
      sortKey: "VALUE_2",
    },
  };
  try {
    const data = await ddbDocClient.send(new PutCommand(params));
    console.log("Success - item added or updated", data);
  } catch (err) {
    console.log("Error", err.stack);
  }
};
putItem();
```
Put an item in a table using PartiQL\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { ExecuteStatementCommand } from "@aws-sdk/client-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient.js";

const tableName = process.argv[2];
const movieYear1 = process.argv[3];
const movieTitle1 = process.argv[4];

export const run = async (tableName, movieTitle1, movieYear1) => {
  const params = {
    Statement: "INSERT INTO " + tableName + "  value  {'title':?, 'year':?}",
    Parameters: [{ S: movieTitle1 }, { N: movieYear1 }],
  };
  try {
    await ddbDocClient.send(new ExecuteStatementCommand(params));
    console.log("Success. Item added.");
    return "Run successfully"; // For unit tests.
  } catch (err) {
    console.error(err);
  }
};
run(tableName, movieTitle1, movieYear1);
```
Put items by batch in a table using PartiQL\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { BatchExecuteStatementCommand } from "@aws-sdk/client-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient.js";

const tableName = process.argv[2];
const movieYear1 = process.argv[3];
const movieTitle1 = process.argv[4];
const movieYear2 = process.argv[5];
const movieTitle2 = process.argv[6];

export const run = async (
  tableName,
  movieYear1,
  movieTitle1,
  movieYear2,
  movieTitle2
) => {
  const params = {
    Statements: [
      {
        Statement:
          "INSERT INTO " + tableName + "  value  {'title':?, 'year':?}",
        Parameters: [{ S: movieTitle1 }, { N: movieYear1 }],
      },
      {
        Statement:
          "INSERT INTO " + tableName + "  value  {'title':?, 'year':?}",
        Parameters: [{ S: movieTitle2 }, { N: movieYear2 }],
      },
    ],
  };
  try {
    await ddbDocClient.send(
      new BatchExecuteStatementCommand(params)
    );
    console.log("Success. Items added.");
    return "Run successfully"; // For unit tests.
  } catch (err) {
    console.error(err);
  }
};
run(tableName, movieYear1, movieTitle1, movieYear2, movieTitle2);
```
+  For API details, see [PutItem](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/putitemcommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript \(v2\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/dynamodb#code-examples)\. 
Put an item in a table\.  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create the DynamoDB service object
var ddb = new AWS.DynamoDB({apiVersion: '2012-08-10'});

var params = {
  TableName: 'CUSTOMER_LIST',
  Item: {
    'CUSTOMER_ID' : {N: '001'},
    'CUSTOMER_NAME' : {S: 'Richard Roe'}
  }
};

// Call DynamoDB to add the item to the table
ddb.putItem(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```
Put an item in a table using the DynamoDB document client\.  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create DynamoDB document client
var docClient = new AWS.DynamoDB.DocumentClient({apiVersion: '2012-08-10'});

var params = {
  TableName: 'TABLE',
  Item: {
    'HASHKEY': VALUE,
    'ATTRIBUTE_1': 'STRING_VALUE',
    'ATTRIBUTE_2': VALUE_2
  }
};

docClient.put(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/dynamodb-example-table-read-write.html#dynamodb-example-table-read-write-writing-an-item)\. 
+  For API details, see [PutItem](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/dynamodb-2012-08-10/PutItem) in *AWS SDK for JavaScript API Reference*\. 

### Query a table<a name="dynamodb_Query_javascript_topic"></a>

The following code example shows how to query a DynamoDB table\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/dynamodb#code-examples)\. 
Create the client\.  

```
// Create service client module using ES6 syntax.
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon DynamoDB service client object.
const ddbClient = new DynamoDBClient({ region: REGION });
export { ddbClient };
```
Query the table\.  

```
// Import required AWS SDK clients and commands for Node.js
import { QueryCommand } from "@aws-sdk/client-dynamodb";
import { ddbClient } from "./libs/ddbClient.js";

// Set the parameters
export const params = {
  KeyConditionExpression: "Season = :s and Episode > :e",
  FilterExpression: "contains (Subtitle, :topic)",
  ExpressionAttributeValues: {
    ":s": { N: "1" },
    ":e": { N: "2" },
    ":topic": { S: "SubTitle" },
  },
  ProjectionExpression: "Episode, Title, Subtitle",
  TableName: "EPISODES_TABLE",
};

export const run = async () => {
  try {
    const data = await ddbClient.send(new QueryCommand(params));
    data.Items.forEach(function (element) {
      console.log(element.Title.S + " (" + element.Subtitle.S + ")");
    });
    return data;
  } catch (err) {
    console.error(err);
  }
};
run();
```
Create the client for the DynamoDB document client\.  

```
// Create service client module using ES6 syntax.
import { DynamoDBDocumentClient} from "@aws-sdk/lib-dynamodb";
import {ddbClient} from "./ddbClient";

const marshallOptions = {
    // Whether to automatically convert empty strings, blobs, and sets to `null`.
    convertEmptyValues: false, // false, by default.
    // Whether to remove undefined values while marshalling.
    removeUndefinedValues: false, // false, by default.
    // Whether to convert typeof object to map attribute.
    convertClassInstanceToMap: false, // false, by default.
};

const unmarshallOptions = {
    // Whether to return numbers as a string instead of converting them to native JavaScript numbers.
    wrapNumbers: false, // false, by default.
};

const translateConfig = { marshallOptions, unmarshallOptions };

// Create the DynamoDB Document client.
const ddbDocClient = DynamoDBDocumentClient.from(ddbClient, translateConfig);

export { ddbDocClient };
```
Query the table using the DynamoDB document client\.  

```
import { QueryCommand } from "@aws-sdk/lib-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient.js";

// Set the parameters.
export const params = {
  ExpressionAttributeNames: { "#r": "rank", "#y": "year" },
  ProjectionExpression: "#r, #y, title",
  TableName: "TABLE_NAME",
  ExpressionAttributeValues: {
    ":t": "MOVIE_NAME",
    ":y": "MOVIE_YEAR",
    ":r": "MOVIE_RANK",
  },
  KeyConditionExpression: "title = :t and #y = :y",
  FilterExpression: "info.#r = :r",
};

export const queryTable = async () => {
  try {
    const data = await ddbDocClient.send(new QueryCommand(params));
    for (let i = 0; i < data.Items.length; i++) {
      console.log(
        "Success. Items with rank of " +
          "MOVIE_RANK" +
          " include\n" +
          "Year = " +
          data.Items[i].year +
          " Title = " +
          data.Items[i].title
      );
    }
  } catch (err) {
    console.log("Error", err);
  }
};
queryTable();
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/dynamodb-example-query-scan.html#dynamodb-example-table-query-scan-querying)\. 
+  For API details, see [Query](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/querycommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript \(v2\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/dynamodb#code-examples)\. 
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

// Create DynamoDB document client
var docClient = new AWS.DynamoDB.DocumentClient({apiVersion: '2012-08-10'});

var params = {
  ExpressionAttributeValues: {
    ':s': 2,
    ':e': 9,
    ':topic': 'PHRASE'
   },
 KeyConditionExpression: 'Season = :s and Episode > :e',
 FilterExpression: 'contains (Subtitle, :topic)',
 TableName: 'EPISODES_TABLE'
};

docClient.query(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.Items);
  }
});
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/dynamodb-example-query-scan.html#dynamodb-example-table-query-scan-querying)\. 
+  For API details, see [Query](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/dynamodb-2012-08-10/Query) in *AWS SDK for JavaScript API Reference*\. 

### Run a PartiQL statement<a name="dynamodb_ExecuteStatement_javascript_topic"></a>

The following code example shows how to run a PartiQL statement on a DynamoDB table\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/dynamodb#code-examples)\. 
Create the client\.  

```
// Create the DynamoDB service client module using ES6 syntax.
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
// Set the AWS Region.
export const REGION = "eu-west-1"; // For example, "us-east-1".
// Create an Amazon DynamoDB service client object.
export const ddbClient = new DynamoDBClient({ region: REGION });
```
Create the DynamoDB document client\.  

```
// Create a service client module using ES6 syntax.
import { DynamoDBDocumentClient } from "@aws-sdk/lib-dynamodb";
import { ddbClient } from "./ddbClient.js";

const marshallOptions = {
  // Whether to automatically convert empty strings, blobs, and sets to `null`.
  convertEmptyValues: false, // false, by default.
  // Whether to remove undefined values while marshalling.
  removeUndefinedValues: false, // false, by default.
  // Whether to convert typeof object to map attribute.
  convertClassInstanceToMap: false, // false, by default.
};

const unmarshallOptions = {
  // Whether to return numbers as a string instead of converting them to native JavaScript numbers.
  wrapNumbers: false, // false, by default.
};

const translateConfig = { marshallOptions, unmarshallOptions };

// Create the DynamoDB document client.
const ddbDocClient = DynamoDBDocumentClient.from(ddbClient, translateConfig);

export { ddbDocClient };
```
Create an item using PartiQL\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { ExecuteStatementCommand } from "@aws-sdk/client-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient.js";

const tableName = process.argv[2];
const movieYear1 = process.argv[3];
const movieTitle1 = process.argv[4];

export const run = async (tableName, movieTitle1, movieYear1) => {
  const params = {
    Statement: "INSERT INTO " + tableName + "  value  {'title':?, 'year':?}",
    Parameters: [{ S: movieTitle1 }, { N: movieYear1 }],
  };
  try {
    await ddbDocClient.send(new ExecuteStatementCommand(params));
    console.log("Success. Item added.");
    return "Run successfully"; // For unit tests.
  } catch (err) {
    console.error(err);
  }
};
run(tableName, movieTitle1, movieYear1);
```
Get an item using PartiQL\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { ExecuteStatementCommand } from "@aws-sdk/client-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient";

const tableName = process.argv[2];
const movieTitle1 = process.argv[3];

export const run = async (tableName, movieTitle1) => {
  const params = {
    Statement: "SELECT * FROM " + tableName + " where title=?",
    Parameters: [{ S: movieTitle1 }],
  };
  try {
    const data = await ddbDocClient.send(new ExecuteStatementCommand(params));
    for (let i = 0; i < data.Items.length; i++) {
      console.log(
        "Success. The query return the following data. Item " + i,
        data.Items[i].year,
        data.Items[i].title,
        data.Items[i].info
      );
    }
    return "Run successfully"; // For unit tests.
  } catch (err) {
    console.error(err);
  }
};
run(tableName, movieTitle1);
```
Update an item using PartiQL\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { ExecuteStatementCommand } from "@aws-sdk/client-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient.js";

const tableName = process.argv[2];
const movieYear1 = process.argv[3];
const movieTitle1 = process.argv[4];
const producer1 = process.argv[5];

export const run = async (tableName, movieYear1, movieTitle1, producer1) => {
  const params = {
    Statement:
      "UPDATE " + tableName + "  SET Producer=? where title=? and year=?",
    Parameters: [{ S: producer1 }, { S: movieTitle1 }, { N: movieYear1 }],
  };
  try {
    await ddbDocClient.send(new ExecuteStatementCommand(params));
    console.log("Success. Item updated.");
    return "Run successfully"; // For unit tests.
  } catch (err) {
    console.error(err);
  }
};
run(tableName, movieYear1, movieTitle1, producer1);
```
Delete an item using PartiQL\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { ExecuteStatementCommand } from "@aws-sdk/client-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient.js";

const tableName = process.argv[2];
const movieYear1 = process.argv[3];
const movieTitle1 = process.argv[4];

export const run = async (tableName, movieYear1, movieTitle1) => {
  const params = {
    Statement: "DELETE FROM " + tableName + " where title=? and year=?",
    Parameters: [{ S: movieTitle1 }, { N: movieYear1 }],
  };
  try {
    await ddbDocClient.send(new ExecuteStatementCommand(params));
    console.log("Success. Item deleted.");
    return "Run successfully"; // For unit tests.
  } catch (err) {
    console.error(err);
  }
};
run(tableName, movieYear1, movieTitle1);
```
+  For API details, see [ExecuteStatement](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/executestatementcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Run batches of PartiQL statements<a name="dynamodb_BatchExecuteStatement_javascript_topic"></a>

The following code example shows how to run batches of PartiQL statements on a DynamoDB table\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/dynamodb#code-examples)\. 
Create the client\.  

```
// Create the DynamoDB service client module using ES6 syntax.
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
// Set the AWS Region.
export const REGION = "eu-west-1"; // For example, "us-east-1".
// Create an Amazon DynamoDB service client object.
export const ddbClient = new DynamoDBClient({ region: REGION });
```
Create the DynamoDB document client\.  

```
// Create a service client module using ES6 syntax.
import { DynamoDBDocumentClient } from "@aws-sdk/lib-dynamodb";
import { ddbClient } from "./ddbClient.js";

const marshallOptions = {
  // Whether to automatically convert empty strings, blobs, and sets to `null`.
  convertEmptyValues: false, // false, by default.
  // Whether to remove undefined values while marshalling.
  removeUndefinedValues: false, // false, by default.
  // Whether to convert typeof object to map attribute.
  convertClassInstanceToMap: false, // false, by default.
};

const unmarshallOptions = {
  // Whether to return numbers as a string instead of converting them to native JavaScript numbers.
  wrapNumbers: false, // false, by default.
};

const translateConfig = { marshallOptions, unmarshallOptions };

// Create the DynamoDB document client.
const ddbDocClient = DynamoDBDocumentClient.from(ddbClient, translateConfig);

export { ddbDocClient };
```
Create a batch of items using PartiQL\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { BatchExecuteStatementCommand } from "@aws-sdk/client-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient.js";

const tableName = process.argv[2];
const movieYear1 = process.argv[3];
const movieTitle1 = process.argv[4];
const movieYear2 = process.argv[5];
const movieTitle2 = process.argv[6];

export const run = async (
  tableName,
  movieYear1,
  movieTitle1,
  movieYear2,
  movieTitle2
) => {
  const params = {
    Statements: [
      {
        Statement:
          "INSERT INTO " + tableName + "  value  {'title':?, 'year':?}",
        Parameters: [{ S: movieTitle1 }, { N: movieYear1 }],
      },
      {
        Statement:
          "INSERT INTO " + tableName + "  value  {'title':?, 'year':?}",
        Parameters: [{ S: movieTitle2 }, { N: movieYear2 }],
      },
    ],
  };
  try {
    await ddbDocClient.send(
      new BatchExecuteStatementCommand(params)
    );
    console.log("Success. Items added.");
    return "Run successfully"; // For unit tests.
  } catch (err) {
    console.error(err);
  }
};
run(tableName, movieYear1, movieTitle1, movieYear2, movieTitle2);
```
Get a batch of items using PartiQL\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { BatchExecuteStatementCommand } from "@aws-sdk/client-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient.js";

const tableName = process.argv[2];
const movieYear1 = process.argv[3];
const movieTitle1 = process.argv[4];
const movieYear2 = process.argv[5];
const movieTitle2 = process.argv[6];

export const run = async (
  tableName,
  movieYear1,
  movieTitle1,
  movieYear2,
  movieTitle2
) => {
  const params = {
    Statements: [
      {
        Statement: "SELECT * FROM " + tableName + " where title=? and year=?",
        Parameters: [{ S: movieTitle1 }, { N: movieYear1 }],
      },
      {
        Statement: "SELECT * FROM " + tableName + " where title=? and year=?",
        Parameters: [{ S: movieTitle2 }, { N: movieYear2 }],
      },
    ],
  };
  try {
    const data = await ddbDocClient.send(
      new BatchExecuteStatementCommand(params)
    );
    for (let i = 0; i < data.Responses.length; i++) {
      console.log(data.Responses[i].Item.year);
      console.log(data.Responses[i].Item.title);
    }
    return "Run successfully"; // For unit tests.
  } catch (err) {
    console.error(err);
  }
};
run(tableName, movieYear1, movieTitle1, movieYear2, movieTitle2);
```
Update a batch of items using PartiQL\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { BatchExecuteStatementCommand } from "@aws-sdk/client-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient.js";

const tableName = process.argv[2];
const movieYear1 = process.argv[3];
const movieTitle1 = process.argv[4];
const movieYear2 = process.argv[6];
const movieTitle2 = process.argv[7];
const producer1 = process.argv[5];
const producer2 = process.argv[8];

export const run = async (
  tableName,
  movieYear1,
  movieTitle1,
  movieYear2,
  movieTitle2,
  producer1,
  producer2
) => {
  const params = {
    Statements: [
      {
        Statement:
          "UPDATE " + tableName + " SET producer=? where title=? and year=?",
        Parameters: [{ S: producer1 }, { S: movieTitle1 }, { N: movieYear1 }],
      },
      {
        Statement:
          "UPDATE " + tableName + " SET producer=? where title=? and year=?",
        Parameters: [{ S: producer2 }, { S: movieTitle2 }, { N: movieYear2 }],
      },
    ],
  };
  try {
    await ddbDocClient.send(
      new BatchExecuteStatementCommand(params)
    );
    console.log("Success. Items updated.");
    return "Run successfully"; // For unit tests.
  } catch (err) {
    console.error(err);
  }
};
run(
  tableName,
  movieYear1,
  movieTitle1,
  movieYear2,
  movieTitle2,
  producer1,
  producer2
);
```
Delete a batch of items using PartiQL\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { BatchExecuteStatementCommand } from "@aws-sdk/client-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient.js";

const tableName = process.argv[2];
const movieYear1 = process.argv[3];
const movieTitle1 = process.argv[4];
const movieYear2 = process.argv[5];
const movieTitle2 = process.argv[6];

export const run = async (
  tableName,
  movieYear1,
  movieTitle1,
  movieYear2,
  movieTitle2
) => {
  try {
    const params = {
      Statements: [
        {
          Statement: "DELETE FROM " + tableName + "  where year=? and title=?",
          Parameters: [{ N: movieYear1 }, { S: movieTitle1 }],
        },
        {
          Statement: "DELETE FROM " + tableName + "  where year=? and title=?",
          Parameters: [{ N: movieYear2 }, { S: movieTitle2 }],
        },
      ],
    };
    const data = await ddbDocClient.send(
      new BatchExecuteStatementCommand(params)
    );
    console.log("Success. Items deleted.", data);
    return "Run successfully"; // For unit tests.
  } catch (err) {
    console.error(err);
  }
};
run(tableName, movieYear1, movieTitle1, movieYear2, movieTitle2);
```
+  For API details, see [BatchExecuteStatement](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/batchexecutestatementcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Scan a table<a name="dynamodb_Scan_javascript_topic"></a>

The following code example shows how to scan a DynamoDB table\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/dynamodb#code-examples)\. 
Create the client\.  

```
// Create the DynamoDB service client module using ES6 syntax.
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
import { DEFAULT_REGION } from "../../../../libs/utils/util-aws-sdk.js";
// Create an Amazon DynamoDB service client object.
export const ddbClient = new DynamoDBClient({ region: DEFAULT_REGION });
```
Create the DynamoDB document client\.  

```
// Create a service client module using ES6 syntax.
import { DynamoDBDocumentClient } from "@aws-sdk/lib-dynamodb";
import { ddbClient } from "./ddbClient.js";

const marshallOptions = {
  // Whether to automatically convert empty strings, blobs, and sets to `null`.
  convertEmptyValues: false, // false, by default.
  // Whether to remove undefined values while marshalling.
  removeUndefinedValues: true, // false, by default.
  // Whether to convert typeof object to map attribute.
  convertClassInstanceToMap: false, // false, by default.
};

const unmarshallOptions = {
  // Whether to return numbers as a string instead of converting them to native JavaScript numbers.
  wrapNumbers: false, // false, by default.
};

// Create the DynamoDB document client.
const ddbDocClient = DynamoDBDocumentClient.from(ddbClient, {
  marshallOptions,
  unmarshallOptions,
});

export { ddbDocClient };
```
Scan a table using the DynamoDB document client\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { ddbDocClient } from "../libs/ddbDocClient.js";
import { ScanCommand } from "@aws-sdk/lib-dynamodb";
// Set the parameters.
export const params = {
  TableName: "TABLE_NAME",
  ProjectionExpression: "#r, #y, title",
  ExpressionAttributeNames: { "#r": "rank", "#y": "year" },
  FilterExpression: "title = :t and #y = :y and info.#r = :r",
  ExpressionAttributeValues: {
    ":r": "MOVIE_RANK",
    ":y": "MOVIE_YEAR",
    ":t": "MOVIE_NAME",
  },
};

export const scanTable = async () => {
  try {
    const data = await ddbDocClient.send(new ScanCommand(params));
    console.log("success", data.Items);
  } catch (err) {
    console.log("Error", err);
  }
};
scanTable();
```
+  For API details, see [Scan](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/scancommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript \(v2\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/dynamodb#code-examples)\. 
  

```
    // Load the AWS SDK for Node.js.
var AWS = require("aws-sdk");
// Set the AWS Region.
AWS.config.update({ region: "REGION" });

// Create DynamoDB service object.
var ddb = new AWS.DynamoDB({ apiVersion: "2012-08-10" });

const params = {
  // Specify which items in the results are returned.
  FilterExpression: "Subtitle = :topic AND Season = :s AND Episode = :e",
  // Define the expression attribute value, which are substitutes for the values you want to compare.
  ExpressionAttributeValues: {
    ":topic": {S: "SubTitle2"},
    ":s": {N: 1},
    ":e": {N: 2},
  },
  // Set the projection expression, which are the attributes that you want.
  ProjectionExpression: "Season, Episode, Title, Subtitle",
  TableName: "EPISODES_TABLE",
};

ddb.scan(params, function (err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
    data.Items.forEach(function (element, index, array) {
      console.log(
          "printing",
          element.Title.S + " (" + element.Subtitle.S + ")"
      );
    });
  }
});
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/dynamodb-example-query-scan.html#dynamodb-example-table-query-scan-scanning)\. 
+  For API details, see [Scan](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/dynamodb-2012-08-10/Scan) in *AWS SDK for JavaScript API Reference*\. 

### Update an item in a table<a name="dynamodb_UpdateItem_javascript_topic"></a>

The following code example shows how to update an item in a DynamoDB table\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/dynamodb#code-examples)\. 
Create the client\.  

```
// Create the DynamoDB service client module using ES6 syntax.
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
import { DEFAULT_REGION } from "../../../../libs/utils/util-aws-sdk.js";
// Create an Amazon DynamoDB service client object.
export const ddbClient = new DynamoDBClient({ region: DEFAULT_REGION });
```
Create the DynamoDB document client\.  

```
// Create a service client module using ES6 syntax.
import { DynamoDBDocumentClient } from "@aws-sdk/lib-dynamodb";
import { ddbClient } from "./ddbClient.js";

const marshallOptions = {
  // Whether to automatically convert empty strings, blobs, and sets to `null`.
  convertEmptyValues: false, // false, by default.
  // Whether to remove undefined values while marshalling.
  removeUndefinedValues: true, // false, by default.
  // Whether to convert typeof object to map attribute.
  convertClassInstanceToMap: false, // false, by default.
};

const unmarshallOptions = {
  // Whether to return numbers as a string instead of converting them to native JavaScript numbers.
  wrapNumbers: false, // false, by default.
};

// Create the DynamoDB document client.
const ddbDocClient = DynamoDBDocumentClient.from(ddbClient, {
  marshallOptions,
  unmarshallOptions,
});

export { ddbDocClient };
```
Update an item in a table using the DynamoDB document client\.  

```
import { UpdateCommand } from "@aws-sdk/lib-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient.js";

export const updateItem = async () => {
  // Set the parameters.
  const params = {
    TableName: "TABLE_NAME",
    Key: {
      title: "MOVIE_NAME",
      year: "MOVIE_YEAR",
    },
    ProjectionExpression: "#r",
    ExpressionAttributeNames: { "#r": "rank" },
    UpdateExpression: "set info.plot = :p, info.#r = :r",
    ExpressionAttributeValues: {
      ":p": "MOVIE_PLOT",
      ":r": "MOVIE_RANK",
    },
  };
  try {
    const data = await ddbDocClient.send(new UpdateCommand(params));
    console.log("Success - item added or updated", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
updateItem();
```
Update an item in a table using PartiQL\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { ExecuteStatementCommand } from "@aws-sdk/client-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient.js";

const tableName = process.argv[2];
const movieYear1 = process.argv[3];
const movieTitle1 = process.argv[4];
const producer1 = process.argv[5];

export const run = async (tableName, movieYear1, movieTitle1, producer1) => {
  const params = {
    Statement:
      "UPDATE " + tableName + "  SET Producer=? where title=? and year=?",
    Parameters: [{ S: producer1 }, { S: movieTitle1 }, { N: movieYear1 }],
  };
  try {
    await ddbDocClient.send(new ExecuteStatementCommand(params));
    console.log("Success. Item updated.");
    return "Run successfully"; // For unit tests.
  } catch (err) {
    console.error(err);
  }
};
run(tableName, movieYear1, movieTitle1, producer1);
```
Update items by batch in a table using PartiQL\.  

```
// Import required AWS SDK clients and commands for Node.js.
import { BatchExecuteStatementCommand } from "@aws-sdk/client-dynamodb";
import { ddbDocClient } from "../libs/ddbDocClient.js";

const tableName = process.argv[2];
const movieYear1 = process.argv[3];
const movieTitle1 = process.argv[4];
const movieYear2 = process.argv[6];
const movieTitle2 = process.argv[7];
const producer1 = process.argv[5];
const producer2 = process.argv[8];

export const run = async (
  tableName,
  movieYear1,
  movieTitle1,
  movieYear2,
  movieTitle2,
  producer1,
  producer2
) => {
  const params = {
    Statements: [
      {
        Statement:
          "UPDATE " + tableName + " SET producer=? where title=? and year=?",
        Parameters: [{ S: producer1 }, { S: movieTitle1 }, { N: movieYear1 }],
      },
      {
        Statement:
          "UPDATE " + tableName + " SET producer=? where title=? and year=?",
        Parameters: [{ S: producer2 }, { S: movieTitle2 }, { N: movieYear2 }],
      },
    ],
  };
  try {
    await ddbDocClient.send(
      new BatchExecuteStatementCommand(params)
    );
    console.log("Success. Items updated.");
    return "Run successfully"; // For unit tests.
  } catch (err) {
    console.error(err);
  }
};
run(
  tableName,
  movieYear1,
  movieTitle1,
  movieYear2,
  movieTitle2,
  producer1,
  producer2
);
```
+  For API details, see [UpdateItem](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/updateitemcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Write a batch of items<a name="dynamodb_BatchWriteItem_javascript_topic"></a>

The following code example shows how to write a batch of DynamoDB items\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/dynamodb#code-examples)\. 
Create the client\.  

```
// Create the DynamoDB service client module using ES6 syntax.
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
import { DEFAULT_REGION } from "../../../../libs/utils/util-aws-sdk.js";
// Create an Amazon DynamoDB service client object.
export const ddbClient = new DynamoDBClient({ region: DEFAULT_REGION });
```
Create the document client\.  

```
// Create a service client module using ES6 syntax.
import { DynamoDBDocumentClient } from "@aws-sdk/lib-dynamodb";
import { ddbClient } from "./ddbClient.js";

const marshallOptions = {
  // Whether to automatically convert empty strings, blobs, and sets to `null`.
  convertEmptyValues: false, // false, by default.
  // Whether to remove undefined values while marshalling.
  removeUndefinedValues: true, // false, by default.
  // Whether to convert typeof object to map attribute.
  convertClassInstanceToMap: false, // false, by default.
};

const unmarshallOptions = {
  // Whether to return numbers as a string instead of converting them to native JavaScript numbers.
  wrapNumbers: false, // false, by default.
};

// Create the DynamoDB document client.
const ddbDocClient = DynamoDBDocumentClient.from(ddbClient, {
  marshallOptions,
  unmarshallOptions,
});

export { ddbDocClient };
```
Add the items to the table\.  

```
import fs from "fs";
import * as R from "ramda";
import { ddbDocClient } from "../libs/ddbDocClient.js";
import { BatchWriteCommand } from "@aws-sdk/lib-dynamodb";

export const writeData = async () => {
  // Before you run this example, download 'movies.json' from https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/GettingStarted.Js.02.html,
  // and put it in the same folder as the example.
  // Get the movie data parse to convert into a JSON object.
  const allMovies = JSON.parse(fs.readFileSync("moviedata.json", "utf8"));
  // Split the table into segments of 25.
  const dataSegments = R.splitEvery(25, allMovies);
  const TABLE_NAME = "TABLE_NAME"
  try {
    // Loop batch write operation 10 times to upload 250 items.
    for (let i = 0; i < 10; i++) {
      const segment = dataSegments[i];
      for (let j = 0; j < 25; j++) {
        const params = {
          RequestItems: {
            [TABLE_NAME]: [
              {
                // Destination Amazon DynamoDB table name.
                PutRequest: {
                  Item: {
                    year: segment[j].year,
                    title: segment[j].title,
                    info: segment[j].info,
                  },
                },
              },
            ],
          },
        };
        ddbDocClient.send(new BatchWriteCommand(params));
      }
      console.log("Success, table updated.");
    }
  } catch (error) {
    console.log("Error", error);
  }
};
writeData();
```
+  For API details, see [BatchWriteItem](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/batchwriteitemcommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript \(v2\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/dynamodb#code-examples)\. 
  

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region
AWS.config.update({region: 'REGION'});

// Create DynamoDB service object
var ddb = new AWS.DynamoDB({apiVersion: '2012-08-10'});

var params = {
  RequestItems: {
    "TABLE_NAME": [
       {
         PutRequest: {
           Item: {
             "KEY": { "N": "KEY_VALUE" },
               "ATTRIBUTE_1": { "S": "ATTRIBUTE_1_VALUE" },
               "ATTRIBUTE_2": { "N": "ATTRIBUTE_2_VALUE" }
           }
         }
       },
       {
         PutRequest: {
           Item: {
             "KEY": { "N": "KEY_VALUE" },
               "ATTRIBUTE_1": { "S": "ATTRIBUTE_1_VALUE" },
               "ATTRIBUTE_2": { "N": "ATTRIBUTE_2_VALUE" }
           }
         }
       }
    ]
  }
};

ddb.batchWriteItem(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/dynamodb-example-table-read-write-batch.html#dynamodb-example-table-read-write-batch-writing)\. 
+  For API details, see [BatchWriteItem](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/dynamodb-2012-08-10/BatchWriteItem) in *AWS SDK for JavaScript API Reference*\. 

## Scenarios<a name="scenarios"></a>

### Get started using tables, items, and queries<a name="dynamodb_Scenario_GettingStartedMovies_javascript_topic"></a>

The following code example shows how to:
+ Create a table that can hold movie data\.
+ Put, get, and update a single movie in the table\.
+ Write movie data to the table from a sample JSON file\.
+ Query for movies that were released in a given year\.
+ Scan for movies that were released in a range of years\.
+ Delete a movie from the table\.
+ Delete the table\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/dynamodb#code-examples)\. 
Create a DynamoDB client\.  

```
// Create the DynamoDB service client module using ES6 syntax.
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
import { DEFAULT_REGION } from "../../../../libs/utils/util-aws-sdk.js";
// Create an Amazon DynamoDB service client object.
export const ddbClient = new DynamoDBClient({ region: DEFAULT_REGION });
```
Create a DynamoDB document client\.  

```
// Create a service client module using ES6 syntax.
import { DynamoDBDocumentClient } from "@aws-sdk/lib-dynamodb";
import { ddbClient } from "./ddbClient.js";

const marshallOptions = {
  // Whether to automatically convert empty strings, blobs, and sets to `null`.
  convertEmptyValues: false, // false, by default.
  // Whether to remove undefined values while marshalling.
  removeUndefinedValues: true, // false, by default.
  // Whether to convert typeof object to map attribute.
  convertClassInstanceToMap: false, // false, by default.
};

const unmarshallOptions = {
  // Whether to return numbers as a string instead of converting them to native JavaScript numbers.
  wrapNumbers: false, // false, by default.
};

// Create the DynamoDB document client.
const ddbDocClient = DynamoDBDocumentClient.from(ddbClient, {
  marshallOptions,
  unmarshallOptions,
});

export { ddbDocClient };
```
Run the scenario\.  

```
import fs from "fs";
import { splitEvery } from "ramda";
import {
  PutCommand,
  GetCommand,
  UpdateCommand,
  BatchWriteCommand,
  DeleteCommand,
  ScanCommand,
  QueryCommand,
} from "@aws-sdk/lib-dynamodb";
import {
  CreateTableCommand,
  DeleteTableCommand,
  waitUntilTableExists,
  waitUntilTableNotExists,
} from "@aws-sdk/client-dynamodb";

import { ddbClient } from "../libs/ddbClient.js";
import { ddbDocClient } from "../libs/ddbDocClient.js";

/**
 * @param {string} tableName
 */
const createTable = async (tableName) => {
  await ddbClient.send(
    new CreateTableCommand({
      AttributeDefinitions: [
        {
          AttributeName: "year",
          AttributeType: "N",
        },
        {
          AttributeName: "title",
          AttributeType: "S",
        },
      ],
      KeySchema: [
        {
          AttributeName: "year",
          KeyType: "HASH",
        },
        {
          AttributeName: "title",
          KeyType: "RANGE",
        },
      ],
      // Enables "on-demand capacity mode".
      BillingMode: "PAY_PER_REQUEST",
      TableName: tableName,
    })
  );
  await waitUntilTableExists(
    { client: ddbClient, maxWaitTime: 15, maxDelay: 2, minDelay: 1 },
    { TableName: tableName }
  );
};

/**
 *
 * @param {string} tableName
 * @param {Record<string, any> | undefined} attributes
 */
const putItem = async (tableName, attributes) => {
  const command = new PutCommand({
    TableName: tableName,
    Item: attributes,
  });

  await ddbDocClient.send(command);
};

/**
 *
 * @param {string} tableName
 * @param {string} filePath
 * @returns { { movieCount: number } } The number of movies written to the database.
 */
const batchWriteMoviesFromFile = async (tableName, filePath) => {
  const fileContents = fs.readFileSync(filePath);
  const movies = JSON.parse(fileContents, "utf8");

  // Map movies to RequestItems.
  const putMovieRequestItems = movies.map(({ year, title, info }) => ({
    PutRequest: { Item: { year, title, info } },
  }));

  // Organize RequestItems into batches of 25. 25 is the max number of items in a batch request.
  const putMovieBatches = splitEvery(25, putMovieRequestItems);
  const batchCount = putMovieBatches.length;

  // Map batches to promises.
  const batchRequests = putMovieBatches.map(async (batch, i) => {
    const command = new BatchWriteCommand({
      RequestItems: {
        [tableName]: batch,
      },
    });

    await ddbDocClient.send(command).then(() => {
      console.log(
        `Wrote batch ${i + 1} of ${batchCount} with ${batch.length} items.`
      );
    });
  });

  // Wait for all batch requests to resolve.
  await Promise.all(batchRequests);

  return { movieCount: movies.length };
};

/**
 *
 * @param {string} tableName
 * @param {{
 * existingMovieName: string,
 * existingMovieYear: string,
 * newMoviePlot: string,
 * newMovieRank: string}} keyUpdate
 */
const updateMovie = async (
  tableName,
  { existingMovieName, existingMovieYear, newMoviePlot, newMovieRank }
) => {
  await ddbClient.send(
    new UpdateCommand({
      TableName: tableName,
      Key: {
        title: existingMovieName,
        year: existingMovieYear,
      },
      // Define expressions for the new or updated attributes.
      ExpressionAttributeNames: { "#r": "rank" },
      UpdateExpression: "set info.plot = :p, info.#r = :r",
      ExpressionAttributeValues: {
        ":p": newMoviePlot,
        ":r": newMovieRank,
      },
      ReturnValues: "ALL_NEW",
    })
  );
};

/**
 * @param {{ title: string, info: { plot: string, rank: number }, year: number }} movie
 */
const logMovie = (movie) => {
  console.log(` | Title: "${movie.title}".`);
  console.log(` | Plot: "${movie.info.plot}`);
  console.log(` | Year: ${movie.year}`);
  console.log(` | Rank: ${movie.info.rank}`);
};

/**
 *
 * @param {{ title: string, info: { plot: string, rank: number }, year: number }[]} movies
 */
const logMovies = (movies) => {
  console.log("\n");
  movies.forEach((movie, i) => {
    if (i > 0) {
      console.log("-".repeat(80));
    }

    logMovie(movie);
  });
};

/**
 *
 * @param {string} tableName
 * @param {string} title
 * @param {number} year
 * @returns
 */
const getMovie = async (tableName, title, year) => {
  const { Item } = await ddbDocClient.send(
    new GetCommand({
      TableName: tableName,
      Key: {
        title,
        year
      },
      // By default, reads are eventually consistent. "ConsistentRead: true" represents
      // a strongly consistent read. This guarantees that the most up-to-date data is returned. It
      // can also result in higher latency and a potential for server errors.
      ConsistentRead: true,
    })
  );

  return Item;
};

/**
 *
 * @param {string} tableName
 * @param {{ title: string, year: number }} key
 */
const deleteMovie = async (tableName, key) => {
  await ddbDocClient.send(
    new DeleteCommand({
      TableName: tableName,
      Key: key,
    })
  );
};

/**
 *
 * @param {string} tableName
 * @param {number} startYear
 * @param {number} endYear
 * @param {Record<string, any>} startKey
 * @returns {Promise<{}[]>}
 */
const findMoviesBetweenYears = async (
  tableName,
  startYear,
  endYear,
  startKey = undefined
) => {
  const { Items, LastEvaluatedKey } = await ddbClient.send(
    new ScanCommand({
      ConsistentRead: true,
      TableName: tableName,
      ExpressionAttributeNames: { "#y": "year" },
      FilterExpression: "#y BETWEEN :y1 AND :y2",
      ExpressionAttributeValues: { ":y1": startYear, ":y2": endYear },
      ExclusiveStartKey: startKey,
    })
  );

  if (LastEvaluatedKey) {
    return Items.concat(
      await findMoviesBetweenYears(
        tableName,
        startYear,
        endYear,
        LastEvaluatedKey
      )
    );
  } else {
    return Items;
  }
};

/**
 *
 * @param {string} tableName
 * @param {number} year
 * @returns
 */
const queryMoviesByYear = async (tableName, year) => {
  const command = new QueryCommand({
    ConsistentRead: true,
    ExpressionAttributeNames: { "#y": "year" },
    TableName: tableName,
    ExpressionAttributeValues: {
      ":y": year,
    },
    KeyConditionExpression: "#y = :y",
  });

  const { Items } = await ddbDocClient.send(command);

  return Items;
};

/**
 *
 * @param {*} tableName
 */
const deleteTable = async (tableName) => {
  await ddbDocClient.send(new DeleteTableCommand({ TableName: tableName }));
  await waitUntilTableNotExists(
    {
      client: ddbClient,
      maxWaitTime: 10,
      maxDelay: 2,
      minDelay: 1,
    },
    { TableName: tableName }
  );
};
export const runScenario = async ({
  tableName,
  newMovieName,
  newMovieYear,
  existingMovieName,
  existingMovieYear,
  newMovieRank,
  newMoviePlot,
  moviesPath,
}) => {
  console.log(`Creating table named: ${tableName}`);
  await createTable(tableName);
  console.log(`\nTable created.`);

  console.log(`\nAdding "${newMovieName}" to ${tableName}.`);
  await putItem(tableName, { title: newMovieName, year: newMovieYear });
  console.log("\nSuccess - single movie added.");

  console.log("\nWriting hundreds of movies in batches.");
  const { movieCount } = await batchWriteMoviesFromFile(tableName, moviesPath);
  console.log(`\nWrote ${movieCount} movies to database.`);

  console.log(`\nGetting "${existingMovieName}."`);
  const originalMovie = await getMovie(
    tableName,
    existingMovieName,
    existingMovieYear
  );
  logMovie(originalMovie);

  console.log(`\nUpdating "${existingMovieName}" with a new plot and rank.`);
  await updateMovie(tableName, {
    existingMovieName,
    existingMovieYear,
    newMoviePlot,
    newMovieRank,
  });
  console.log(`\n"${existingMovieName}" updated.`);

  console.log(`\nGetting latest info for "${existingMovieName}"`);
  const updatedMovie = await getMovie(
    tableName,
    existingMovieName,
    existingMovieYear
  );
  logMovie(updatedMovie);

  console.log(`\nDeleting "${newMovieName}."`);
  await deleteMovie(tableName, { title: newMovieName, year: newMovieYear });
  console.log(`\n"${newMovieName} deleted.`);

  const [scanY1, scanY2] = [1985, 2003];
  console.log(
    `\nScanning ${tableName} for movies that premiered between ${scanY1} and ${scanY2}.`
  );
  const scannedMovies = await findMoviesBetweenYears(tableName, scanY1, scanY1);
  logMovies(scannedMovies);

  const queryY = 2003;
  console.log(`Querying ${tableName} for movies that premiered in ${queryY}.`);
  const queriedMovies = await queryMoviesByYear(tableName, queryY);
  logMovies(queriedMovies);

  console.log(`Deleting ${tableName}.`);
  await deleteTable(tableName);
  console.log(`${tableName} deleted.`);
};

const main = async () => {
  const args = {
    tableName: "myNewTable",
    newMovieName: "myMovieName",
    newMovieYear: 2022,
    existingMovieName: "This Is the End",
    existingMovieYear: 2013,
    newMovieRank: 200,
    newMoviePlot: "A coder cracks code...",
    moviesPath: "../../../../../../resources/sample_files/movies.json",
  };

  try {
    await runScenario(args);
  } catch (err) {
    // Some extra error handling here to be sure the table is cleaned up if something
    // goes wrong during the scenario run.

    console.error(err);

    const tableName = args.tableName;

    if (tableName) {
      console.log(`Attempting to delete ${tableName}`);
      await ddbClient
        .send(new DeleteTableCommand({ TableName: tableName }))
        .then(() => console.log(`\n${tableName} deleted.`))
        .catch((err) => console.error(`\nFailed to delete ${tableName}.`, err));
    }
  }
};

export { main };
```
+ For API details, see the following topics in *AWS SDK for JavaScript API Reference*\.
  + [BatchWriteItem](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/batchwriteitemcommand.html)
  + [CreateTable](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/createtablecommand.html)
  + [DeleteItem](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/deleteitemcommand.html)
  + [DeleteTable](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/deletetablecommand.html)
  + [DescribeTable](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/describetablecommand.html)
  + [GetItem](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/getitemcommand.html)
  + [PutItem](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/putitemcommand.html)
  + [Query](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/querycommand.html)
  + [Scan](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/scancommand.html)
  + [UpdateItem](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/updateitemcommand.html)

### Query a table by using batches of PartiQL statements<a name="dynamodb_Scenario_PartiQLBatch_javascript_topic"></a>

The following code example shows how to:
+ Get a batch of items by running multiple SELECT statements\.
+ Add a batch of items by running multiple INSERT statements\.
+ Update a batch of items by running multiple UPDATE statements\.
+ Delete a batch of items by running multiple DELETE statements\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/dynamodb#code-examples)\. 
Create the client\.  

```
// Create the DynamoDB service client module using ES6 syntax.
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
import { DEFAULT_REGION } from "../../../../libs/utils/util-aws-sdk.js";
// Create an Amazon DynamoDB service client object.
export const ddbClient = new DynamoDBClient({ region: DEFAULT_REGION });
```
Create the document client\.  

```
// Create a service client module using ES6 syntax.
import { DynamoDBDocumentClient } from "@aws-sdk/lib-dynamodb";
import { ddbClient } from "./ddbClient.js";

const marshallOptions = {
  // Whether to automatically convert empty strings, blobs, and sets to `null`.
  convertEmptyValues: false, // false, by default.
  // Whether to remove undefined values while marshalling.
  removeUndefinedValues: true, // false, by default.
  // Whether to convert typeof object to map attribute.
  convertClassInstanceToMap: false, // false, by default.
};

const unmarshallOptions = {
  // Whether to return numbers as a string instead of converting them to native JavaScript numbers.
  wrapNumbers: false, // false, by default.
};

// Create the DynamoDB document client.
const ddbDocClient = DynamoDBDocumentClient.from(ddbClient, {
  marshallOptions,
  unmarshallOptions,
});

export { ddbDocClient };
```
Query items by batch\.  

```
*/
import fs from "fs";
import {splitEvery} from "ramda";
import {
    BatchExecuteStatementCommand,
    BatchWriteCommand
} from "@aws-sdk/lib-dynamodb";
import {
    CreateTableCommand,
    DeleteTableCommand,
    waitUntilTableExists,
    waitUntilTableNotExists,
} from "@aws-sdk/client-dynamodb";

import {ddbClient} from "../libs/ddbClient.js";
import {ddbDocClient} from "../libs/ddbDocClient.js";

/**
 * @param {string} tableName
 */
const createTable = async (tableName) => {
    await ddbClient.send(
        new CreateTableCommand({
            TableName: tableName,
            AttributeDefinitions: [
                {
                    AttributeName: "year",
                    AttributeType: "N",
                },
                {
                    AttributeName: "title",
                    AttributeType: "S",
                }
            ],
            KeySchema: [
                {
                    AttributeName: "year",
                    KeyType: "HASH",
                },
                {
                    AttributeName: "title",
                    KeyType: "RANGE",
                },
            ],
            // Enables "on-demand capacity mode".
            BillingMode: "PAY_PER_REQUEST"
        })
    );
    await waitUntilTableExists(
        {client: ddbClient, maxWaitTime: 15, maxDelay: 2, minDelay: 1},
        {TableName: tableName}
    );
};

/**
 *
 * @param {string} tableName
 * @param {string} filePath
 * @returns { { movieCount: number } } The number of movies written to the database.
 */
const batchWriteMoviesFromFile = async (tableName, filePath) => {
    const fileContents = fs.readFileSync(filePath);
    const movies = JSON.parse(fileContents, "utf8");

    // Map movies to RequestItems.
    const putMovieRequestItems = movies.map(({year, title, info}) => ({
        PutRequest: {Item: {year, title, info}},
    }));

    // Organize RequestItems into batches of 25. 25 is the max number of items in a batch request.
    const putMovieBatches = splitEvery(25, putMovieRequestItems);
    const batchCount = putMovieBatches.length;

    // Map batches to promises.
    const batchRequests = putMovieBatches.map(async (batch, i) => {
        const command = new BatchWriteCommand({
            RequestItems: {
                [tableName]: batch,
            },
        });

        await ddbDocClient.send(command).then(() => {
            console.log(
                `Wrote batch ${i + 1} of ${batchCount} with ${batch.length} items.`
            );
        });
    });

    // Wait for all batch requests to resolve.
    await Promise.all(batchRequests);

    return {movieCount: movies.length};
};

/**
 *
 * @param {string} tableName
 * @param {{
 * existingMovieName1: string,
 * existingMovieYear1: number }} keyUpdate1
 * @param {{
 * existingMovieName2: string,
 * existingMovieYear2: number }} keyUpdate2
 */

const batchGetMovies = async (tableName, keyUpdate1, keyUpdate2) => {
    const Items = await ddbDocClient.send(
        new BatchExecuteStatementCommand({
                Statements: [
                    {
                        Statement:
                            "SELECT * FROM " + tableName + " where title=? and year=?",
                        Parameters: [keyUpdate1.existingMovieName1, keyUpdate1.existingMovieYear1]
                    },
                    {
                        Statement:
                            "SELECT * FROM " + tableName + " where title=? and year=?",
                        Parameters: [keyUpdate2.existingMovieName2, keyUpdate2.existingMovieYear2]
                    }
                ]
            }
        )
    )
    return Items
};
/**
 *
 * @param {string} tableName
 * @param {{
 * existingMovieName1: string,
 * existingMovieYear1: number,
 * newProducer1: string }} keyUpdate1
 * @param {{
 * existingMovieName2: string,
 * existingMovieYear2: number,
 * newProducer2: string }} keyUpdate2
 */
const batchUpdateMovies = async (
    tableName,
    keyUpdate1, keyUpdate2
) => {
    await ddbDocClient.send(
        new BatchExecuteStatementCommand({
            Statements: [
                {
                    Statement:
                        "UPDATE " +
                        tableName +
                        " SET Producer=? where title=? and year=?",
                    Parameters: [keyUpdate1.newProducer1, keyUpdate1.existingMovieName1, keyUpdate1.existingMovieYear1
                    ],
                },
                {
                    Statement:
                        "UPDATE " +
                        tableName +
                        " SET Producer=? where title=? and year=?",
                    Parameters: [
                        keyUpdate2.newProducer2, keyUpdate2.existingMovieName2, keyUpdate2.existingMovieYear2
                    ],
                }
            ],
        })
    );
};

/**
 *
 * @param {string} tableName
 * @param {{ existingMovieName1: string, existingMovieYear1: number }} key1,
 * @param {{ existingMovieName2: string, existingMovieYear2: number}} key2
 */
const batchDeleteMovies = async (tableName, key1, key2) => {
    await ddbDocClient.send(
        new BatchExecuteStatementCommand({
            Statements: [
                {
                    Statement:
                        "DELETE FROM " + tableName + " where title=? and year=?",
                    Parameters: [key1.existingMovieName1, key1.existingMovieYear1],
                },
                {
                    Statement:
                        "DELETE FROM " + tableName + " where title=? and year=?",
                    Parameters: [key2.existingMovieName2, key2.existingMovieYear2],
                },
            ],
        }))
};

/**
 *
 * @param {string} tableName
 * @param {{ newMovieName1: string, newMovieYear1: number }} key1,
 * @param {{ newMovieName2: string, newMovieYear2: number }} key2
 */

const batchPutItems = async (tableName, key1, key2) => {
    const command = new BatchExecuteStatementCommand({
        Statements: [
            {
                Statement:
                    "INSERT INTO " + tableName + " value  {'title':?, 'year':?}",
                Parameters: [key1.newMovieName1, key1.newMovieYear1],
            },
            {
                Statement:
                    "INSERT INTO " + tableName + " value  {'title':?, 'year':?}",
                Parameters: [key2.newMovieName2, key2.newMovieYear2],
            }
        ]
    })

    await ddbDocClient.send(command);
};

/**
 *
 * @param {{ title: string, info: { plot: string, rank: number }, year: number }[]} movies
 */
const logMovies = (Items) => {
    console.log("Success. The query return the following data.");
    for (let i = 0; i < Items.Responses.length; i++) {
        console.log(Items.Responses[i].Item);
    }
};


/**
 *
 * @param {*} tableName
 */
const deleteTable = async (tableName) => {
    await ddbDocClient.send(new DeleteTableCommand({TableName: tableName}));
    await waitUntilTableNotExists(
        {
            client: ddbClient,
            maxWaitTime: 10,
            maxDelay: 2,
            minDelay: 1,
        },
        {TableName: tableName}
    );
};

export const runScenario = async ({
                                      tableName,
                                      newMovieName1,
                                      newMovieYear1,
                                      newMovieName2,
                                      newMovieYear2,
                                      existingMovieName1,
                                      existingMovieYear1,
                                      existingMovieName2,
                                      existingMovieYear2,
                                      newProducer1,
                                      newProducer2,
                                      moviesPath
                                  }) => {
    await createTable(tableName);
    console.log(`Creating table named: ${tableName}`);
    console.log(`\nTable created.`);
    console.log("\nWriting hundreds of movies in batches.");
    const {movieCount} = await batchWriteMoviesFromFile(tableName, moviesPath);
    console.log(`\nWrote ${movieCount} movies to database.`);
    console.log(`\nGetting "${existingMovieName1}" and "${existingMovieName2}"`);
    const originalMovies = await batchGetMovies(
        tableName,
        {
            existingMovieName1,
            existingMovieYear1
        },
        {
            existingMovieName2,
            existingMovieYear2
        }
    );
    logMovies(originalMovies);
    console.log(`\nUpdating "${existingMovieName1} and ${existingMovieName2} with new producers.`);
    await batchUpdateMovies(tableName,
        {
            existingMovieName1,
            existingMovieYear1,
            newProducer1
        },
        {
            existingMovieName2,
            existingMovieYear2,
            newProducer2
        }
    );
    //console.log(`\n"${existingMovieName1}" and ${existingMovieName2}" updated.`);
    console.log(`\nDeleting "${existingMovieName1}."`);
    await batchDeleteMovies(tableName, {
            existingMovieName1,
            existingMovieYear1
        },
        {
            existingMovieName2,
            existingMovieYear2
        }
    );
    console.log(`\n"${existingMovieName1} and ${existingMovieName2} deleted.`);
    console.log(`\nAdding "${newMovieName1}" and "${newMovieName2}" to ${tableName}.`);
    await batchPutItems(tableName, {newMovieName1, newMovieYear1}, {newMovieName2, newMovieYear2});
    console.log("\nSuccess - single movie added.");
    console.log(`Deleting ${tableName}.`);
    await deleteTable(tableName);
    console.log(`${tableName} deleted.`);
};

const main = async () => {
    const args = {
        tableName: "myNewTable",
        newMovieName1: "myMovieName1",
        newMovieYear1: 2022,
        newMovieName2: "myMovieName2",
        newMovieYear2: 2023,
        existingMovieName1: "This Is the End",
        existingMovieYear1: 2013,
        existingMovieName2: "Deep Impact",
        existingMovieYear2: 1998,
        newProducer1: "Amazon Movies",
        newProducer2: "Amazon Movies2",
        moviesPath: "../../../../../../resources/sample_files/movies.json",
    };

    try {
        await runScenario(args);
    } catch (err) {
        // Some extra error handling here to be sure the table is cleaned up if something
        // goes wrong during the scenario run.

        console.error(err);

        const tableName = args.tableName;

        if (tableName) {
            console.log(`Attempting to delete ${tableName}`);
            await ddbClient
                .send(new DeleteTableCommand({TableName: tableName}))
                .then(() => console.log(`\n${tableName} deleted.`))
                .catch((err) => console.error(`\nFailed to delete ${tableName}.`, err));
        }
    }
};

export {main};
```
+  For API details, see [BatchExecuteStatement](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/batchexecutestatementcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Query a table using PartiQL<a name="dynamodb_Scenario_PartiQLSingle_javascript_topic"></a>

The following code example shows how to:
+ Get an item by running a SELECT statement\.
+ Add an item by running an INSERT statement\.
+ Update an item by running an UPDATE statement\.
+ Delete an item by running a DELETE statement\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/dynamodb#code-examples)\. 
Create the client\.  

```
// Create the DynamoDB service client module using ES6 syntax.
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
import { DEFAULT_REGION } from "../../../../libs/utils/util-aws-sdk.js";
// Create an Amazon DynamoDB service client object.
export const ddbClient = new DynamoDBClient({ region: DEFAULT_REGION });
```
Create the document client\.  

```
// Create a service client module using ES6 syntax.
import { DynamoDBDocumentClient } from "@aws-sdk/lib-dynamodb";
import { ddbClient } from "./ddbClient.js";

const marshallOptions = {
  // Whether to automatically convert empty strings, blobs, and sets to `null`.
  convertEmptyValues: false, // false, by default.
  // Whether to remove undefined values while marshalling.
  removeUndefinedValues: true, // false, by default.
  // Whether to convert typeof object to map attribute.
  convertClassInstanceToMap: false, // false, by default.
};

const unmarshallOptions = {
  // Whether to return numbers as a string instead of converting them to native JavaScript numbers.
  wrapNumbers: false, // false, by default.
};

// Create the DynamoDB document client.
const ddbDocClient = DynamoDBDocumentClient.from(ddbClient, {
  marshallOptions,
  unmarshallOptions,
});

export { ddbDocClient };
```
Query single items\.  

```
*/
import fs from "fs";
import {splitEvery} from "ramda";
import {
    ExecuteStatementCommand,
    BatchWriteCommand
} from "@aws-sdk/lib-dynamodb";
import {
    CreateTableCommand,
    DeleteTableCommand,
    waitUntilTableExists,
    waitUntilTableNotExists,
} from "@aws-sdk/client-dynamodb";

import {ddbClient} from "../libs/ddbClient.js";
import {ddbDocClient} from "../libs/ddbDocClient.js";

/**
 * @param {string} tableName
 */
const createTable = async (tableName) => {
    await ddbClient.send(
        new CreateTableCommand({
            TableName: tableName,
            AttributeDefinitions: [
                {
                    AttributeName: "year",
                    AttributeType: "N",
                },
                {
                    AttributeName: "title",
                    AttributeType: "S",
                }
            ],
            KeySchema: [
                {
                    AttributeName: "year",
                    KeyType: "HASH",
                },
                {
                    AttributeName: "title",
                    KeyType: "RANGE",
                },
            ],
            // Enables "on-demand capacity mode".
            BillingMode: "PAY_PER_REQUEST"
        })
    );
    await waitUntilTableExists(
        {client: ddbClient, maxWaitTime: 15, maxDelay: 2, minDelay: 1},
        {TableName: tableName}
    );
};

/**
 *
 * @param {string} tableName
 * @param {string} filePath
 * @returns { { movieCount: number } } The number of movies written to the database.
 */
const batchWriteMoviesFromFile = async (tableName, filePath) => {
    const fileContents = fs.readFileSync(filePath);
    const movies = JSON.parse(fileContents, "utf8");

    // Map movies to RequestItems.
    const putMovieRequestItems = movies.map(({year, title, info}) => ({
        PutRequest: {Item: {year, title, info}},
    }));

    // Organize RequestItems into batches of 25. 25 is the max number of items in a batch request.
    const putMovieBatches = splitEvery(25, putMovieRequestItems);
    const batchCount = putMovieBatches.length;

    // Map batches to promises.
    const batchRequests = putMovieBatches.map(async (batch, i) => {
        const command = new BatchWriteCommand({
            RequestItems: {
                [tableName]: batch,
            },
        });

        await ddbDocClient.send(command).then(() => {
            console.log(
                `Wrote batch ${i + 1} of ${batchCount} with ${batch.length} items.`
            );
        });
    });

    // Wait for all batch requests to resolve.
    await Promise.all(batchRequests);

    return {movieCount: movies.length};
};

/**
 *
 * @param {string} tableName
 * @param {{
 * existingMovieName: string,
 * existingMovieYear: number }} keyUpdate
 * @returns
 */

const getMovie = async (tableName, keyUpdate) => {
    const {Items} = await ddbDocClient.send(
        new ExecuteStatementCommand({
            Statement: "SELECT * FROM " + tableName + " where title=? and year=?",
            Parameters: [keyUpdate.existingMovieName, keyUpdate.existingMovieYear]
        })
    )
    return Items
};
/**
 *
 * @param {string} tableName
 * @param {{
 * existingMovieName: string,
 * existingMovieYear: number,
 * newProducer: string }} keyUpdate
 */
const updateMovie = async (
    tableName, keyUpdate
) => {
    await ddbClient.send(
        new ExecuteStatementCommand({
            Statement:
                "UPDATE " +
                tableName +
                " SET Producer=? where title=? and year=?",
            Parameters: [
                keyUpdate.newProducer,
                keyUpdate.existingMovieName,
                keyUpdate.existingMovieYear
            ],
        })
    );
};

/**
 *
 * @param {string} tableName
 * @param {{ existingMovieName: string, existingMovieYear: number }} key
 */
const deleteMovie = async (tableName, key) => {
    await ddbDocClient.send(
        new ExecuteStatementCommand({
            Statement: "DELETE FROM " + tableName + " where title=? and year=?",
            Parameters: [key.existingMovieName, key.existingMovieYear],
        })
    );
};

/**
 *
 * @param {string} tableName
 * @param {{ newMovieName: string, newMovieYear: number }} key
 */

const putItem = async (tableName, key) => {
    const command = new ExecuteStatementCommand({
        Statement:
            "INSERT INTO " + tableName + " value  {'title':?, 'year':?}",
        Parameters: [key.newMovieName, key.newMovieYear],
    })

    await ddbDocClient.send(command);
};

/**
 * @param {{ title: string, info: { plot: string, rank: number }, year: number }} movie
 */
const logMovie = (movie) => {
    console.log(` | Title: "${movie.title}".`);
    console.log(` | Plot: "${movie.info.plot}`);

    console.log(` | Year: ${movie.year}`);
    console.log(` | Rank: ${movie.info.rank}`);
};

/**
 *
 * @param {{ title: string, info: { plot: string, rank: number }, year: number }[]} movies
 */
const logMovies = (movies) => {
    console.log("\n");
    movies.forEach((movie, i) => {
        if (i > 0) {
            console.log("-".repeat(80));
        }
        logMovie(movie);
    });
};


/**
 *
 * @param {*} tableName
 */
const deleteTable = async (tableName) => {
    await ddbDocClient.send(new DeleteTableCommand({TableName: tableName}));
    await waitUntilTableNotExists(
        {
            client: ddbClient,
            maxWaitTime: 10,
            maxDelay: 2,
            minDelay: 1,
        },
        {TableName: tableName}
    );
};

export const runScenario = async ({tableName, newMovieName, newMovieYear, existingMovieName, existingMovieYear, newProducer, moviesPath}) => {
    console.log(`Creating table named: ${tableName}`);
    await createTable(tableName);
    console.log(`\nTable created.`);
    console.log("\nWriting hundreds of movies in batches.");
    const {movieCount} = await batchWriteMoviesFromFile(tableName, moviesPath);
    console.log(`\nWrote ${movieCount} movies to database.`);
    console.log(`\nGetting "${existingMovieName}."`);
    const originalMovie = await getMovie(
        tableName,
        {
            existingMovieName,
            existingMovieYear
        }
    );
    logMovies(originalMovie);
    console.log(`\nUpdating "${existingMovieName}" with a new producer.`);
    await updateMovie(tableName, {
        newProducer,
        existingMovieName,
        existingMovieYear
    });
    console.log(`\n"${existingMovieName}" updated.`);
    console.log(`\nDeleting "${existingMovieName}."`);
    await deleteMovie(tableName, {existingMovieName, existingMovieYear});
    console.log(`\n"${existingMovieName} deleted.`);
    console.log(`\nAdding "${newMovieName}" to ${tableName}.`);
    await putItem(tableName, {newMovieName, newMovieYear});
    console.log("\nSuccess - single movie added.");
    console.log(`Deleting ${tableName}.`);
    await deleteTable(tableName);
    console.log(`${tableName} deleted.`);
};

const main = async () => {
    const args = {
        tableName: "myNewTable",
        newMovieName: "myMovieName",
        newMovieYear: 2022,
        existingMovieName: "This Is the End",
        existingMovieYear: 2013,
        newProducer: "Amazon Movies",
        moviesPath: "../../../../../../resources/sample_files/movies.json",
    };

    try {
        await runScenario(args);
    } catch (err) {
        // Some extra error handling here to be sure the table is cleaned up if something
        // goes wrong during the scenario run.

        console.error(err);

        const tableName = args.tableName;

        if (tableName) {
            console.log(`Attempting to delete ${tableName}`);
            await ddbClient
                .send(new DeleteTableCommand({TableName: tableName}))
                .then(() => console.log(`\n${tableName} deleted.`))
                .catch((err) => console.error(`\nFailed to delete ${tableName}.`, err));
        }
    }
};

export {main};
```
+  For API details, see [ExecuteStatement](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/executestatementcommand.html) in *AWS SDK for JavaScript API Reference*\. 