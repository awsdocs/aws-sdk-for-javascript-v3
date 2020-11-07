--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Using the DynamoDB Utilities<a name="dynamodb-example-dynamodb-utilities"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to access a DynamoDB table using the DynamoDB utilities\.

## The Scenario<a name="dynamodb-example-document-client-scenario"></a>

The DynamoDB utilities simplify working with items by abstracting the notion of attribute values\. This abstraction annotates native JavaScript types supplied as input parameters, and converts annotated response data to native JavaScript types\.

For more information about the DynamoDB utilities, see [@aws\-sdk/utils\-dynamo README](https://github.com/aws/aws-sdk-js-v3/tree/master/packages/util-dynamodb) on GitHub\. For more information about programming with Amazon DynamoDB, see [Programming with DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Programming.html) in the *Amazon DynamoDB Developer Guide*\.

In this example, you use a series of Node\.js modules to perform basic operations on a DynamoDB table using DynamoDB utilities\. The code uses the SDK for JavaScript to query and scan tables using these methods of the DynamoDB class:
+ [getItem](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html#get-property)
+ [putItem](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html#put-property)
+ [updateItem](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html#update-property)
+ [query](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html#query-property)
+ [deleteItem](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html#delete-property)

## Prerequisite Tasks<a name="dynamodb-example-document-client-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules, including the `@aws-sdk/util-dynamodb`, which is a package that provides utilities to `@aws-sdk/dynamodb`\. Follow the instructions on [ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/dynamodb/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so with minor adjustments these examples can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create a DynamoDB table whose items you can access\. For more information about creating a DynamoDB table using the SDK for JavaScript, see [Creating and using tables in DynamoDB](dynamodb-examples-using-tables.md)\. You can also use the [DynamoDB console](https://console.aws.amazon.com/dynamodb/) to create a table\.
**Note**  
These examples import and use the required AWS Service V3 package clients, V3 commands, and use the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

## Getting an Item from a Table<a name="dynamodb-example-document-client-get"></a>

Create a Node\.js module with the file name `ddbdoc_get_item.ts`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\. This includes the `@aws-sdk/util-dynamodb`, which is a package that provides utilities to `@aws-sdk/dynamodb`\. Create a JSON object containing the parameters needed get an item from the table, which in this example includes the name of the table, the name of the hash key in that table, and the value of the hash key for the item you want to get\. Use the `marshall` command of the `@aws-sdk/util-dynamodb` package to convert the object into a DynamoDB record\. Call the `getItem` method of the DynamoDB client\. Use the `unmarshall` command of the `@aws-sdk/util-dynamodb` package to convert the Dynamo record into a JavaScript object\.

```
// Import required AWS SDK clients and commands for Node.js
const { DynamoDB } = require("@aws-sdk/client-dynamodb");
const { marshall, unmarshall } = require("@aws-sdk/util-dynamodb");

// Set the parameters
const params = {
  TableName: "TABLE_NAME",
  // Convert the key JavaScript object you are retrieving to the required DynamoDB format. The format of values specifies
  // the datatype. The following list demonstrates different datatype formatting requirements:
  // HashKey: "hashKey",
  // NumAttribute: 1,
  // BoolAttribute: true,
  // ListAttribute: [1, "two", false],
  // MapAttribute: { foo: "bar" },
  // NullAttribute: null
  Key: marshall({
    primaryKey: VALUE, // For example, "Season: 2"
    sortKey: VALUE, // For example,  "Episode: 1" (only required if table has sort key)
  }),
};

// Create DynamoDB document client
const client = new DynamoDB({ region: "REGION" });

const run = async () => {
  try {
    const { Item } = await client.getItem(params);
    // Convert the DynamoDB record you retrieved into a JavaScript object
    unmarshall(Item);
    // Print the JavaScript object to console
    console.log(Item);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ddbdoc_get_item.ts // To use JavaScript, enter 'node ddbdoc_get_item.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddbdoc_get_item.ts)\.

## Putting an Item in a Table<a name="dynamodb-example-document-client-put"></a>

Create a Node\.js module with the file name `ddbdoc_put_item.ts`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\. This includes the `@aws-sdk/util-dynamodb`, which is a package that provides utilities to `@aws-sdk/dynamodb`\. Create a JSON object containing the parameters needed to write an item to the table, which in this example includes the name of the table and a description of the item to add or update that includes the hashkey and value and names and values for attributes to set on the item\. Use the `marshall` command of the `@aws-sdk/util-dynamodb` package to convert the object into a DynamoDB record\. Call the `putItem` method of the DynamoDB client\.

```
// Import required AWS SDK clients and commands for Node.js
const { DynamoDB } = require("@aws-sdk/client-dynamodb");
const { marshall, unmarshall } = require("@aws-sdk/util-dynamodb");

// Set the parameters
const TableName = "TABLE_NAME";
// Define the JavaScript object you are creating or replacing. The format of values specifies the datatype.
// The following list demonstrates different datatype formatting requirements:
// HashKey: "hashKey",
// NumAttribute: 1,
// BoolAttribute: true,
// ListAttribute: [1, "two", false],
// MapAttribute: { foo: "bar" },
// NullAttribute: null
const input = {
    primaryKey: VALUE, // For example, "Season: 2"
    sortKey: VALUE // For example,  "Episode: 2" (only required if table has sort key)
    NEW_ATTRIBUTE_1: NEW_ATTRIBUTE_1_VALUE //For example "'Title': 'The Beginning'"
};
// Marshall util converts then JavaScript object to DynamoDB format
const Item = marshall(input);

// Create DynamoDB document client
const client = new DynamoDB({ region:"REGION" });

const run = async () => {
    try {
        const data = await client.putItem({ TableName, Item });
        console.log('Success - put')}
    catch(err){
        console.log('Error', err)}
}
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ddbdoc_put_item.ts // To use JavaScript, enter 'node ddbdoc_put_item.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddbdoc_put_item.ts)\.

## Updating an Item in a Table<a name="dynamodb-example-document-client-update"></a>

Create a Node\.js module with the file name `ddbdoc_update_item.ts`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\. This includes the `@aws-sdk/util-dynamodb`, which is a package that provides utilities to `@aws-sdk/dynamodb`\. Create a JSON object containing the parameters needed to write an item to the table, which in this example includes the name of the table, the key of the item to update, a set of `UpdateExpressions` that define the attributes of the item to update with tokens you assign values to in the `ExpressionAttributeValues` parameters\. Use the `marshall` command of the `@aws-sdk/util-dynamodb` package to convert the object into a DynamoDB record\.Call the `updateItem` method of the DynamoDB client\.

```
// Import required AWS SDK clients and commands for Node.js
const { DynamoDB } = require("@aws-sdk/client-dynamodb");
const { marshall, unmarshall } = require("@aws-sdk/util-dynamodb");

// Set the parameters
const params = {
  TableName: "TABLE_NAME",
  // Convert the key JavaScript object you are deleting to the required DynamoDB format. The format of values
  // specifies the datatype. The following list demonstrates different datatype formatting requirements:
  // HashKey: "hashKey",
  // NumAttribute: 1,
  // BoolAttribute: true,
  // ListAttribute: [1, "two", false],
  // MapAttribute: { foo: "bar" },
  // NullAttribute: null
  Key: marshall({
    primaryKey: VALUE, // For example, "Season: 2"
    sortKey: VALUE, // For example,  "Episode: 1" (only required if table has sort key)
  }),
  // Define expressions for the new or updated attributes
  UpdateExpression: "set ATTRIBUTE_NAME_1 = :t, ATTRIBUTE_NAME_2 = :s", // For example, "'set Title = :t, Subtitle = :s'"
  // Convert the attribute JavaScript object you are deleting to the required DynamoDB format
  ExpressionAttributeValues: marshall({
    ":t": NEW_ATTRIBUTE_VALUE_1, // For example "':t' : 'NEW_TITLE'"
    ":s": NEW_ATTRIBUTE_VALUE_2, // For example " ':s' : 'NEW_SUBTITLE'"
  }),
};

// Create DynamoDB document client
const client = new DynamoDB({ region: "REGION" });

const run = async () => {
  try {
    const { Item } = await client.updateItem(params);
    console.log("Success - updated");
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ddbdoc_update_item.ts // If you prefer JavaScript, enter 'node ddbdoc_update_item.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddbdoc_update_item.ts)\.

## Querying a Table<a name="dynamodb-example-document-client-query"></a>

This example queries a table that contains episode information about a video series, returning the episode titles and subtitles of second season episodes past episode 9 that contain a specified phrase in their subtitle\.

Create a Node\.js module with the file name `ddbdoc_query_item.ts`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\. This includes the `@aws-sdk/util-dynamodb`, which is a package that provides utilities to `@aws-sdk/dynamodb`\. Create a JSON object containing the parameters needed to query the table, which in this example includes the table name, the `ExpressionAttributeValues` needed by the query, and a `KeyConditionExpression` that uses those values to define which items the query returns\. Use the `marshall` command of the `@aws-sdk/util-dynamodb` package to convert the object into a DynamoDB record\. Call the `query` method of the DynamoDB client\.

```
// Import required AWS SDK clients and commands for Node.js
const { DynamoDB } = require("@aws-sdk/client-dynamodb");
const { marshall, unmarshall } = require("@aws-sdk/util-dynamodb");

// Set the parameters
const params = {
  TableName: "TABLE_NAME",
  // Convert the JavaScript object defining the objects to the required DynamoDB format.The format of values
  // specifies the datatype. The following list demonstrates different datatype formatting requirements:
  // HashKey: "hashKey",
  // NumAttribute: 1,
  // BoolAttribute: true,
  // ListAttribute: [1, "two", false],
  // MapAttribute: { foo: "bar" },
  // NullAttribute: null
  ExpressionAttributeValues: marshall({
    ":s": 2,
    ":e": 9,
    ":topic": "The Return",
  }),
  // Specifies the values that define the range of the retrieved items. In this case, items in Season 2 before episode 9.
  KeyConditionExpression: "Season = :s and Episode > :e",
  // Filter that returns only episodes that meet previous criteria and have the subtitle 'The Return'
  FilterExpression: "contains (Subtitle, :topic)",
};

// Create DynamoDB document client
const client = new DynamoDB({ region: "us-east-1" });

const run = async () => {
  try {
    const data = await client.query(params);
    console.log("Success - query");
    console.log(data.Items);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ddbdoc_query_item.ts // If you prefer JavaScript, enter 'node ddbdoc_query_item.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddbdoc_query_item.ts)\.

## Deleting an Item from a Table<a name="dynamodb-example-document-client-delete"></a>

Create a Node\.js module with the file name `ddbdoc_delete_item.ts`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\. This includes the `@aws-sdk/util-dynamodb`, which is a package that provides utilities to `@aws-sdk/dynamodb`\. To access DynamoDB, create a `DynamoDB` object\. Create a JSON object containing the parameters needed to delete an item in the table, which in this example includes the name of the table and the name and value of the hashkey of the item you want to delete\. Use the `marshall` command of the `@aws-sdk/util-dynamodb` package to convert the object into a DynamoDB record\. Call the `deleteItem` method of the DynamoDB client\.

```
// Import required AWS SDK clients and commands for Node.js
const { DynamoDB } = require("@aws-sdk/client-dynamodb");
const { marshall, unmarshall } = require("@aws-sdk/util-dynamodb");

// Set the parameters
const params = {
  TableName: "TABLE_NAME",
  // Convert the key JavaScript object you are deleting to the required DynamoDB format. The format of values specifies
  // the datatype. The following list demonstrates different datatype formatting requirements:
  // HashKey: "hashKey",
  // NumAttribute: 1,
  // BoolAttribute: true,
  // ListAttribute: [1, "two", false],
  // MapAttribute: { foo: "bar" },
  // NullAttribute: null
  Key: marshall({
    primaryKey: VALUE_1, // For example, "Season: 2"
    sortKey: VALUE_2, // For example,  "Episode: 1" (only required if table has sort key)
  }),
};

// Create DynamoDB document client
const client = new DynamoDB({ region: "us-east-1" });

const run = async () => {
  try {
    const { Item } = await client.deleteItem(params);
    console.log("Success -deleted");
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ddbdoc_delete_item.ts // If you prefer JavaScript, enter 'node ddbdoc_deletem_item.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddbdoc_delete_item.ts)\.