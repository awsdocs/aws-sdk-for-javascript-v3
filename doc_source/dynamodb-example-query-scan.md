--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Querying and scanning a DynamoDB table<a name="dynamodb-example-query-scan"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to query and scan a DynamoDB table for items\.

## The scenario<a name="dynamodb-example-table-query-scan-scenario"></a>

Querying finds items in a table or a secondary index using only primary key attribute values\. You must provide a partition key name and a value for which to search\. You can also provide a sort key name and value, and use a comparison operator to refine the search results\. Scanning finds items by checking every item in the specified table\.

In this example, you use a series of Node\.js modules to identify one or more items you want to retrieve from a DynamoDB table\. The code uses the SDK for JavaScript to query and scan tables using these methods of the DynamoDB client class:
+ [QueryCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/querycommand.html)
+ [ScanCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-dynamodb/classes/scancommand.html)

## Prerequisite tasks<a name="dynamodb-example-table-query-scan-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Set up the project environment to run these Node\.js examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/dynamodb/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create a DynamoDB table whose items you can access\. For more information about creating a DynamoDB table, see [Creating and using tables in DynamoDB](dynamodb-examples-using-tables.md)\.

**Important**  
These examples use ECMAScript6 \(ES6\)\. This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.  
However, if you prefer to use CommonJS sytax, please refer to [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)

## Querying a table<a name="dynamodb-example-table-query-scan-querying"></a>

This example queries a table that contains episode information about a video series, returning the episode titles and subtitles of second season episodes past episode 9 that contain a specified phrase in their subtitle\.

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

Create a Node\.js module with the file name `ddb_query.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client servicec object\. Create a JSON object containing the parameters needed to query the table, which in this example includes the table name, the `ExpressionAttributeValues` needed by the query, a `KeyConditionExpression` that uses those values to define which items the query returns, and the names of attribute values to return for each item\. Call the `QueryCommand` method of the DynamoDB service object\.

The primary key for the table is composed of the following attributes:
+ `Season`
+ `Episode`

You can run the code [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/QueryExample/ddb_createtable_tv.js) to create the table that this query targets, and the code [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/QueryExample/ddb_batchwriteitem_tv.js) to populate the table\.

```
// Import required AWS SDK clients and commands for Node.js
import { QueryCommand } from "@aws-sdk/client-dynamodb";
import { ddbClient } from "./libs/ddbClient.js";

// Set the parameters
export const params = {
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

export const run = async () => {
  try {
    const data = await ddbClient.send(new QueryCommand(params));
    return data;
    data.Items.forEach(function (element, index, array) {
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
node ddb_query.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_query.js)\.

## Scanning a table<a name="dynamodb-example-table-query-scan-scanning"></a>

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

Create a Node\.js module with the file name `ddb_scan.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. To access DynamoDB, create a `DynamoDB` client service object\. Create a JSON object containing the parameters needed to scan the table for items, which in this example includes the name of the table, the list of attribute values to return for each matching item, and an expression to filter the result set to find items containing a specified phrase\. Call the `ScanQuery` method of the DynamoDB service object\.

```
// Import required AWS SDK clients and commands for Node.js
import { ScanCommand } from "@aws-sdk/client-dynamodb";
import { ddbClient } from "./libs/ddbClient.js";

// Set the parameters.
export const params = {
  // Specify which items in the results are returned.
  FilterExpression: "Subtitle = :topic AND Season = :s AND Episode = :e",
  // Define the expression attribute value, which are substitutes for the values you want to compare.
  ExpressionAttributeValues: {
    ":topic": { S: "SubTitle2" },
    ":s": { N: "1" },
    ":e": { N: "2" },
  },
  // Set the projection expression, which the the attributes that you want.
  ProjectionExpression: "Season, Episode, Title, Subtitle",
  TableName: "EPISODES_TABLE",
};


export const run = async () => {
  try {
    const data = await ddbClient.send(new ScanCommand(params));
    return data;
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
node ddb_scan.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/dynamodb/src/ddb_scan.js)\.