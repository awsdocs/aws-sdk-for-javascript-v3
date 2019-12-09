# Querying and Scanning a DynamoDB Table<a name="dynamodb-example-query-scan"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to query and scan a DynamoDB table for items\.

## The Scenario<a name="dynamodb-example-table-query-scan-scenario"></a>

Querying finds items in a table or a secondary index using only primary key attribute values\. You must provide a partition key name and a value for which to search\. You can also provide a sort key name and value, and use a comparison operator to refine the search results\. Scanning finds items by checking every item in the specified table\.

In this example, you use a series of Node\.js modules to identify one or more items you want to retrieve from a DynamoDB table\. The code uses the SDK for JavaScript to query and scan tables using these methods of the DynamoDB client class:
+ [query](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#query-property)
+ [scan](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#scan-property)

## Prerequisite Tasks<a name="dynamodb-example-table-query-scan-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Install Node\.js\. For more information, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.
+ Create a DynamoDB table whose items you can access\. For more information about creating a DynamoDB table, see [Creating and Using Tables in DynamoDB](dynamodb-examples-using-tables.md)\.

## Querying a Table<a name="dynamodb-example-table-query-scan-querying"></a>

This example queries a table that contains episode information about a video series, returning the episode titles and subtitles of second season episodes past episode 9 that contain a specified phrase in their subtitle\.

Create a Node\.js module with the file name `ddb_query.js`\. Be sure to configure the SDK as previously shown\. To access DynamoDB, create an `AWS.DynamoDB` service object\. Create a JSON object containing the parameters needed to query the table, which in this example includes the table name, the `ExpressionAttributeValues` needed by the query, a `KeyConditionExpression` that uses those values to define which items the query returns, and the names of attribute values to return for each item\. Call the `query` method of the DynamoDB service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region
AWS.config.update({region: 'REGION'});

// Create DynamoDB service object
var ddb = new AWS.DynamoDB({apiVersion: '2012-08-10'});

var params = {
  ExpressionAttributeValues: {
    ':s': {N: '2'},
    ':e' : {N: '09'},
    ':topic' : {S: 'PHRASE'}
  },
  KeyConditionExpression: 'Season = :s and Episode > :e',
  ProjectionExpression: 'Episode, Title, Subtitle',
  FilterExpression: 'contains (Subtitle, :topic)',
  TableName: 'EPISODES_TABLE'
};

ddb.query(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    //console.log("Success", data.Items);
    data.Items.forEach(function(element, index, array) {
      console.log(element.Title.S + " (" + element.Subtitle.S + ")");
    });
  }
});
```

To run the example, type the following at the command line\.

```
node ddb_query.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/dynamodb/ddb_query.js)\.

## Scanning a Table<a name="dynamodb-example-table-query-scan-scanning"></a>

Create a Node\.js module with the file name `ddb_scan.js`\. Be sure to configure the SDK as previously shown\. To access DynamoDB, create an `AWS.DynamoDB` service object\. Create a JSON object containing the parameters needed to scan the table for items, which in this example includes the name of the table, the list of attribute values to return for each matching item, and an expression to filter the result set to find items containing a specified phrase\. Call the `scan` method of the DynamoDB service object\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the region
AWS.config.update({region: 'REGION'});

// Create DynamoDB service object
var ddb = new AWS.DynamoDB({apiVersion: '2012-08-10'});

var params = {
  ExpressionAttributeValues: {
    ':s': {N: '2'},
    ':e' : {N: '09'},
    ':topic' : {S: 'PHRASE'}
  },
  ProjectionExpression: 'Episode, Title, Subtitle',
  FilterExpression: 'contains (Subtitle, :topic)',
  TableName: 'EPISODES_TABLE'
};

ddb.scan(params, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    //console.log("Success", data.Items);
    data.Items.forEach(function(element, index, array) {
      console.log(element.Title.S + " (" + element.Subtitle.S + ")");
    });
  }
});
```

To run the example, type the following at the command line\.

```
node ddb_scan.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/dynamodb/ddb_scan.js)\.