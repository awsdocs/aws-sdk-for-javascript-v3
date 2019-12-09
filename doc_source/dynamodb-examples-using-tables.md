# Creating and Using Tables in DynamoDB<a name="dynamodb-examples-using-tables"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create and manage tables used to store and retrieve data from DynamoDB\.

## The Scenario<a name="dynamodb-examples-using-tables-scenario"></a>

Similar to other database systems, DynamoDB stores data in tables\. A DynamoDB table is a collection of data that's organized into items that are analogous to rows\. To store or access data in DynamoDB, you create and work with tables\.

In this example, you use a series of Node\.js modules to perform basic operations with a DynamoDB table\. The code uses the SDK for JavaScript to create and work with tables by using these methods of the `AWS.DynamoDB` client class:
+ [createTable](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#createTable-property)
+ [listTables](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#listTables-property)
+ [describeTable](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#describeTable-property)
+ [deleteTable](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#deleteTable-property)

## Prerequisite Tasks<a name="dynamodb-examples-using-tables-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Install Node\.js\. For more information, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.

## Creating a Table<a name="dynamodb-examples-using-tables-creating-a-table"></a>

Create a Node\.js module with the file name `ddb_createtable.js`\. Be sure to configure the SDK as previously shown\. To access DynamoDB, create an `AWS.DynamoDB` service object\. Create a JSON object containing the parameters needed to create a table, which in this example includes the name and data type for each attribute, the key schema, the name of the table, and the units of throughput to provision\. Call the `createTable` method of the DynamoDB service object\.

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

To run the example, type the following at the command line\.

```
node ddb_createtable.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/dynamodb/ddb_createtable.js)\.

## Listing Your Tables<a name="dynamodb-examples-using-tables-listing-tables"></a>

Create a Node\.js module with the file name `ddb_listtables.js`\. Be sure to configure the SDK as previously shown\. To access DynamoDB, create an `AWS.DynamoDB` service object\. Create a JSON object containing the parameters needed to list your tables, which in this example limits the number of tables listed to 10\. Call the `listTables` method of the DynamoDB service object\.

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

To run the example, type the following at the command line\.

```
node ddb_listtables.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/dynamodb/ddb_listtables.js)\.

## Describing a Table<a name="dynamodb-examples-using-tables-describing-a-table"></a>

Create a Node\.js module with the file name `ddb_describetable.js`\. Be sure to configure the SDK as previously shown\. To access DynamoDB, create an `AWS.DynamoDB` service object\. Create a JSON object containing the parameters needed to describe a table, which in this example includes the name of the table provided as a command\-line parameter\. Call the `describeTable` method of the DynamoDB service object\.

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

To run the example, type the following at the command line\.

```
node ddb_describetable.js TABLE_NAME
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/dynamodb/ddb_describetable.js)\.

## Deleting a Table<a name="dynamodb-examples-using-tables-deleting-a-table"></a>

Create a Node\.js module with the file name `ddb_deletetable.js`\. Be sure to configure the SDK as previously shown\. To access DynamoDB, create an `AWS.DynamoDB` service object\. Create a JSON object containing the parameters needed to delete a table, which in this example includes the name of the table provided as a command\-line parameter\. Call the `deleteTable` method of the DynamoDB service object\.

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

To run the example, type the following at the command line\.

```
node ddb_deletetable.js TABLE_NAME
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/dynamodb/ddb_deletetable.js)\.