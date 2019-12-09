# Using the DynamoDB Document Client<a name="dynamodb-example-document-client"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to access a DynamoDB table using the document client\.

## The Scenario<a name="dynamodb-example-document-client-scenario"></a>

The DynamoDB document client simplifies working with items by abstracting the notion of attribute values\. This abstraction annotates native JavaScript types supplied as input parameters, as well as converts annotated response data to native JavaScript types\.

For more information on the DynamoDB Document Client class, see [AWS\.DynamoDB\.DocumentClient](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html) in the API Reference\. For more information on programming with Amazon DynamoDB, see [Programming with DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Programming.html) in the *Amazon DynamoDB Developer Guide*\.

In this example, you use a series of Node\.js modules to perform basic operations on a DynamoDB table using the document client\. The code uses the SDK for JavaScript to query and scan tables using these methods of the DynamoDB Document Client class:
+ [get](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html#get-property)
+ [put](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html#put-property)
+ [update](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html#update-property)
+ [query](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html#query-property)
+ [delete](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB/DocumentClient.html#delete-property)

## Prerequisite Tasks<a name="dynamodb-example-document-client-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Install Node\.js\. For more information, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.
+ Create a DynamoDB table whose items you can access\. For more information about creating a DynamoDB table using the SDK for JavaScript, see [Creating and Using Tables in DynamoDB](dynamodb-examples-using-tables.md)\. You can also use the [DynamoDB console](https://console.aws.amazon.com/dynamodb/) to create a table\.

## Getting an Item from a Table<a name="dynamodb-example-document-client-get"></a>

Create a Node\.js module with the file name `ddbdoc_get.js`\. Be sure to configure the SDK as previously shown\. To access DynamoDB, create an `AWS.DynamoDB.DocumentClient` object\. Create a JSON object containing the parameters needed get an item from the table, which in this example includes the name of the table, the name of the hash key in that table, and the value of the hash key for the item you want to get\. Call the `get` method of the DynamoDB `DocumentClient` class\.

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

To run the example, type the following at the command line\.

```
node ddbdoc_get.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/dynamodb/ddbdoc_get.js)\.

## Putting an Item in a Table<a name="dynamodb-example-document-client-put"></a>

Create a Node\.js module with the file name `ddbdoc_put.js`\. Be sure to configure the SDK as previously shown\. To access DynamoDB, create an `AWS.DynamoDB.DocumentClient` object\. Create a JSON object containing the parameters needed to write an item to the table, which in this example includes the name of the table and a description of the item to add or update that includes the hashkey and value as well as names and values for attributes to set on the item\. Call the `put` method of the DynamoDB document client\.

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

To run the example, type the following at the command line\.

```
node ddbdoc_put.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/dynamodb/ddbdoc_put.js)\.

## Updating an Item in a Table<a name="dynamodb-example-document-client-update"></a>

Create a Node\.js module with the file name `ddbdoc_update.js`\. Be sure to configure the SDK as previously shown\. To access DynamoDB, create an `AWS.DynamoDB.DocumentClient` object\. Create a JSON object containing the parameters needed to write an item to the table, which in this example includes the name of the table, the key of the item to update, a set of `UpdateExpressions` that define the attributes of the item to update with tokens you assign values to in the `ExpressionAttributeValues` parameters\. Call the `update` method of the DynamoDB document client\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region
AWS.config.update({region: 'REGION'});

// Create DynamoDB document client
var docClient = new AWS.DynamoDB.DocumentClient({apiVersion: '2012-08-10'});

// Create variables to hold numeric key values
var season = SEASON_NUMBER;
var episode = EPISODES_NUMBER;

var params = {
  TableName: 'EPISODES_TABLE',
  Key: {
    'Season' : season,
    'Episode' : episode
  },
  UpdateExpression: 'set Title = :t, Subtitle = :s',
  ExpressionAttributeValues: {
    ':t' : 'NEW_TITLE',
    ':s' : 'NEW_SUBTITLE'
  }
};

docClient.update(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);
  }
});
```

To run the example, type the following at the command line\.

```
node ddbdoc_update.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/dynamodb/ddbdoc_update.js)\.

## Querying a Table<a name="dynamodb-example-document-client-query"></a>

This example queries a table that contains episode information about a video series, returning the episode titles and subtitles of second season episodes past episode 9 that contain a specified phrase in their subtitle\.

Create a Node\.js module with the file name `ddbdoc_query.js`\. Be sure to configure the SDK as previously shown\. To access DynamoDB, create an `AWS.DynamoDB.DocumentClient` object\. Create a JSON object containing the parameters needed to query the table, which in this example includes the table name, the `ExpressionAttributeValues` needed by the query, and a `KeyConditionExpression` that uses those values to define which items the query returns\. Call the `query` method of the DynamoDB document client\.

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

To run the example, type the following at the command line\.

```
node ddbdoc_query.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/dynamodb/ddbdoc_query.js)\.

## Deleting an Item from a Table<a name="dynamodb-example-document-client-delete"></a>

Create a Node\.js module with the file name `ddbdoc_delete.js`\. Be sure to configure the SDK as previously shown\. To access DynamoDB, create an `AWS.DynamoDB.DocumentClient` object\. Create a JSON object containing the parameters needed to delete an item in the table, which in this example includes the name of the table as well as a the name and value of the hashkey of the item you want to delete\. Call the `delete` method of the DynamoDB document client\.

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

To run the example, type the following at the command line\.

```
node ddbdoc_delete.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/dynamodb/ddbdoc_delete.js)\.