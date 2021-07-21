--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Creating and using tables in DynamoDB<a name="dynamodb-examples-using-tables"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create and manage tables used to store and retrieve data from DynamoDB\.

## The scenario<a name="dynamodb-examples-using-tables-scenario"></a>

Similar to other database systems, DynamoDB stores data in tables\. A DynamoDB table is a collection of data that's organized into items that are analogous to rows\. To store or access data in DynamoDB, you create and work with tables\.

In this example, you use a series of Node\.js modules to perform basic operations with a DynamoDB table\. The code uses the SDK for JavaScript to create and work with tables by using these methods of the `DynamoDB` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/createtablecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/createtablecommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/listtablescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/listtablescommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/describetablecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/describetablecommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/deletetablecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/deletetablecommand.html)

## Prerequisite tasks<a name="dynamodb-examples-using-tables-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Set up the project environment to run these Node\.js examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/dynamodb/README.md)\.
+ Install SDK for JavaScript DynamoDB client\. For more information, see [What's new in Version 3](welcome.md#welcome_whats_new_v3)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples use ECMAScript6 \(ES6\)\. This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.  
However, if you prefer to use CommonJS sytax, please refer to [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)

## Creating a table<a name="dynamodb-examples-using-tables-creating-a-table"></a>

Create a `libs` directory, and create a Node\.js module with the file name `ddbClient.js`\. Copy and paste the code below into it, which creates the Amazon S3 client object\. Replace *REGION* with your AWS region\.

```
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon DynamoDB service client object.
const ddbClient = new DynamoDBClient({ region: REGION });
export { ddbClient };
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/libs/ddbClient.js)\.

Create a Node\.js module with the file name `ddb_createtable.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. Create a JSON object containing the parameters needed to create a table, which in this example includes the name and data type for each attribute, the key schema, the name of the table, and the units of throughput to provision\. Call the `CreateTableCommand` method of the DynamoDB service object\.

**Note**  
Replace *TABLE\_NAME*with the name of the table, *ATTRIBUTE\_NAME\_1* with the name of the partition key, *ATTRIBUTE\_NAME\_2* with the name of the sort key \(optionally\), and *ATTRIBUTE\_TYPE* with the type of the attribute \(for example, N \[for a number\], S \[for a string\] etc\.\)\.

**Note**  
The primary key for the table is composed of the following attributes:  
`Season`
`Episode`

```
// Import required AWS SDK clients and commands for Node.js
import { CreateTableCommand } from "@aws-sdk/client-dynamodb";
import { ddbClient } from "./libs/ddbClient.js";

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
  TableName: "TEST_TABLE", //TABLE_NAME
  StreamSpecification: {
    StreamEnabled: false,
  },
};

const run = async () => {
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

To run the example, enter the following at the command prompt\.

```
node ddb_createtable.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_createtable.js)\.

## Listing your tables<a name="dynamodb-examples-using-tables-listing-tables"></a>

Create a `libs` directory, and create a Node\.js module with the file name `ddbClient.js`\. Copy and paste the code below into it, which creates the Amazon S3 client object\. Replace *REGION* with your AWS region\.

```
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon DynamoDB service client object.
const ddbClient = new DynamoDBClient({ region: REGION });
export { ddbClient };
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/libs/ddbClient.js)\.

Create a Node\.js module with the file name `ddb_listtables.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. Create a JSON object containing the parameters needed to list your tables, which in this example limits the number of tables listed to 10\. Call the `ListTablesCommand` method of the DynamoDB service object\.

```
// Import required AWS SDK clients and commands for Node.js
import { ListTablesCommand } from "@aws-sdk/client-dynamodb";
import { ddbClient } from "./libs/ddbClient.js";

const run = async () => {
  try {
    const data = await ddbClient.send(new ListTablesCommand({}));
    console.log(data.TableNames.join("\n"));
    return data;
  } catch (err) {
    console.error(err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ddb_listtables.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_listtables.js)\.

## Describing a table<a name="dynamodb-examples-using-tables-describing-a-table"></a>

Create a `libs` directory, and create a Node\.js module with the file name `ddbClient.js`\. Copy and paste the code below into it, which creates the Amazon S3 client object\. Replace *REGION* with your AWS region\.

```
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon DynamoDB service client object.
const ddbClient = new DynamoDBClient({ region: REGION });
export { ddbClient };
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/libs/ddbClient.js)\.

Create a Node\.js module with the file name `ddb_describetable.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. Create a JSON object containing the parameters needed to describe a `DescribeTableCommand` method of the DynamoDB service object\.

**Note**  
Replace *TABLE\_NAME* with the name of the table\.

```
// Import required AWS SDK clients and commands for Node.js
import { DescribeTableCommand } from "@aws-sdk/client-dynamodb";
import { ddbClient } from "./libs/ddbClient.js";

// Set the parameters
const params = { TableName: "TABLE_NAME" }; //TABLE_NAME

const run = async () => {
  try {
    const data = await ddbClient.send(new DescribeTableCommand(params));
    console.log("Success", data.Table.KeySchema);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ddb_describetable.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_describetable.js)\.

## Deleting a table<a name="dynamodb-examples-using-tables-deleting-a-table"></a>

Create a `libs` directory, and create a Node\.js module with the file name `ddbClient.js`\. Copy and paste the code below into it, which creates the Amazon S3 client object\. Replace *REGION* with your AWS region\.

```
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon DynamoDB service client object.
const ddbClient = new DynamoDBClient({ region: REGION });
export { ddbClient };
```

This code is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/libs/ddbClient.js)\.

Create a Node\.js module with the file name `ddb_deletetable.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. Create a JSON object containing the parameters needed to delete a table, which in this example includes the name of the table provided as a command\-line parameter\. Call the `DeleteTableCommand` method of the DynamoDB service object\.

```
// Import required AWS SDK clients and commands for Node.js
import { DeleteTableCommand } from "@aws-sdk/client-dynamodb";
import { ddbClient } from "./libs/ddbClient.js";

// Set the parameters
const params = {
  TableName: "TABLE_NAME",
};

const run = async () => {
  try {
    const data = await ddbClient.send(new DeleteTableCommand(params));
    console.log("Success, table deleted", data);
    return data;
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
node ddb_deletetable.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_deletetable.js)\.