--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Using the DynamoDB Document Client<a name="dynamodb-example-dynamodb-utilities"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to access a DynamoDB table using the DynamoDB utilities\.

## The Scenario<a name="dynamodb-example-document-client-scenario"></a>

The DynamoDB Document client simplifies working with items by abstracting the notion of attribute values\. This abstraction annotates native JavaScript types supplied as input parameters, and converts annotated response data to native JavaScript types\.

For more information about the DynamoDB Document Client, see [@aws\-sdk/lib\-dynamodb README](https://github.com/aws/aws-sdk-js-v3/tree/main/lib/lib-dynamodb) on GitHub\. For more information about programming with Amazon DynamoDB, see [Programming with DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Programming.html) in the *Amazon DynamoDB Developer Guide*\.

In this example, you use a series of Node\.js modules to perform basic operations on a DynamoDB table using DynamoDB utilities\. The code uses the SDK for JavaScript to query and scan tables using these methods of the DynamoDB class:
+ getItem
+ putItem
+ updateItem
+ query
+ deleteItem

For more information on configuring the DynamoDB document client, see [@aws\-sdk/lib\-dynamodb](https://github.com/aws/aws-sdk-js-v3/tree/main/lib/lib-dynamodb)\.

## Prerequisite Tasks<a name="dynamodb-example-document-client-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Set up the project environment to run these Node\.js examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/dynamodb/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create a DynamoDB table whose items you can access\. For more information about creating a DynamoDB table using the SDK for JavaScript, see [Creating and using tables in DynamoDB](dynamodb-examples-using-tables.md)\. You can also use the [DynamoDB console](https://console.aws.amazon.com/dynamodb/) to create a table\.

**Important**  
These examples use ECMAScript6 \(ES6\)\. This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.  
However, if you prefer to use CommonJS syntax, please refer to [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)

## Getting an Item from a Table<a name="dynamodb-example-document-client-get"></a>

Create a `libs` directory, and create a Node\.js module with the file name `ddbDocClient.js`\. Copy and paste the code below into it, which creates the DynamoDB document client object\. Replace *REGION* with your AWS region\.

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

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/libs/ddbDocClient.js)\.

In the `libs` directory create a Node\.js module with the file name `ddbClient.js`\. Copy and paste the code below into it, which creates the DynamoDB client object\. Replace *REGION* with your AWS region\.

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

Create a Node\.js module with the file name `ddbdoc_get_item.js`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\. This includes the `@aws-sdk/lib-dynamodb`, a library package that provides document client functionality to `@aws-sdk/client-dynamodb`\. Next, set the configuration as shown below for marshalling and unmarshalling \- as an optional second parameter \- during creation of document client\. Next, create the clients\. Now create a JSON object containing the parameters needed get an item from the table, which in this example includes the name of the table, the name of the hash key in that table, and the value of the hash key for the item you want to get\. Call the `GetCommand` method of the DynamoDB Document client\.

```
import { GetCommand } from "@aws-sdk/lib-dynamodb";
import { ddbDocClient } from "./libs/ddbDocClient";

// Set the parameters.
export const params = {
  TableName: "TABLE_NAME",
  /*
  Convert the key JavaScript object you are retrieving to the
  required Amazon DynamoDB record. The format of values specifies
  the datatype. The following list demonstrates different
  datatype formatting requirements:
  String: "String",
  NumAttribute: 1,
  BoolAttribute: true,
  ListAttribute: [1, "two", false],
  MapAttribute: { foo: "bar" },
  NullAttribute: null
   */
  Key: {
    primaryKey: "VALUE", // For example, 'Season': 2.
    sortKey: "VALUE", // For example,  'Episode': 1; (only required if table has sort key).
  },
};

export const run = async () => {
  try {
    const data = await ddbDocClient.send(new GetCommand(params));
    console.log("Success :", data);
    // console.log("Success :", data.Item);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ddbdoc_get_item.js // To use JavaScript, enter 'node ddbdoc_get_item.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddbdoc_get_item.js)\.

## Putting an Item in a Table<a name="dynamodb-example-document-client-put"></a>

Create a `libs` directory, and create a Node\.js module with the file name `ddbDocClient.js`\. Copy and paste the code below into it, which creates the DynamoDB document client object\. Replace *REGION* with your AWS region\.

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

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/libs/ddbDocClient.js)\.

In the `libs` directory create a Node\.js module with the file name `ddbClient.js`\. Copy and paste the code below into it, which creates the DynamoDB client object\. Replace *REGION* with your AWS region\.

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

Create a Node\.js module with the file name `ddbdoc_put_item.js`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\. This includes the `@aws-sdk/lib-dynamodb`, a library package that provides document client functionality to `@aws-sdk/client-dynamodb`\. Next, set the configuration as shown below for marshalling and unmarshalling \- as an optional second parameter \- during creation of document client\. Next, create the clients\. Create a JSON object containing the parameters needed to write an item to the table, which in this example includes the name of the table and a description of the item to add or update that includes the hashkey and value and names and values for attributes to set on the item\. Call the `PutCommand` method of the DynamoDB Document client\.

```
import { PutCommand } from "@aws-sdk/lib-dynamodb";
import { ddbDocClient } from "./libs/ddbDocClient";

// Set the parameters.
export const params = {
  TableName: "TABLE_NAME",
  /*
    Convert the key JavaScript object you are adding to the
    required Amazon DynamoDB record. The format of values specifies
    the datatype. The following list demonstrates different
    datatype formatting requirements:
    String: "String",
    NumAttribute: 1,
    BoolAttribute: true,
    ListAttribute: [1, "two", false],
    MapAttribute: { foo: "bar" },
    NullAttribute: null
     */
  Item: {
    primaryKey: "VALUE_1", // For example, 'Season': 2
    sortKey: "VALUE_2", // For example,  'Episode': 2 (only required if table has sort key)
    NEW_ATTRIBUTE_1: "NEW_ATTRIBUTE_1_VALUE", //For example 'Title': 'The Beginning'
  },
};

export const run = async () => {
  try {
    const data = await ddbDocClient.send(new PutCommand(params));
    console.log("Success - item added or updated", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ddbdoc_put_item.js // To use JavaScript, enter 'node ddbdoc_put_item.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddbdoc_put_item.js)\.

## Updating an Item in a Table<a name="dynamodb-example-document-client-update"></a>

Create a `libs` directory, and create a Node\.js module with the file name `ddbDocClient.js`\. Copy and paste the code below into it, which creates the DynamoDB document client object\. Replace *REGION* with your AWS region\.

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

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/libs/ddbDocClient.js)\.

In the `libs` directory create a Node\.js module with the file name `ddbClient.js`\. Copy and paste the code below into it, which creates the DynamoDB client object\. Replace *REGION* with your AWS region\.

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

Create a Node\.js module with the file name `ddbdoc_update_item.js`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\. This includes the `@aws-sdk/lib-dynamodb`, a library package that provides document client functionality to `@aws-sdk/client-dynamodb`\. Next, set the configuration as shown below for marshalling and unmarshalling \- as an optional second parameter \- during creation of document client\. Next, create the clients\. Create a JSON object containing the parameters needed to write an item to the table, which in this example includes the name of the table, the key of the item to update, a set of `UpdateExpressions` that define the attributes of the item to update with tokens you assign values to in the `ExpressionAttributeValues` parameters\.Call the `UpdateCommand` method of the DynamoDB Document client\.

```
import { UpdateCommand } from "@aws-sdk/lib-dynamodb";
import { ddbDocClient } from "./libs/ddbDocClient";

// Set the parameters
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
    primaryKey: "VALUE_1", // For example, 'Season': 2.
    sortKey: "VALUE_2", // For example,  'Episode': 1; (only required if table has sort key).
  },
  // Define expressions for the new or updated attributes
  UpdateExpression: "set ATTRIBUTE_NAME_1 = :t, ATTRIBUTE_NAME_2 = :s", // For example, "'set Title = :t, Subtitle = :s'"
  ExpressionAttributeValues: {
    ":t": "NEW_ATTRIBUTE_VALUE_1", // For example ':t' : 'NEW_TITLE'
    ":s": "NEW_ATTRIBUTE_VALUE_2", // For example ':s' : 'NEW_SUBTITLE'
  },
  ReturnValues: "ALL_NEW"
};

export const run = async () => {
  try {
    const data = await ddbDocClient.send(new UpdateCommand(params));
    console.log("Success - item added or updated", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ddbdoc_update_item.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddbdoc_update_item.js)\.

## Querying a Table<a name="dynamodb-example-document-client-query"></a>

Create a `libs` directory, and create a Node\.js module with the file name `ddbDocClient.js`\. Copy and paste the code below into it, which creates the DynamoDB document client object\. Replace *REGION* with your AWS region\.

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

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/libs/ddbDocClient.js)\.

In the `libs` directory create a Node\.js module with the file name `ddbClient.js`\. Copy and paste the code below into it, which creates the DynamoDB client object\. Replace *REGION* with your AWS region\.

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

This example queries a table that contains episode information about a video series, returning the episode titles and subtitles of second season episodes past episode 9 that contain a specified phrase in their subtitle\.

Create a Node\.js module with the file name `ddbdoc_query_item.js`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\. This includes the `@aws-sdk/lib-dynamodb`, a library package that provides document client functionality to `@aws-sdk/client-dynamodb`\. Create a JSON object containing the parameters needed to query the table, which in this example includes the table name, the `ExpressionAttributeValues` needed by the query, and a `KeyConditionExpression` that uses those values to define which items the query returns\. Call the `QueryCommand` method of the DynamoDB client\.

```
import { QueryCommand } from "@aws-sdk/lib-dynamodb";
import { ddbDocClient } from "./libs/ddbDocClient";

// Set the parameters
export const params = {
  TableName: "TABLE_NAME",
  /*
  Convert the JavaScript object defining the objects to the required
  Amazon DynamoDB record. The format of values specifies the datatype. The
  following list demonstrates different datatype formatting requirements:
  String: "String",
  NumAttribute: 1,
  BoolAttribute: true,
  ListAttribute: [1, "two", false],
  MapAttribute: { foo: "bar" },
  NullAttribute: null
   */
  ExpressionAttributeValues: {
    ":s": 1,
    ":e": 1,
    ":topic": "Title2",
  },
  // Specifies the values that define the range of the retrieved items. In this case, items in Season 2 before episode 9.
  KeyConditionExpression: "Season = :s and Episode > :e",
  // Filter that returns only episodes that meet previous criteria and have the subtitle 'The Return'
  FilterExpression: "contains (Subtitle, :topic)",
};

export const run = async () => {
  try {
    const data = await ddbDocClient.send(new QueryCommand(params));
    console.log("Success. Item details: ", data);
    // console.log("Success. Item details: ", data.Items);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ddbdoc_query_item.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddbdoc_query_item.js)\.

## Deleting an Item from a Table<a name="dynamodb-example-document-client-delete"></a>

Create a `libs` directory, and create a Node\.js module with the file name `ddbDocClient.js`\. Copy and paste the code below into it, which creates the DynamoDB document client object\. Replace *REGION* with your AWS region\.

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

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/libs/ddbDocClient.js)\.

In the `libs` directory create a Node\.js module with the file name `ddbClient.js`\. Copy and paste the code below into it, which creates the DynamoDB client object\. Replace *REGION* with your AWS region\.

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

Create a Node\.js module with the file name `ddbdoc_delete_item.js`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\. This includes the `@aws-sdk/lib-dynamodb`, a library package that provides document client functionality to `@aws-sdk/client-dynamodb`\. Next, set the configuration as shown below for marshalling and unmarshalling \- as an optional second parameter \- during creation of document client\. Next, create the clients\. To access DynamoDB, create a `DynamoDB` object\. Create a JSON object containing the parameters needed to delete an item in the table, which in this example includes the name of the table and the name and value of the hashkey of the item you want to delete\. Call the `DeleteCommand` method of the DynamoDB client\.

```
import { DeleteCommand } from "@aws-sdk/lib-dynamodb";
import { ddbDocClient } from "./libs/ddbDocClient";

// Set the parameters
export const params = {
  TableName: "TABLE_NAME",
  /*
  Convert the key JavaScript object you are deleting to the
  required Amazon DynamoDB record. The format of values specifies
  the datatype. The following list demonstrates different
  datatype formatting requirements:
  String: "String",
  NumAttribute: 1,
  BoolAttribute: true,
  ListAttribute: [1, "two", false],
  MapAttribute: { foo: "bar" },
  NullAttribute: null
   */
  Key: {
    primaryKey: "VALUE_1", // For example, 'Season': 2.
    sortKey: "VALUE_2", // For example,  'Episode': 1; (only required if table has sort key).
  },
};

export const run = async () => {
  try {
    const data = await ddbDocClient.send(new DeleteCommand(params));
    console.log("Success - item deleted");
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ddbdoc_delete_item.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddbdoc_delete_item.js)\.