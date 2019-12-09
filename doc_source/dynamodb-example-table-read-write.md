# Reading and Writing a Single Item in DynamoDB<a name="dynamodb-example-table-read-write"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to add an item in a DynamoDB table\.
+ How to retrieve, an item in a DynamoDB table\.
+ How to delete an item in a DynamoDB table\.

## The Scenario<a name="dynamodb-example-table-read-write-scenario"></a>

In this example, you use a series of Node\.js modules to read and write one item in a DynamoDB table by using these methods of the `AWS.DynamoDB` client class:
+ [putItem](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#putItem-property)
+ [getItem](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#getItem-property)
+ [deleteItem](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#deleteItem-property)

## Prerequisite Tasks<a name="dynamodb-example-table-read-write-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Install Node\.js\. For more information, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.
+ Create a DynamoDB table whose items you can access\. For more information about creating a DynamoDB table, see [Creating and Using Tables in DynamoDB](dynamodb-examples-using-tables.md)\.

## Writing an Item<a name="dynamodb-example-table-read-write-writing-an-item"></a>

Create a Node\.js module with the file name `ddb_putitem.js`\. Be sure to configure the SDK as previously shown\. To access DynamoDB, create an `AWS.DynamoDB` service object\. Create a JSON object containing the parameters needed to add an item, which in this example includes the name of the table and a map that defines the attributes to set and the values for each attribute\. Call the `putItem` method of the DynamoDB service object\.

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

To run the example, type the following at the command line\.

```
node ddb_putitem.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/dynamodb/ddb_putitem.js)\.

## Getting an Item<a name="dynamodb-example-table-read-write-getting-an-item"></a>

Create a Node\.js module with the file name `ddb_getitem.js`\. Be sure to configure the SDK as previously shown\. To access DynamoDB, create an `AWS.DynamoDB` service object\. To identify the item to get, you must provide the value of the primary key for that item in the table\. By default, the `getItem` method returns all the attribute values defined for the item\. To get only a subset of all possible attribute values, specify a projection expression\.

Create a JSON object containing the parameters needed to get an item, which in this example includes the name of the table, the name and value of the key for the item you're getting, and a projection expression that identifies the item attribute you want to retrieve\. Call the `getItem` method of the DynamoDB service object\.

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

To run the example, type the following at the command line\.

```
node ddb_getitem.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/dynamodb/ddb_getitem.js)\.

## Deleting an Item<a name="dynamodb-example-table-read-write-deleting-an-item"></a>

Create a Node\.js module with the file name `ddb_deleteitem.js`\. Be sure to configure the SDK as previously shown\. To access DynamoDB, create an `AWS.DynamoDB` service object\. Create a JSON object containing the parameters needed to delete an item, which in this example includes the name of the table and both the key name and value for the item you're deleting\. Call the `deleteItem` method of the DynamoDB service object\.

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

To run the example, type the following at the command line\.

```
node ddb_deleteitem.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/dynamodb/ddb_deleteitem.js)\.