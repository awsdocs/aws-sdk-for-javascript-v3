--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Reading and writing a single item in DynamoDB<a name="dynamodb-example-table-read-write"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to add an item in a DynamoDB table\.
+ How to retrieve, an item in a DynamoDB table\.
+ How to delete an item in a DynamoDB table\.

## The scenario<a name="dynamodb-example-table-read-write-scenario"></a>

In this example, you use a series of Node\.js modules to read and write one item in a DynamoDB table by using these methods of the `DynamoDB` client class:
+ [PutItemCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/putitemcommand.html)
+ [UpdateItemCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/updateitemcommand.html)
+ [GetItemCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/getitemcommand.html)
+ [DeleteItemCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/deleteitemcommand.html)

## Prerequisite tasks<a name="dynamodb-example-table-read-write-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Set up the project environment to run these Node\.js examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/dynamodb/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create a DynamoDB table whose items you can access\. For more information about creating a DynamoDB table, see [Creating and using tables in DynamoDB](dynamodb-examples-using-tables.md)\.

**Important**  
These examples use ECMAScript6 \(ES6\)\. This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.  
However, if you prefer to use CommonJS sytax, please refer to [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)

## Writing an item<a name="dynamodb-example-table-read-write-writing-an-item"></a>

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

Create a Node\.js module with the file name `ddb_putitem.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. Create a JSON object containing the parameters needed to add an item, which in this example includes the name of the table and a map that defines the attributes to set and the values for each attribute\. Call the `PutItemCommand` method of the DynamoDB client service object\.

**Note**  
Replace *TABLE\_NAME* with the name of the table\.

**Note**  
The following code example writes to a table with a primary key composed of only a partion key \- `CUSTOMER_ID` \- and a sort key \- `CUSTOMER_NAME`\. If the table's primary key is composed of only a partition key, you only specify the partition key\. 

```
// Import required AWS SDK clients and commands for Node.js
import { PutItemCommand } from "@aws-sdk/client-dynamodb";
import { ddbClient } from "./libs/ddbClient.js";

// Set the parameters
export const params = {
  TableName: "TABLE_NAME",
  Item: {
    CUSTOMER_ID: { N: "001" },
    CUSTOMER_NAME: { S: "Richard Roe" },
  },
};

export const run = async () => {
  try {
    const data = await ddbClient.send(new PutItemCommand(params));
    console.log(data);
    return data;
  } catch (err) {
    console.error(err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ddb_putitem.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_putitem.js)\.

## Update an item<a name="dynamodb-example-table-update-an-item"></a>

You can update an existing item in table\. Create a `libs` directory, and create a Node\.js module with the file name `ddbClient.js`\. Copy and paste the code below into it, which creates the DynamoDB client object\. Replace *REGION* with your AWS region\.

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

Create a Node\.js module with the file name `ddb_updateitem.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. Create a JSON object containing the parameters needed to add an item, which in this example includes the name of the table, the key to update, andu date expression that maps the new attribute names, and values for each new attribute\. Call the `UpdateItemCommand` method of the DynamoDB client service object\.

**Note**  
Replace *TABLE\_NAME* with the name of the table\.

**Note**  
The following code example writes to a table with a primary key composed of only a partion key\. If the table's primary key is composed of only a partition key, you only specify the partition key\. 

```
// Import required AWS SDK clients and commands for Node.js
import { UpdateItemCommand } from "@aws-sdk/client-dynamodb";
import { ddbClient } from "./libs/ddbClient.js";

export const params = {
    TableName: "TABLE_NAME",
    /*
    Convert the attribute JavaScript object you are updating to the required
    Amazon  DynamoDB record. The format of values specifies the datatype. The
    following list demonstrates different datatype formatting requirements:
    String: "String",
    NumAttribute: 1,
    BoolAttribute: true,
    ListAttribute: [1, "two", false],
    MapAttribute: { foo: "bar" },
    NullAttribute: null
     */
    Key: {
        primaryKey: {"ATTRIBUTE_TYPE":"KEY_VALUE"}, // For example, 'Season': {N:2}.
        sortKey: {"ATTRIBUTE_TYPE":"KEY_VALUE"} // For example,  'Episode': {S: "The return"}; (only required if table has sort key).
    },
    // Define expressions for the new or updated attributes
    UpdateExpression: "set NEW_ATTRIBUTE_NAME_1 = :t, NEW_ATTRIBUTE_NAME_2 = :s", // For example, "'set Title = :t, Subtitle = :s'"
    ExpressionAttributeValues: {
        ":t": "NEW_ATTRIBUTE_VALUE_1", // For example ':t' : 'NEW_TITLE'
        ":s": "NEW_ATTRIBUTE_VALUE_2", // For example ':s' : 'NEW_SUBTITLE'
    },
    ReturnValues: "ALL_NEW"
};
export const run = async () => {
    try {
        const data = await ddbClient.send(new UpdateItemCommand(params));
        console.log(data);
        return data;
    } catch (err) {
        console.error(err);
    }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ddb_putitem.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_putitem.js)\.

## Getting an item<a name="dynamodb-example-table-read-write-getting-an-item"></a>

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

Create a Node\.js module with the file name `ddb_getitem.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. To identify the item to get, you must provide the value of the primary key for that item in the table\. By default, the `GetItemCommand` method returns all the attribute values defined for the item\. To get only a subset of all possible attribute values, specify a projection expression\.

Create a JSON object containing the parameters needed to get an item, which in this example includes the name of the table, the name and value of the key for the item you're getting, and a projection expression that identifies the item attribute you want to retrieve\. Call the `GetItemCommand` method of the DynamoDB client service object\.

**Note**  
Replace *TABLE\_NAME* with the name of the table, *KEY\_NAME* with the primary key of the table, *KEY\_NAME\_VALUE* with the value of the primary key row containing the attribute value, and *ATTRIBUTE\_NAME* the name of the attribute column containing the attribute value\.

The following code example retrieves an item from a table with a primary key composed of only a partion key \- `KEY_NAME` \- and not of both a partion and sort key\. If the table has a primary key composed of a partition key and a sort key, you must also specify the sort key name and attribute\.

```
// Import required AWS SDK clients and commands for Node.js
import { GetItemCommand } from "@aws-sdk/client-dynamodb";
import { ddbClient } from "./libs/ddbClient.js";

// Set the parameters
export const params = {
  TableName: "TABLE_NAME", //TABLE_NAME
  Key: {
    KEY_NAME: { N: "KEY_VALUE" },
  },
  ProjectionExpression: "ATTRIBUTE_NAME",
};

export const run = async () => {
  const data = await ddbClient.send(new GetItemCommand(params));
  return data;
  console.log("Success", data.Item);
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ddb_getitem.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_getitem.js)\.

## Deleting an item<a name="dynamodb-example-table-read-write-deleting-an-item"></a>

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

Create a Node\.js module with the file name `ddb_deleteitem.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. Create a JSON object containing the parameters needed to delete an item, which in this example includes the name of the table and both the key name and value for the item you're deleting\. Call the `DeleteItemCommand` method of the DynamoDB client service object\.

**Note**  
Replace *TABLE\_NAME* with the name of the table\.

**Note**  
The following code example below deletes a item with a primary key composed of only a partion key \- `KEY_NAME` \- and not of both a partion and sort key\. If the table has a primary key composed of a partition key and a sort key, you must also specify the sort key name and attribute\.

```
// Import required AWS SDK clients and commands for Node.js
import { DeleteItemCommand } from "@aws-sdk/client-dynamodb";
import { ddbClient } from "./libs/ddbClient.js";

// Set the parameters
export const params = {
  TableName: "CUSTOMER_LIST_NEWEST",
  Key: {
    CUSTOMER_ID: { N: "1" },
  },
};

export const run = async () => {
  try {
    const data = await ddbClient.send(new DeleteItemCommand(params));
    console.log("Success, item deleted", data);
    return data;
  } catch (err) {
    console.log("Error", err);
    /*if (err && err.code === "ResourceNotFoundException") {
      console.log("Error: Table not found");
    } else if (err && err.code === "ResourceInUseException") {
      console.log("Error: Table in use");
    }*/
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ddb_deleteitem.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_deleteitem.js)\.