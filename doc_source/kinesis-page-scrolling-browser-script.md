--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Create the browser script<a name="kinesis-page-scrolling-browser-script"></a>

## Configuring the SDK<a name="kinesis-page-scrolling-configure-sdk"></a>

Create a `libs` directory, and create a Node\.js module with the file name `kinesisClient.js`\. Copy and paste the code below into it, which creates the Kinesis client object\. Replace *REGION* with your AWS Region\. Replace *IDENTITY\_POOL\_ID* with the Amazon Cognito identity pool id you created in [Create the AWS resources ](kinesis-page-scrolling-provision-resources.md)\.

```
// Import the required AWS SDK clients and commands for Node.js.
const {CognitoIdentityClient} = require("@aws-sdk/client-cognito-identity");
const {
  fromCognitoIdentityPool,
} = require("@aws-sdk/credential-provider-cognito-identity");
const { KinesisClient, PutRecordsCommand } = require("@aws-sdk/client-kinesis");

// Configure Credentials to use Cognito
const REGION = "REGION";
const kinesisClient = new KinesisClient({
  region: REGION,
  credentials: fromCognitoIdentityPool({
    client: new CognitoIdentityClient({region: REGION}),
    identityPoolId: "IDENTITY_POOL_ID" // IDENTITY_POOL_ID
  })
});
export {kinesisClient}
```

You can find this code [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/kinesis/src/libs/kinesisClient.js)\.

## Creating Scroll Records<a name="kinesis-page-scrolling-create-records"></a>

Scroll progress is calculated using the `scrollHeight` and `scrollTop` properties of the `<div>` containing the content of the blog post\. Each scroll record is created in an event listener function for the `scroll` event and then added to an array of records for periodic submission to Kinesis\. Replace *PARTITION\_KEY* with a paritition key, which must be a string\. For more information about partition strings, see [PutRecord](https://docs.aws.amazon.com/kinesis/latest/APIReference/API_PutRecord.html) in the *Amazon Kinesis Data Analytics developer guide*\.

The following code snippet shows this step\. \(See [Bundling the browser script](kinesis-page-scrolling-full.md) for the full example\.\)

```
import { PutRecordsCommand } from "@aws-sdk/client-kinesis";
import { kinesisClient } from "./libs/kinesisClient.js";
// Get the ID of the web page element.
var blogContent = document.getElementById('BlogContent');

// Get scrollable height.
var scrollableHeight = blogContent.clientHeight;

var recordData = [];
var TID = null;
blogContent.addEventListener('scroll', function(event) {
  console.log('scrolled');
  clearTimeout(TID);
  // Prevent creating a record while a user is actively scrolling.
  TID = setTimeout(function() {
    // Calculate the percentage.
    var scrollableElement = event.target;
    var scrollHeight = scrollableElement.scrollHeight;
    var scrollTop = scrollableElement.scrollTop;

    var scrollTopPercentage = Math.round((scrollTop / scrollHeight) * 100);
    var scrollBottomPercentage = Math.round(((scrollTop + scrollableHeight) / scrollHeight) * 100);

    // Create the Amazon Kinesis record.
    var record = {
      Data: JSON.stringify({
        blog: window.location.href,
        scrollTopPercentage: scrollTopPercentage,
        scrollBottomPercentage: scrollBottomPercentage,
        time: new Date()
      }),
      PartitionKey: 'PARTITION_KEY' // Must be a string.
    };
    recordData.push(record);
  }, 100);
});
```

## Submitting Records to Kinesis<a name="kinesis-page-scrolling-submit-records"></a>

Once each second, if there are records in the array, those pending records are sent to Kinesis\. Replace *STREAM\_NAME* with the name of the stream you created in the [Create the AWS resources ](kinesis-page-scrolling-provision-resources.md) section of this example\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

The following code snippet shows this step\. \(See [Bundling the browser script](kinesis-page-scrolling-full.md) for the full example\.\)

```
// Helper function to upload data to Amazon Kinesis.
const uploadData = async () => {
  try {
    const data = await client.send(new PutRecordsCommand({
      Records: recordData,
      StreamName: 'STREAM_NAME'
    }));
    console.log('data', data);
    console.log("Kinesis updated", data);
  } catch (err) {
    console.log("Error", err);
  }
};
// Run uploadData every second if data exists.
setInterval(function() {
  if (!recordData.length) {
    return;
  }
  uploadData();
  // clear record data
  recordData = [];
}, 1000);
```