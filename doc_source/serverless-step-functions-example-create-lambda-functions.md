--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Create the Lambda functions<a name="serverless-step-functions-example-create-lambda-functions"></a>

This topic is part of a tutorial that demonstrates to to invoke Lambda functions using AWS Step Functions\. To start at the beginning of the tutorial, see [Creating AWS serverless workflows using AWS SDK for JavaScript](serverless-step-functions-example.md)\.

Use the Lambda runtime API to create the Lambda functions\. In this example, there are three workflow steps that each correspond to each Lambda function\. 

Create these Lambda function, as described in the following sections:
+ [getId Lambda function](#serverless-step-functions-example-getid) \- Used as the first step in the workflow that processes the ticket ID value\.
+ [addItem Lambda class](#serverless-step-functions-example-additem) \- Used as the second step in the workflow that assigns the ticket to an employee and stores the data in a DynamoDB database\.
+ [sendemail Lambda class](#serverless-step-functions-example-sendemail) \- Used as the third step in the workflow that use the Amazon SES to send an email message to the employee to notify them about the ticket\.

## getId Lambda function<a name="serverless-step-functions-example-getid"></a>

Create a Lambda function that returns the ticket ID value that is passed to the second step in the workflow\. 

```
exports.handler = async (event) => {
// Create a support case using the input as the case ID, then return a confirmation message
try{
    const myCaseID = event.inputCaseID;
    var myMessage = "Case " + myCaseID + ": opened...";
    var result = { Case: myCaseID, Message: myMessage };
   }
catch(err){
    console.log('Error', err);
   }
};
```

Enter the following in the command line to use webpack to bundle the file into a file named `index.js`\.

```
webpack getid.js --mode development --target node --devtool false --output-library-target umd -o index.js
```

Then compress `index.js` into a `ZIP` file name `getid.js.zip.` Upload the `ZIP` file to the Amazon S3 bucket you created in the the topic of this example\.

This code example is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/lambda-step-functions/src/lambda1/getid.js)\.

## addItem Lambda class<a name="serverless-step-functions-example-additem"></a>

Create a Lambda function that selects an employee to assign the ticket, then stores the ticket data in a DynamoDB table named **Case**\.

```
"use strict";
// Load the required clients and commands.
const { DynamoDBClient, PutItemCommand } = require("@aws-sdk/client-dynamodb");

const REGION = "eu-west-1"; //e.g. "us-east-1"
// Create the client service objects.
    const dbclient = new DynamoDBClient({ region: REGION });
exports.handler = async (event) => {
 try{
        // Helper function to send message using Amazon SNS.
        const val = event;
        //PersistCase adds an item to a DynamoDB table
        const tmp = (Math.random() <= 0.5) ? 1 : 2;
        console.log(tmp);
        if (tmp == 1) {
            const params = {
                TableName: "Case",
                Item: {
                    id: {N: val.Case},
                    empEmail: {S: "brmur@amazon.com"},
                    name: {S: "Tom Blue"}
                },
            }
            console.log('adding item for tom');
            try {
                const data = await dbclient.send(new PutItemCommand(params));
                console.log(data);
            } catch (err) {
                console.error(err);
            }
            var result = { Email: params.Item.empEmail };
            return result;
        } else {
            const params = {
                TableName: "Case",
                Item: {
                    id: {N: val.Case},
                    empEmail: {S: "brmur@amazon.com"},
                    name: {S: "Sarah White"}
                },
            }
            console.log('adding item for sarah');
            try {
                const data = await dbclient.send(new PutItemCommand(params));
                console.log(data);
            } catch (err) {
                console.error(err);
            }
            return params.Item.empEmail;
            var result = { Email: params.Item.empEmail };
        }
    }
    catch(err){
     console.log("Error" , err)
    }
}
```

Enter the following in the command line to use webpack to bundle the file into a file named `index.js`\.

```
webpack additem.js --mode development --target node --devtool false --output-library-target umd -o index.js
```

Then compress `index.js` into a `ZIP` file name `additem.js.zip.` Upload the `ZIP` file to the Amazon S3 bucket you created in the the topic of this example\.

This code example is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/lambda-step-functions/src/lambda2/additem.js)\.

### sendemail Lambda class<a name="serverless-step-functions-example-sendemail"></a>

Create a Lambda function that sends an email to notify them about the new ticket\. The email address that is passed from the second step is used\.

```
// Load the required clients and commands.
const { SESClient, SendEmailCommand } = require("@aws-sdk/client-ses");

// Set the AWS Region.
const REGION = "eu-west-1"; //e.g. "us-east-1"

// Create the client service objects.
const sesclient = new SESClient({ region: REGION });

exports.handler = async (event) => {
    // Enter a sender email address. This address must be verified.
    const sender = "Sender Name <briangermurray@gmail.com>";

    // AWS Step Functions passes the  employee's email to the event.
    // This address must be verified.
    const recepient = event.S;

    // The subject line for the email.
    const subject = "New case";

// The email body for recipients with non-HTML email clients.
    const body_text =
        "Hello,\r\n"
        + "Please check the database for new ticket assigned to you.";

// The HTML body of the email.
    const body_html = `<html><head></head><body><h1>Hello!</h1><p>Please check the database for new ticket assigned to you.</p></body></html>`;

// The character encoding for the email.
    const charset = "UTF-8";
    var params = {
        Source: sender,
        Destination: {
            ToAddresses: [
                recepient
            ],
        },
        Message: {
            Subject: {
                Data: subject,
                Charset: charset
            },
            Body: {
                Text: {
                    Data: body_text,
                    Charset: charset
                },
                Html: {
                    Data: body_html,
                    Charset: charset
                }
            }
        }
    };
    try {
        const data = await sesclient.send(new SendEmailCommand(params));
        console.log(data);
    } catch (err) {
        console.error(err);
    }

};
```

Enter the following in the command line to use webpack to bundle the file into a file named `index.js`\.

```
webpack sendemail.js --mode development --target node --devtool false --output-library-target umd -o index.js
```

Then compress `index.js` into a `ZIP` file name `sendemail.js.zip.` Upload the `ZIP` file to the Amazon S3 bucket you created in the the topic of this example\.

This code example is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/lambda-step-functions/src/lambda3/sendemail.js)\.

### Deploy the Lambda functions<a name="serverless-step-functions-example-deploy"></a>

To deploy the `getid` Lambda function: 

1. Open the Lambda console at [Amazon Web Services Console](https://us-west-2.console.aws.amazon.com/lambda/home)\.

1. Choose **Create Function**\.

1. Choose **Author from scratch**\.

1. In the **Basic** information section, enter **getid** as the name\.

1. In the **Runtime**, choose **Node\.js 14x**\.

1. Choose **Use an existing role**, and then choose **lambda\-support** \(the IAM role that you created in the \)\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

1. Choose **Create function**\.

1.  choose **Upload from** \- **Amazon S3 location**\.

1. Choose **Upload**, choose **Upload from** \- **Amazon S3 location**, and enter the **Amazon S3 link URL**\.  
![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

1. Choose **Save\.**

1. Repeat this procedure for the **additem\.js\.zip** and **sendemail\.js\.zip** to new Lambda functions\. When you finish, you will have three Lambda functions that you can reference in the Amazon States Language document\.