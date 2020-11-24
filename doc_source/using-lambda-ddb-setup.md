--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Create and populate a DynamoDB table<a name="using-lambda-ddb-setup"></a>

In this task, you create and populate the Amazon DynamoDB table used by the application\.

![\[Create a DynamoDB table for the tutorial application\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/create-ddb-table.png)![\[Create a DynamoDB table for the tutorial application\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Create a DynamoDB table for the tutorial application\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

The AWS Lambda function generates three random numbers, then uses those numbers as keys to look up file names stored in an Amazon DynamoDB table\. In the `src` folder in your environment there are two Node\.js scripts named `ddb-table-create.ts` and `ddb-table-populate.ts`\. Together these files create the DynamoDB table and populate it with the names of the image files in the Amazon S3 bucket\. The Lambda function exclusively provides access to the table\. Completing this portion of the application requires you to do these things:
+ Edit the Node\.js code used to create the DynamoDB table\.
+ Run the setup script that creates the DynamoDB table\.
+ Run the setup script, which populates the DynamoDB table with data the application expects and needs\.

**To edit the Node\.js script that creates the DynamoDB table for the tutorial application**

1. In a text editor open `ddb-table-create.ts` in the `src` directory\.

1. Find this line in the script\.

   `TableName: "TABLE_NAME"`

   Change *TABLE\_NAME* to one you choose\. Make a note of the table name\.

1. Save and close the file\.

**To run the Node\.js setup script that creates the DynamoDB table**
+ At the command line, type the following\.

  `node ddb-table-create.ts`

## Table creation script<a name="using-lambda-ddb-population"></a>

The setup script `ddb-table-create.ts` runs the following code\. It creates the parameters that the JSON needs to create the table\. This includes setting the table name, defining the sort key for the table \(`slotPosition`\), and defining the name of the attribute that contains the file name of one of the 16 \.png images used to display a slot wheel result\. It then calls the `createTable` method to create the table\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Load the required clients and packages
const {
  DynamoDBClient,
  CreateTableCommand
} = require("@aws-sdk/client-dynamodb");

//Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Instantiate a DynamoDB client
const ddb = new DynamoDBClient(REGION);

// Define the table schema
const tableParams = {
  AttributeDefinitions: [
    {
      AttributeName: "slotPosition",
      AttributeType: "N",
    },
  ],
  KeySchema: [
    {
      AttributeName: "slotPosition",
      KeyType: "HASH",
    },
  ],
  ProvisionedThroughput: {
    ReadCapacityUnits: 5,
    WriteCapacityUnits: 5,
  },

  TableName: "TABLE_NAME", //TABLE_NAME
  StreamSpecification: {
    StreamEnabled: false,
  },
};

const run = async () => {
  try {
    const data = await ddb.send(new CreateTableCommand(tableParams));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};

run();
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/lambda/src/ddb-table-create.ts)\.

After the DynamoDB table exists, you can populate it with the items and data the application needs\. The `slotassets` directory contains the Node\.js script `ddb-table-populate.ts` that automates data population for the DynamoDB table you just created\. 

**To run the Node\.js setup script that populates the DynamoDB table with data**

1. In a text editor open `ddb-table-populate.ts`\.

1. Find this line in the script\.

   `var myTable = 'TABLE_NAME';`

   Change *TABLE\_NAME* to the name of the table you created previously\.

1. Save and close the file\.

1. At the command line, type the following\.

   `node ddb-table-populate.ts`

## Table population script<a name="dynamodb-examples-populate-tables"></a>

The setup script `ddb-table-populate.ts` runs the following code\. It creates the parameters that the JSON needs to create each data item for the table\. These include a unique numeric ID value for `slotPosition` and the file name of one of the 16 \.png images of a slot wheel result for `imageFile`\. After setting the needed parameters for each possible result, the code repeatedly calls a function that executes the `putItem` method to populate items in the table\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Load the DynamoDB client
const { DynamoDBClient, PutItemCommand } = require("@aws-sdk/client-dynamodb");

//Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Instantiate a DynamoDB client
const ddb = new DynamoDBClient(REGION);
//Set the parameters
const myTable = "TABLE_NAME"; //TABLE_NAME

// Add the four spade results
const run = async () => {
  let params = {
    TableName: myTable,
    Item: { slotPosition: { N: "0" }, imageFile: { S: "spad_a.png" } },
  };
  await post(params);

  params = {
    TableName: myTable,
    Item: { slotPosition: { N: "1" }, imageFile: { S: "spad_k.png" } },
  };
  await post(params);

  params = {
    TableName: myTable,
    Item: { slotPosition: { N: "2" }, imageFile: { S: "spad_q.png" } },
  };
  await post(params);

  params = {
    TableName: myTable,
    Item: { slotPosition: { N: "3" }, imageFile: { S: "spad_j.png" } },
  };
  await post(params);

  // Add the four heart results
  params = {
    TableName: myTable,
    Item: { slotPosition: { N: "4" }, imageFile: { S: "hart_a.png" } },
  };
  await post(params);

  params = {
    TableName: myTable,
    Item: { slotPosition: { N: "5" }, imageFile: { S: "hart_k.png" } },
  };
  await post(params);

  params = {
    TableName: myTable,
    Item: { slotPosition: { N: "6" }, imageFile: { S: "hart_q.png" } },
  };
  await post(params);

  params = {
    TableName: myTable,
    Item: { slotPosition: { N: "7" }, imageFile: { S: "hart_j.png" } },
  };
  await post(params);

  // Add the four diamonds results
  params = {
    TableName: myTable,
    Item: { slotPosition: { N: "8" }, imageFile: { S: "diam_a.png" } },
  };
  await post(params);

  params = {
    TableName: myTable,
    Item: { slotPosition: { N: "9" }, imageFile: { S: "diam_k.png" } },
  };
  await post(params);

  params = {
    TableName: myTable,
    Item: { slotPosition: { N: "10" }, imageFile: { S: "diam_q.png" } },
  };
  await post(params);

  params = {
    TableName: myTable,
    Item: { slotPosition: { N: "11" }, imageFile: { S: "diam_j.png" } },
  };
  await post(params);

  // Add the four clubs results
  params = {
    TableName: myTable,
    Item: { slotPosition: { N: "12" }, imageFile: { S: "club_a.png" } },
  };
  await post(params);

  params = {
    TableName: myTable,
    Item: { slotPosition: { N: "13" }, imageFile: { S: "club_k.png" } },
  };
  await post(params);

  params = {
    TableName: myTable,
    Item: { slotPosition: { N: "14" }, imageFile: { S: "club_q.png" } },
  };
  await post(params);

  params = {
    TableName: myTable,
    Item: { slotPosition: { N: "15" }, imageFile: { S: "club_j.png" } },
  };
  await post(params);
};
run();

const post = async (params) => {
  try {
    const data = await ddb.send(new PutItemCommand(params));
    console.log("Success", data);
  } catch (err) {
    console.log("Error", err);
  }
};
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/lambda/src/ddb-table-populate.ts)\.

Choose **next** to continue the tutorial\.