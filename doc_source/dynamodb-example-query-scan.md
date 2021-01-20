--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Querying and scanning a DynamoDB table<a name="dynamodb-example-query-scan"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to query and scan a DynamoDB table for items\.

## The scenario<a name="dynamodb-example-table-query-scan-scenario"></a>

Querying finds items in a table or a secondary index using only primary key attribute values\. You must provide a partition key name and a value for which to search\. You can also provide a sort key name and value, and use a comparison operator to refine the search results\. Scanning finds items by checking every item in the specified table\.

In this example, you use a series of Node\.js modules to identify one or more items you want to retrieve from a DynamoDB table\. The code uses the SDK for JavaScript to query and scan tables using these methods of the DynamoDB client class:
+ [QueryCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#query-property)
+ [ScanCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/DynamoDB.html#scan-property)

## Prerequisite tasks<a name="dynamodb-example-table-query-scan-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/dynamodb/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so with minor adjustments these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create a DynamoDB table whose items you can access\. For more information about creating a DynamoDB table, see [Creating and using tables in DynamoDB](dynamodb-examples-using-tables.md)\.

## Querying a table<a name="dynamodb-example-table-query-scan-querying"></a>

This example queries a table that contains episode information about a video series, returning the episode titles and subtitles of second season episodes past episode 9 that contain a specified phrase in their subtitle\.

Create a Node\.js module with the file name `ddb_query.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. Create a JSON object containing the parameters needed to query the table, which in this example includes the table name, the `ExpressionAttributeValues` needed by the query, a `KeyConditionExpression` that uses those values to define which items the query returns, and the names of attribute values to return for each item\. Call the `QueryCommand` method of the DynamoDB service object\.

**Note**  
Replace *REGION* with your AWS Region\.

The primary key for the table is composed of the following attributes:
+ `Season`
+ `Episode`

You can run the code [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/QueryExample/ddb_createtable_tv.ts) to create the table that this query targets, and the code [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/QueryExample/ddb_batchwriteitem_tv.ts) to populate the table\.

```
// Import required AWS SDK clients and commands for Node.js
const { DynamoDBClient, QueryCommand } = require("@aws-sdk/client-dynamodb");

// Set the AWS Region
const REGION = "region"; //e.g. "us-east-1"

// Set the parameters
const params = {
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

// Create DynamoDB service object
const dbclient = new DynamoDBClient(REGION);
const run = async () => {
  try {
    const results = await dbclient.send(new QueryCommand(params));
    results.Items.forEach(function (element, index, array) {
      console.log(element.Title.S + " (" + element.Subtitle.S + ")");
    });
  } catch (err) {
    console.error(err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ddb_query.ts // If you prefer JavaScript, enter 'node ddb_query.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_query.ts)\.

## Scanning a table<a name="dynamodb-example-table-query-scan-scanning"></a>

Create a Node\.js module with the file name `ddb_scan.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. Create a JSON object containing the parameters needed to scan the table for items, which in this example includes the name of the table, the list of attribute values to return for each matching item, and an expression to filter the result set to find items containing a specified phrase\. Call the `ScanQuery` method of the DynamoDB service object\.

```
// Import required AWS SDK clients and commands for Node.js
const { DynamoDBClient, ScanCommand } = require("@aws-sdk/client-dynamodb");
const REGION = "REGION";

// Set parameters
const params = {
  KeyConditionExpression: "Subtitle = :topic",
  FilterExpression: "contains (Subtitle, :topic)",
  ExpressionAttributeValues: {
    ":topic": { S: "Sub" },
  },
  ProjectionExpression: "Season, Episode, Title, Subtitle",
  TableName: "EPISODES_TABLE",
};

// Create DynamoDB service object
const dbclient = new DynamoDBClient({ region: REGION });

async function run() {
  try {
    const data = await dbclient.send(new ScanCommand(params));
    data.Items.forEach(function (element, index, array) {
      console.log(element.Title.S + " (" + element.Subtitle.S + ")");
    });
  } catch (err) {
    console.log("Error", err);
  }
}
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ddb_scan.ts // If you prefer JavaScript, enter 'node ddb_scan.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_scan.ts)\.
