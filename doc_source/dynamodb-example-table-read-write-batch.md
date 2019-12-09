# Reading and Writing Items in Batch in DynamoDB<a name="dynamodb-example-table-read-write-batch"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to read and write batches of items in a DynamoDB table\.

## The Scenario<a name="dynamodb-example-table-read-write-batch-scenario"></a>

In this example, you use a series of Node\.js modules to put a batch of items in a DynamoDB table as well as read a batch of items\. The code uses the SDK for JavaScript to perform batch read and write operations using these methods of the DynamoDB client class:
+ [batchGetItem](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#batchGetItem-property)
+ [batchWriteItem](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#batchWriteItem-property)

## Prerequisite Tasks<a name="dynamodb-example-table-read-write-batch-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Install Node\.js\. For more information, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.
+ Create a DynamoDB table whose items you can access\. For more information about creating a DynamoDB table, see [Creating and Using Tables in DynamoDB](dynamodb-examples-using-tables.md)\.

## Reading Items in Batch<a name="dynamodb-example-table-read-write-batch-reading"></a>

Create a Node\.js module with the file name `ddb_batchgetitem.js`\. Be sure to configure the SDK as previously shown\. To access DynamoDB, create an `AWS.DynamoDB` service object\. Create a JSON object containing the parameters needed to get a batch of items, which in this example includes the name of one or more tables from which to read, the values of keys to read in each table, and the projection expression that specifies the attributes to return\. Call the `batchGetItem` method of the DynamoDB service object\.

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

To run the example, type the following at the command line\.

```
node ddb_batchgetitem.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/dynamodb/ddb_batchgetitem.js)\.

## Writing Items in Batch<a name="dynamodb-example-table-read-write-batch-writing"></a>

Create a Node\.js module with the file name `ddb_batchwriteitem.js`\. Be sure to configure the SDK as previously shown\. To access DynamoDB, create an `AWS.DynamoDB` service object\. Create a JSON object containing the parameters needed to get a batch of items, which in this example includes the table into which you want to write items, the key\(s\) you want to write for each item, and the attributes along with their values\. Call the `batchWriteItem` method of the DynamoDB service object\.

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

To run the example, type the following at the command line\.

```
node ddb_batchwriteitem.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/dynamodb/ddb_batchwriteitem.js)\.