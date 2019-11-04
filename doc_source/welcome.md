# What Is the AWS SDK for JavaScript?<a name="welcome"></a>

The [AWS SDK for JavaScript](aws-jsdk-reference.md) provides a JavaScript API for AWS services\. You can use the JavaScript API to build libraries or applications for [Node\.js](https://nodejs.org/en/) or the browser\.



Not all services are immediately available in the SDK\. To find out which services are currently supported by the AWS SDK for JavaScript, see [ https://github\.com/aws/aws\-sdk\-js/blob/master/SERVICES\.md]( https://github.com/aws/aws-sdk-js/blob/master/SERVICES.md)\. For information about the SDK for JavaScript on GitHub, see [Additional Resources](resources.md)\.

## Using the SDK with Node\.js<a name="welcome_node"></a>

Node\.js is a cross\-platform runtime for running server\-side JavaScript applications\. You can set up Node\.js on an Amazon EC2 instance to run on a server\. You can also use Node\.js to write on\-demand AWS Lambda functions\.

Using the SDK for Node\.js differs from the way in which you use it for JavaScript in a web browser\. The difference comes from the way in which you load the SDK and in how you obtain the credentials needed to access specific web services\. When use of particular APIs differs between Node\.js and the browser, those differences will be called out\.

## Using the SDK with AWS Cloud9<a name="welcome_cloud9"></a>

You can also develop Node\.js applications using the SDK for JavaScript in the AWS Cloud9 IDE\. For a sample of how to use AWS Cloud9 for Node\.js development, see [Node\.js Sample for AWS Cloud9](https://docs.aws.amazon.com/cloud9/latest/user-guide//sample-nodejs.html) in the *AWS Cloud9 User Guide*\. For more information on using AWS Cloud9 with the SDK for JavaScript, see [Using AWS Cloud9 with the AWS SDK for JavaScript](cloud9-javascript.md)\.

## Using the SDK with AWS Amplify<a name="welcome_amplify"></a>

For browser\-based web, mobile, and hybrid apps, you can also use the [AWS Amplify Library on GitHub](https://github.com/aws/aws-amplify), which extends the SDK for JavaScript, providing a declarative interface\.

**Note**  
Frameworks such as AWS Amplify might not offer the same browser support as the SDK for JavaScript\. Check a framework's documentation for details\.

## Using the SDK with Web Browsers<a name="welcome_web"></a>

All major web browsers support execution of JavaScript\. JavaScript code that is running in a web browser is often called *client\-side JavaScript*\.

Using the SDK for JavaScript in a web browser differs from the way in which you use it for Node\.js\. The difference comes from the way in which you load the SDK and in how you obtain the credentials needed to access specific web services\. When use of particular APIs differs between Node\.js and the browser, those differences will be called out\.

For a list of browsers that are supported by the AWS SDK for JavaScript, see [Web Browsers Supported](browsers-supported.md)\.

### Common Use Cases<a name="welcome_use_cases"></a>

Using the SDK for JavaScript in browser scripts makes it possible to realize a number of compelling use cases\. Here are several ideas for things you can build in a browser application by using the SDK for JavaScript to access various web services\.
+ Build a custom console to AWS services in which you access and combine features across Regions and services to best meet your organizational or project needs\.
+ Use Amazon Cognito Identity to enable authenticated user access to your browser applications and websites, including use of third\-party authentication from Facebook and others\.
+ Use Amazon Kinesis to process click streams or other marketing data in real time\.
+ Use Amazon DynamoDB for serverless data persistence such as individual user preferences for website visitors or application users\.
+ Use AWS Lambda to encapsulate proprietary logic that you can invoke from browser scripts without downloading and revealing your intellectual property to users\.

### About the Examples<a name="welcome_examples"></a>

You can browse the SDK for JavaScript examples in the [AWS Code Sample Catalog](https://docs.aws.amazon.com/code-samples/latest/catalog/code-catalog-javascript.html)\.