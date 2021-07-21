--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Bundling the browser script<a name="kinesis-page-scrolling-full"></a>

This topic describes how to bundle the browser script that captures scroll progress on the page and reports it to Kinesis and the required AWS SDK for JavaScript modules for this example\. 

If you haven't already, follow the [Prerequisite tasks](kinesis-page-scrolling-prerequisites.md) for this example to install webpack\. 

**Note**  
For information about*webpack*, see [Bundling applications with webpack](webpack.md)\.

Run the the following in the command line to bundle the JavaScript for this example into a file called `<main.js>` :

```
webpack kinesis-example.js --mode development --target web --devtool false -o main.js
```

Here is the complete browser script code for the Kinesis capturing webpage scroll progress example\.

```
// Configure Credentials to use Cognito
const { CognitoIdentityClient } = require("@aws-sdk/client-cognito-identity");
const {
  fromCognitoIdentityPool,
} = require("@aws-sdk/credential-provider-cognito-identity");
const { Kinesis, PutRecordsCommand } = require("@aws-sdk/client-kinesis");

const REGION = "REGION";
const client = new Kinesis({
  region: REGION,
  credentials: fromCognitoIdentityPool({
    client: new CognitoIdentityClient({region: REGION}),
    identityPoolId: "IDENTITY_POOL_ID" // IDENTITY_POOL_ID
  })
});
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