--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Capturing Webpage Scroll Progress with Amazon Kinesis<a name="kinesis-examples-capturing-page-scrolling"></a>

![\[JavaScript code example that applies to browser execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/browsericon.png)

**This browser script example shows:**
+ How to capture scroll progress in a webpage with Amazon Kinesis as an example of streaming page usage metrics for later analysis\.

## Prerequisite Tasks<a name="kinesis-examples-capturing-page-scrolling-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/kinesis/README.md)\.
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

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

## Configuring the SDK<a name="kinesis-examples-capturing-page-scrolling-configure-sdk"></a>

Obtain the credentials needed to configure the SDK by calling the `CognitoIdentityCredentials` method, providing the Amazon Cognito identity pool ID\. Upon success, create the Kinesis service object in the callback function\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

The following code snippet shows this step\. \(See [Capturing Webpage Scroll Progress Code](kinesis-examples-capturing-page-scrolling-full.md) for the full example\.\)

## Creating Scroll Records<a name="kinesis-examples-capturing-page-scrolling-create-records"></a>

Scroll progress is calculated using the `scrollHeight` and `scrollTop` properties of the `<div>` containing the content of the blog post\. Each scroll record is created in an event listener function for the `scroll` event and then added to an array of records for periodic submission to Kinesis\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

The following code snippet shows this step\. \(See [Capturing Webpage Scroll Progress Code](kinesis-examples-capturing-page-scrolling-full.md) for the full example\.\)

## Submitting Records to Kinesis<a name="kinesis-examples-capturing-page-scrolling-submit-records"></a>

Once each second, if there are records in the array, those pending records are sent to Kinesis\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

The following code snippet shows this step\. \(See [Capturing Webpage Scroll Progress Code](kinesis-examples-capturing-page-scrolling-full.md) for the full example\.\)