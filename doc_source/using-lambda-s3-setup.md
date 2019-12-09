# Create an Amazon S3 Bucket Configured as a Static Website<a name="using-lambda-s3-setup"></a>

In this task, you create and prepare the Amazon S3 bucket used by the application\.

![\[JavaScript running in a browser that creates an Amazon S3 bucket\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/create-s3-bucket.png)![\[JavaScript running in a browser that creates an Amazon S3 bucket\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[JavaScript running in a browser that creates an Amazon S3 bucket\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

For this application, the first thing you need to create is an Amazon S3 bucket to store all the browser assets\. These include the HTML file, all graphics files, and the CSS file\. The bucket is configured as a static website so that it also serves the application from the bucket's URL\. 

The `slotassets` directory contains the Node\.js script `s3-bucket-setup.js` that creates the Amazon S3 bucket and sets the website configuration\. 

**To create and configure the Amazon S3 bucket that the tutorial application uses**
+ At the command line, type the following command, where *BUCKET\_NAME* is the name for the bucket:

  `node s3-bucket-setup.js BUCKET_NAME`

  The bucket name must be globally unique\. If the command succeeds, the script displays the URL of the new bucket\. Make a note of this URL because you'll use it later\.

## Setup Script<a name="using-lambda-s3-script"></a>

The setup script runs the following code\. It takes the command\-line argument that is passed in and uses it to specify the bucket name and the parameter that makes the bucket publicly readable\. It then sets up the parameters used to enable the bucket to act as a static website host\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Load credentials and set Region from JSON file
AWS.config.loadFromPath('./config.json');

// Create S3 service object
s3 = new AWS.S3({apiVersion: '2006-03-01'});

// Create params JSON for S3.createBucket
var bucketParams = {
  Bucket : process.argv[2],
  ACL : 'public-read'
};

// Create params JSON for S3.setBucketWebsite
var staticHostParams = {
  Bucket: process.argv[2],
  WebsiteConfiguration: {
  ErrorDocument: {
    Key: 'error.html'
  },
  IndexDocument: {
    Suffix: 'index.html'
  },
  }
};

// Call S3 to create the bucket
s3.createBucket(bucketParams, function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Bucket URL is ", data.Location);
    // Set the new policy on the newly created bucket
    s3.putBucketWebsite(staticHostParams, function(err, data) {
      if (err) {
        // Display error message
        console.log("Error", err);
      } else {
        // Update the displayed policy for the selected bucket
        console.log("Success", data);
      }
    });
  }
});
```

Click **next** to continue the tutorial\.