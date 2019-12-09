# Create and Populate a DynamoDB Table<a name="using-lambda-ddb-setup"></a>

In this task, you create and populate the Amazon DynamoDB table used by the application\.

![\[Create a DynamoDB table for the tutorial application\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/create-ddb-table.png)![\[Create a DynamoDB table for the tutorial application\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Create a DynamoDB table for the tutorial application\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

The AWS Lambda function generates three random numbers, then uses those numbers as keys to look up file names stored in an Amazon DynamoDB table\. In the `slotassets.zip` archive file are two Node\.js scripts named `ddb-table-create.js` and `ddb-table-populate.js`\. Together these files create the DynamoDB table and populate it with the names of the image files in the Amazon S3 bucket\. The Lambda function exclusively provides access to the table\. Completing this portion of the application requires you to do these things:
+ Edit the Node\.js code used to create the DynamoDB table\.
+ Run the setup script that creates the DynamoDB table\.
+ Run the setup script, which populates the DynamoDB table with data the application expects and needs\.

**To edit the Node\.js script that creates the DynamoDB table for the tutorial application**

1. Open `ddb-table-create.js` in the `slotassets` directory in a text editor\.

1. Find this line in the script\.

   `TableName: "TABLE_NAME"`

   Change *TABLE\_NAME* to one you choose\. Make a note of the table name\.

1. Save and close the file\.

**To run the Node\.js setup script that creates the DynamoDB table**
+ At the command line, type the following\.

  `node ddb-table-create.js`

## Table Creation Script<a name="using-lambda-ddb-population"></a>

The setup script `ddb-table-create.js` runs the following code\. It creates the parameters that the JSON needs to create the table\. This includes setting the table name, defining the sort key for the table \(`slotPosition`\), and defining the name of the attribute that contains the file name of one of the 16 PNG images used to display a slot wheel result\. It then calls the `createTable` method to create the table\. 

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Load credentials and set Region from JSON file
AWS.config.loadFromPath('./config.json');

// Create DynamoDB service object
var ddb = new AWS.DynamoDB({apiVersion: '2012-08-10'});

var tableParams = {
  AttributeDefinitions: [
    {
      AttributeName: 'slotPosition',
      AttributeType: 'N'
    },
    {
      AttributeName: 'imageFile',
      AttributeType: 'S'
    }
  ],
  KeySchema: [
    {
      AttributeName: 'slotPosition',
      KeyType: 'HASH'
    },
    {
      AttributeName: 'imageFile',
      KeyType: 'RANGE'
    }
  ],
  ProvisionedThroughput: {
    ReadCapacityUnits: 1,
    WriteCapacityUnits: 1
  },
  TableName: 'TABLE_NAME',
  StreamSpecification: {
    StreamEnabled: false
  }
};

ddb.createTable(tableParams, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data);

  }
});
```

Once the DynamoDB table exists, you can populate it with the items and data the application needs\. The `slotassets` directory contains t Node\.js script `ddb-table-populate.js` that automates data population for the DynamoDB table you just created\. 

**To run the Node\.js setup script that populates the DynamoDB table with data**

1. Open `ddb-table-populate.js` in a text editor\.

1. Find this line in the script\.

   `var myTable = 'TABLE_NAME';`

   Change *TABLE\_NAME* to the name of the table you created previously\.

1. Save and close the file\.

1. At the command line, type the following\.

   `node ddb-table-populate.js`

## Table Population Script<a name="dynamodb-examples-populate-tables"></a>

The setup script `ddb-table-populate.js` runs the following code\. It creates the parameters that the JSON needs to create each data item for the table\. These include a unique numeric ID value for `slotPosition` and the file name of one of the 16 PNG images of a slot wheel result for `imageFile`\. After setting the needed parameters for each possible result, the code repeatedly calls a function that executes the `putItem` method to populate items in the table\. 

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Load credentials and set Region from JSON file
AWS.config.loadFromPath('./config.json');

// Create DynamoDB service object
var ddb = new AWS.DynamoDB({apiVersion: '2012-08-10'});

var myTable = 'TABLE_NAME';

// Add the four results for spades
var params = {
  TableName: myTable,
  Item: {'slotPosition' : {N: '0'}, 'imageFile' : {S: 'spad_a.png'}
  }
};
post();

var params = {
  TableName: myTable,
  Item: {'slotPosition' : {N: '1'}, 'imageFile' : {S: 'spad_k.png'}
  }
};
post();

var params = {
  TableName: myTable,
  Item: {'slotPosition' : {N: '2'}, 'imageFile' : {S: 'spad_q.png'}
  }
};
post();

var params = {
  TableName: myTable,
  Item: {'slotPosition' : {N: '3'}, 'imageFile' : {S: 'spad_j.png'}
  }
};
post();

// Add the four results for hearts
.
.
.

// Add the four results for diamonds
.
.
.

// Add the four results for clubs
var params = {
  TableName: myTable,
  Item: {'slotPosition' : {N: '12'}, 'imageFile' : {S: 'club_a.png'}
  }
};
post();

var params = {
  TableName: myTable,
  Item: {'slotPosition' : {N: '13'}, 'imageFile' : {S: 'club_k.png'}
  }
};
post();

var params = {
  TableName: myTable,
  Item: {'slotPosition' : {N: '14'}, 'imageFile' : {S: 'club_q.png'}
  }
};
post();

var params = {
  TableName: myTable,
  Item: {'slotPosition' : {N: '15'}, 'imageFile' : {S: 'club_j.png'}
  }
};
post();


function post () {
  ddb.putItem(params, function(err, data) {
    if (err) {
      console.log("Error", err);
    } else {
      console.log("Success", data);
    }
  });
}
```

Click **next** to continue the tutorial\.