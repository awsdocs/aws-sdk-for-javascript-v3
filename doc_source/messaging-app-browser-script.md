--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Creating the browser script<a name="messaging-app-browser-script"></a>

This topic is part of a tutorial that create an AWS application that sends and retrieves messages by using the AWS SDK for JavaScript and Amazon Simple Queue Service \(Amazon SQS\)\. To start at the beginning of the tutorial, see [Creating an example messaging application](messaging-app.md)\.

In this topic, you create a the browser script for the app\. When you have created the browser script, you bundle it into a file called `main.js` as described in [Bundling the JavaScript](#messaging-app-bundle)\.

Create a file named `index.ts`\. Copy and paste the code from [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/cross-services/message-app/js/index.html) into it\.

This code is explained in the following sections: 

1. [Configuration](#messagining-app-script-config)

1. [populateChat](#messagining-app-script-onload)

1. [pushmessages](#messagining-app-script-pushmessages)

1. [purge](#messagining-app-script-purge)

## Configuration<a name="messagining-app-script-config"></a>

Import the required AWS SDK for JavaScript modules and commands\. Create clients for Amazon Lex, Amazon Comprehend, and Amazon Translate\. Replace *REGION* with AWS Region, and *IDENTITY\_POOL\_ID* with the ID of the identity pool you created in the [Create the AWS resources ](lex-bot-provision-resources.md)\. To retrieve this identity pool ID, open the identity pool in the Amazon Cognito console, choose **Edit identity pool**, and choose **Sample code** in the side menu\. The identity pool ID is shown in red text in the console\.

![\[Preparing an Amazon Cognito identity pool for the browser script\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/identity-pool-id.png)![\[Preparing an Amazon Cognito identity pool for the browser script\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Preparing an Amazon Cognito identity pool for the browser script\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

 Replace *SQS\_QUEUE\_NAME* with the name of the Amazon SQS Queue you created in the [Create the AWS resources ](messaging-app-provision-resources.md)\.

```
const { CognitoIdentityClient } = require("@aws-sdk/client-cognito-identity");
const {
  fromCognitoIdentityPool,
} = require("@aws-sdk/credential-provider-cognito-identity");
const {
  SQSClient,
  GetQueueUrlCommand,
  SendMessageCommand,
  ReceiveMessageCommand,
  PurgeQueueCommand,
} = require("@aws-sdk/client-sqs");

const REGION = "REGION"; // For example, "us-east-1".
const IdentityPoolId = "IDENTITY_POOL_ID"; // The Amazon Cognito Identity Pool ID.
const QueueName = "SQS_QUEUE_NAME"; // The Amazon SQS queue name, which must end in .fifo for this example.

const sqsClient = new SQSClient({
  region: REGION,
  credentials: fromCognitoIdentityPool({
    client: new CognitoIdentityClient({ region: REGION }),
    identityPoolId: IdentityPoolId,
  }),
});
```

## populateChat<a name="messagining-app-script-onload"></a>

The `populateChat` function onload automatically retrieves the URL for the Amazon SQS Queue, and retrieves all messages in the queue, and displays them\.

```
$(function () {
  populateChat();
});

const populateChat = async () => {
  try {
    // Set the Amazon SQS Queue parameters.
    const queueParams = {
      QueueName: QueueName,
      Attributes: {
        DelaySeconds: "60",
        MessageRetentionPeriod: "86400",
      },
    };
    // Get the Amazon SQS Queue URL.
    const data = await sqsClient.send(new GetQueueUrlCommand(queueParams));
    console.log("Success. The URL of the SQS Queue is: ", data.QueueUrl);
    // Set the parameters for retrieving the messages in the Amazon SQS Queue.
    var getMessageParams = {
      QueueUrl: data.QueueUrl,
      MaxNumberOfMessages: 10,
      MessageAttributeNames: ["All"],
      VisibilityTimeout: 20,
      WaitTimeSeconds: 20,
    };
    try {
      // Retrieve the messages from the Amazon SQS Queue.
      const data = await sqsClient.send(
        new ReceiveMessageCommand(getMessageParams)
      );
      console.log("Successfully retrieved messages", data.Messages);

      // Loop through messages for user and message body.
      var i;
      for (i = 0; i < data.Messages.length; i++) {
        const name = data.Messages[i].MessageAttributes.Name.StringValue;
        const body = data.Messages[i].Body;
        // Create the HTML for the message.
        var userText = body + "<br><br><b>" + name;
        var myTextNode = $("#base").clone();
        myTextNode.text(userText);
        var image_url;
        var n = name.localeCompare("Scott");
        if (n == 0) image_url = "./images/av1.png";
        else image_url = "./images/av2.png";
        var images_div =
          '<img src="' +
          image_url +
          '" alt="Avatar" class="right" style=""width:100%;"">';
        myTextNode.html(userText);
        myTextNode.append(images_div);

        // Add the message to the GUI.
        $("#messages").append(myTextNode);
      }
    } catch (err) {
      console.log("Error loading messages: ", err);
    }
  } catch (err) {
    console.log("Error retrieving SQS queue URL: ", err);
  }
};
```

## Push messages<a name="messagining-app-script-pushmessages"></a>

The user selects their name and enters their message, and submits the message, which initiates the `pushMessage` function\. `pushMessage` retrieves the Amazon SQS Queue Url, and then sends a message with a unique message ID value \(a GUID\) the message text, and the user to the Amazon SQS Queue\. It then retrieves all the messages from the Amazon SQS Queue and displays them\.

```
const pushMessage = async () => {
  // Get and convert user and message input.
  var user = $("#username").val();
  var message = $("#textarea").val();

  // Create random deduplication ID.
  var dt = new Date().getTime();
  var uuid = "xxxxxxxx-xxxx-4xxx-yxxx-xxxxxxxxxxxx".replace(/[xy]/g, function (
    c
  ) {
    var r = (dt + Math.random() * 16) % 16 | 0;
    dt = Math.floor(dt / 16);
    return (c == "x" ? r : (r & 0x3) | 0x8).toString(16);
  });

  try {
    // Set the Amazon SQS Queue parameters.
    const queueParams = {
      QueueName: QueueName,
      Attributes: {
        DelaySeconds: "60",
        MessageRetentionPeriod: "86400",
      },
    };
    const data = await sqsClient.send(new GetQueueUrlCommand(queueParams));
    console.log("Success. The URL of the SQS Queue is: ", data.QueueUrl);
    // Set the parameters for the message.
    var messageParams = {
      MessageAttributes: {
        Name: {
          DataType: "String",
          StringValue: user,
        },
      },
      MessageBody: message,
      MessageDeduplicationId: uuid,
      MessageGroupId: "GroupA",
      QueueUrl: data.QueueUrl,
    };
    const result = await sqsClient.send(new SendMessageCommand(messageParams));
    console.log("Success", result.MessageId);

    // Set the parameters for retrieving all messages in the SQS queue.
    var getMessageParams = {
      QueueUrl: data.QueueUrl,
      MaxNumberOfMessages: 10,
      MessageAttributeNames: ["All"],
      VisibilityTimeout: 20,
      WaitTimeSeconds: 20,
    };

    // Retrieve messages from SQS Queue.
    const final = await sqsClient.send(
      new ReceiveMessageCommand(getMessageParams)
    );
    console.log("Successfully retrieved", final.Messages);
    $("#messages").empty();
    // Loop through messages for user and message body.
    var i;
    for (i = 0; i < final.Messages.length; i++) {
      const name = final.Messages[i].MessageAttributes.Name.StringValue;
      const body = final.Messages[i].Body;
      // Create the HTML for the message.
      var userText = body + "<br><br><b>" + name;
      var myTextNode = $("#base").clone();
      myTextNode.text(userText);
      var image_url;
      var n = name.localeCompare("Scott");
      if (n == 0) image_url = "./images/av1.png";
      else image_url = "./images/av2.png";
      var images_div =
        '<img src="' +
        image_url +
        '" alt="Avatar" class="right" style=""width:100%;"">';
      myTextNode.html(userText);
      myTextNode.append(images_div);
      // Add the HTML to the GUI.
      $("#messages").append(myTextNode);
    }
  } catch (err) {
    console.log("Error", err);
  }
};
// Make the function available to the browser window.
window.pushMessage = pushMessage;
```

## Purge messages<a name="messagining-app-script-purge"></a>

`purge` deletes the messages from the Amazon SQS Queue and from the user interface\. 

```
// Delete the message from the Amazon SQS queue.
const purge = async () => {
  try {
    // Set the Amazon SQS Queue parameters.
    const queueParams = {
      QueueName: QueueName,
      Attributes: {
        DelaySeconds: "60",
        MessageRetentionPeriod: "86400",
      },
    };
    // Get the Amazon SQS Queue URL.
    const data = await sqsClient.send(new GetQueueUrlCommand(queueParams));
    console.log("Success", data.QueueUrl);
    // Delete all the messages in the Amazon SQS Queue.
    const result = await sqsClient.send(
      new PurgeQueueCommand({ QueueUrl: data.QueueUrl })
    );
    // Delete all the messages from the GUI.
    $("#messages").empty();
    console.log("Success. All messages deleted.", data);
  } catch (err) {
    console.log("Error", err);
  }
};

// Make the function available to the browser window.
window.purge = purge;
```

### Bundling the JavaScript<a name="messaging-app-bundle"></a>

This comlete browser script code is available [here on GitHub\.](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/cross-services/message-app/js/index.html)\.

Now use webpack to bundle the `index.ts` and AWS SDK for JavaScript modules into a single file, `main.js`\.

1. If you haven't already, follow the [Prerequisites](messaging-app-prerequisites.md) for this example to install webpack\. 
**Note**  
For information about*webpack*, see [Bundling applications with webpack](webpack.md)\.

1. Run the the following in the command line to bundle the JavaScript for this example into a file called `<index.js>` :

   ```
   webpack index.ts --mode development --libraryTarget commonjs2 --target web --devtool false -o main.js
   ```