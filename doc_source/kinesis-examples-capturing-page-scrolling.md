# Capturing Webpage Scroll Progress with Amazon Kinesis<a name="kinesis-examples-capturing-page-scrolling"></a>

![\[JavaScript code example that applies to browser execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/browsericon.png)

**This browser script example shows:**
+ How to capture scroll progress in a webpage with Amazon Kinesis as an example of streaming page usage metrics for later analysis\.

## The Scenario<a name="kinesis-examples-capturing-page-scrolling-scenario"></a>

In this example, a simple HTML page simulates the content of a blog page\. As the reader scrolls the simulated blog post, the browser script uses the SDK for JavaScript to record the scroll distance down the page and send that data to Kinesis using the [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Kinesis.html#putRecords-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/Kinesis.html#putRecords-property) method of the Kinesis client class\. The streaming data captured by Amazon Kinesis Data Streams can then be processed by Amazon EC2 instances and stored in any of several data stores including Amazon DynamoDB and Amazon Redshift\.

![\[JavaScript in a browser script sending scroll data to Kinesis.\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/kinesis-examples.png)

## Prerequisite Tasks<a name="kinesis-examples-capturing-page-scrolling-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Create an Kinesis stream\. You need to include the stream's resource ARN in the browser script\. For more information about creating Amazon Kinesis Data Streams, see [Managing Kinesis Streams](https://docs.aws.amazon.com/streams/latest/dev/working-with-streams.html) in the *Amazon Kinesis Data Streams Developer Guide*\.
+ Create an Amazon Cognito identity pool with access enabled for unauthenticated identities\. You need to include the identity pool ID in the code to obtain credentials for the browser script\. For more information about Amazon Cognito identity pools, see [Identity Pools](https://docs.aws.amazon.com/cognito/latest/developerguide/identity-pools.html) in the *Amazon Cognito Developer Guide*\.
+ Create an IAM role whose policy grants permission to submit data to an Kinesis stream\. For more information about creating an IAM role, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

Use the following role policy when creating the IAM role\.

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
         "Effect": "Allow",
         "Action": [
            "mobileanalytics:PutEvents",
            "cognito-sync:*"
         ],
         "Resource": [
            "*"
         ]
      },
      {
         "Effect": "Allow",
         "Action": [
            "kinesis:Put*"
         ],
         "Resource": [
            "STREAM_RESOURCE_ARN"
         ]
      }
   ]
}
```

## The Blog Page<a name="kinesis-examples-capturing-page-scrolling-html"></a>

The HTML for the blog page consists mainly of a series of paragraphs contained within a `<div>` element\. The scrollable height of this `<div>` is used to help calculate how far a reader has scrolled through the content as they read\. The HTML also contains a pair of `<script>` elements\. One of these elements adds the SDK for JavaScript to the page and the other adds the browser script that captures scroll progress on the page and reports it to Kinesis\.

```
<!DOCTYPE html>
<html>
    <head>
        <title>AWS SDK for JavaScript - Amazon Kinesis Application</title>
    </head>
    <body>
        <div id="BlogContent" style="width: 60%; height: 800px; overflow: auto;margin: auto; text-align: center;">
            <div>
                <p>
                Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vestibulum vitae nulla eget nisl bibendum feugiat. Fusce rhoncus felis at ultricies luctus. Vivamus fermentum cursus sem at interdum. Proin vel lobortis nulla. Aenean rutrum odio in tellus semper rhoncus. Nam eu felis ac augue dapibus laoreet vel in erat. Vivamus vitae mollis turpis. Integer sagittis dictum odio. Duis nec sapien diam. In imperdiet sem nec ante laoreet, vehicula facilisis sem placerat. Duis ut metus egestas, ullamcorper neque et, accumsan quam. Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos.
                </p>
                <!-- Additional paragraphs in the blog page appear here -->
            </div>
        </div>
        <script src="https://sdk.amazonaws.com/js/aws-sdk-2.283.1.min.js"></script>
        <script src="kinesis-example.js"></script>
    </body>
</html>
```

## Configuring the SDK<a name="kinesis-examples-capturing-page-scrolling-configure-sdk"></a>

Obtain the credentials needed to configure the SDK by calling the `CognitoIdentityCredentials` method, providing the Amazon Cognito identity pool ID\. Upon success, create the Kinesis service object in the callback function\.

The following code snippet shows this step\. \(See [Capturing Webpage Scroll Progress Code](kinesis-examples-capturing-page-scrolling-full.md) for the full example\.\)

```
// Configure Credentials to use Cognito
AWS.config.credentials = new AWS.CognitoIdentityCredentials({
    IdentityPoolId: 'IDENTITY_POOL_ID'
});

AWS.config.region = 'REGION';
// We're going to partition Amazon Kinesis records based on an identity.
// We need to get credentials first, then attach our event listeners.
AWS.config.credentials.get(function(err) {
    // attach event listener
    if (err) {
        alert('Error retrieving credentials.');
        console.error(err);
        return;
    }
    // create Amazon Kinesis service object
    var kinesis = new AWS.Kinesis({
        apiVersion: '2013-12-02'
    });
```

## Creating Scroll Records<a name="kinesis-examples-capturing-page-scrolling-create-records"></a>

Scroll progress is calculated using the `scrollHeight` and `scrollTop` properties of the `<div>` containing the content of the blog post\. Each scroll record is created in an event listener function for the `scroll` event and then added to an array of records for periodic submission to Kinesis\.

The following code snippet shows this step\. \(See [Capturing Webpage Scroll Progress Code](kinesis-examples-capturing-page-scrolling-full.md) for the full example\.\)

```
    // Get the ID of the Web page element.
    var blogContent = document.getElementById('BlogContent');

    // Get Scrollable height
    var scrollableHeight = blogContent.clientHeight;

    var recordData = [];
    var TID = null;
    blogContent.addEventListener('scroll', function(event) {
        clearTimeout(TID);
        // Prevent creating a record while a user is actively scrolling
        TID = setTimeout(function() {
            // calculate percentage
            var scrollableElement = event.target;
            var scrollHeight = scrollableElement.scrollHeight;
            var scrollTop = scrollableElement.scrollTop;

            var scrollTopPercentage = Math.round((scrollTop / scrollHeight) * 100);
            var scrollBottomPercentage = Math.round(((scrollTop + scrollableHeight) / scrollHeight) * 100);

            // Create the Amazon Kinesis record
            var record = {
                Data: JSON.stringify({
                    blog: window.location.href,
                    scrollTopPercentage: scrollTopPercentage,
                    scrollBottomPercentage: scrollBottomPercentage,
                    time: new Date()
                }),
                PartitionKey: 'partition-' + AWS.config.credentials.identityId
            };
            recordData.push(record);
        }, 100);
    });
```

## Submitting Records to Kinesis<a name="kinesis-examples-capturing-page-scrolling-submit-records"></a>

Once each second, if there are records in the array, those pending records are sent to Kinesis\.

The following code snippet shows this step\. \(See [Capturing Webpage Scroll Progress Code](kinesis-examples-capturing-page-scrolling-full.md) for the full example\.\)

```
    // upload data to Amazon Kinesis every second if data exists
    setInterval(function() {
        if (!recordData.length) {
            return;
        }
        // upload data to Amazon Kinesis
        kinesis.putRecords({
            Records: recordData,
            StreamName: 'NAME_OF_STREAM'
        }, function(err, data) {
            if (err) {
                console.error(err);
            }
        });
        // clear record data
        recordData = [];
    }, 1000);
});
```