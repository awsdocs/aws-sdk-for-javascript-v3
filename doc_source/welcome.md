# What Is the AWS SDK for JavaScript?<a name="welcome"></a>

The [AWS SDK for JavaScript API Reference](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/) provides a JavaScript API for AWS services\. You can use the JavaScript API to build libraries or applications for [Node\.js](https://nodejs.org/en/) or the browser\.

![\[Relationship between JavaScript environments, the SDK, and Amazon Web Services\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/sdk-overview-v3.png)

## What's New in Version 3<a name="welcome_whats_new_v3"></a>

Version 3 of the SDK for JavaScript \(V3\) contains the following new features:

Modularized Packages  
Users can now use a separate package for each service\.

New Middleware Stack  
Users can now use a middleware stack to control the lifecycle of an operation call\.

In addition, the SDK is written in TypeScript, which has many advantages, such as static typing\.

### Modularized Packages<a name="welcome_whats_new_v3_modularized_packages"></a>

Version 2 of the SDK for JavaScript \(V2\) required you to use the entire AWS SDK, as follows\.

```
var AWS = require("aws-sdk");
```

Loading the entire SDK isn’t an issue if your application is using many AWS services\. However, if you need to use only a few services, it means increasing the size of your application with code you don't need or use\.

In V3, you can load and use only the individual services you need\. This is shown in the following example, which only gives you access to Amazon Simple Storage Service \(Amazon S3\)\.

```
const s3 = require("@aws-sdk/client-s3");
```

Not only can you load and use individual services, but you can also load and use only the service commands you need\. This is shown in the following example, which only gives you access to the Amazon S3 client and `ListBucketsCommand` command\.

```
const {
  S3Client,
  ListBucketsCommand
} = require('@aws-sdk/client-s3')
```

#### Comparing Code Size<a name="welcome_whats_new_v3_modularized_packages_code_size"></a>

In V2, a simple code example that lists all of your Amazon S3 buckets in the `us-west-2` Region might look like the following\.

```
var AWS = require("aws-sdk");
// Set the Region
AWS.config.update({region: "us-west-2"})
// Create S3 service object
s3Client = new AWS.S3({apiVersion: "2006-03-01"})

// List your buckets
s3Client.listBuckets(function(err, data) {
  if (err) {
    console.log("Error", err);
  } else {
    console.log("Success", data.Buckets);
  }
})
```

V3 looks like the following\.

```
(async function () {
  const S3 = require('@aws-sdk/client-s3')
  const s3Client = new S3.S3Client({
    region: 'us-west-2'
  })

  const command = new S3.ListBucketsCommand({})

  try {
    const results = await s3Client.send(command)
    results.Buckets.forEach(function (item, index) {
      console.log(item.Name)
    })
  } catch (err) {
    console.error(err)
  }
})()
```

The `aws-sdk` package adds about 40 MB to your application\. Replacing `var AWS = require("aws-sdk")` with `const s3 = require("@aws-sdk/client-s3")` reduces that overhead to about 3MB\. Restricting the import to just the Amazon S3 client and `ListBucketsCommand` command reduces the overhead to less that 100KB:

```
// Load the S3 client and ListBucketsCommand command for Node.js
const {
  S3Client,
  ListBucketsCommand
} = require('@aws-sdk/client-s3')
        
const s3Client = new S3Client({})
const listBucketsCommand = new ListBucketsCommand({})
```

### New Middleware Stack<a name="welcome_whats_new_v3_middleware_stack"></a>

V2 of the SDK enabled modifying a request throughout the multiple stages of its lifecycle by attaching event listeners to the request\. This approach can make it difficult to debug what went wrong during a request’s lifecycle\.

In V3, you can use a new middleware stack to control the lifecycle of an operation call\. This approach provides a couple of benefits\. Each middleware stage in the stack calls the next middleware stage after making any changes to the request object\. This also makes debugging issues in the stack much easier, because you can see exactly which middleware stages were called leading up to the error\.

The following example adds a custom header to an AWS Lambda client using middleware\. The first argument is a function that accepts `next`, which is the next middleware stage in the stack to call, and `context`, which is an object that contains some information about the operation being called\. The function returns a function that accepts `args`, which is an object that contains the parameters passed to the operation and the request, and returns the result from calling the next middleware with `args`\.

```
lambda.middlewareStack.add(
  (next, context) => args => {
    args.request.headers["Custom-Header"] = "value";
    return next(args);
  },
  {
    step: "build"
  }
)

lambda.send(new PutObjectCommand(params))
```

## Using the SDK with Node\.js<a name="welcome_node"></a>

Node\.js is a cross\-platform runtime for running server\-side JavaScript applications\. You can set up Node\.js on an Amazon Elastic Compute Cloud \(Amazon EC2\) instance to run on a server\. You can also use Node\.js to write on\-demand AWS Lambda functions\.

Using the SDK for Node\.js differs from the way in which you use it for JavaScript in a web browser\. The difference comes from the way in which you load the SDK and in how you obtain the credentials needed to access specific web services\. When use of particular APIs differs between Node\.js and the browser, those differences will be called out\.

## Using the SDK with AWS Cloud9<a name="welcome_cloud9"></a>

You can also develop Node\.js applications using the SDK for JavaScript in the AWS Cloud9 IDE\. For an example of how to use AWS Cloud9 for Node\.js development, see [Node\.js Sample for AWS Cloud9](https://docs.aws.amazon.com/cloud9/latest/user-guide//sample-nodejs.html) in the *AWS Cloud9 User Guide*\. 

## Using the SDK with AWS Amplify<a name="welcome_amplify"></a>

For browser\-based web, mobile, and hybrid apps, you can also use the [AWS Amplify Library on GitHub](https://github.com/aws/aws-amplify)\. It extends the SDK for JavaScript, providing a declarative interface\.

**Note**  
Frameworks such as Amplify might not offer the same browser support as the SDK for JavaScript\. See the framework's documentation for details\.

## Using the SDK with Web Browsers<a name="welcome_web"></a>

All major web browsers support execution of JavaScript\. JavaScript code that is running in a web browser is often called *client\-side JavaScript*\.

Using the SDK for JavaScript in a web browser differs from the way in which you use it for Node\.js\. The difference comes from the way in which you load the SDK and in how you obtain the credentials needed to access specific web services\. When use of particular APIs differs between Node\.js and the browser, those differences will be called out\.

For a list of browsers that are supported by the AWS SDK for JavaScript, see [Supported Web Browsers](browsers-supported.md)\.

### Common Use Cases<a name="welcome_use_cases"></a>

Using the SDK for JavaScript in browser scripts makes it possible to realize a number of compelling use cases\. Here are several ideas for things you can build in a browser application by using the SDK for JavaScript to access various web services\.
+ Build a custom console to AWS services in which you access and combine features across Regions and services to best meet your organizational or project needs\.
+ Use Amazon Cognito Identity to enable authenticated user access to your browser applications and websites, including use of third\-party authentication from Facebook and others\.
+ Use Amazon Kinesis to process click streams or other marketing data in real time\.
+ Use Amazon DynamoDB for serverless data persistence such as individual user preferences for website visitors or application users\.
+ Use AWS Lambda to encapsulate proprietary logic that you can invoke from browser scripts without downloading and revealing your intellectual property to users\.

### About the Examples<a name="welcome_examples"></a>

You can browse the SDK for JavaScript examples in the [AWS Code Sample Catalog](https://docs.aws.amazon.com/code-samples/latest/catalog/code-catalog-javascript.html)\.

### Resources<a name="welcome_resources"></a>

In addition to this guide, the following online resources are available for SDK for JavaScript developers\.
+ [AWS SDK for JavaScript API Reference](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/)
+ [JavaScript Developer Blog](https://aws.amazon.com/blogs/developer/category/programing-language/javascript/)
+ [AWS JavaScript Forum](https://forums.aws.amazon.com/forum.jspa?forumID=148)
+ [JavaScript examples in the AWS Code Catalog](https://docs.aws.amazon.com/code-samples/latest/catalog/code-catalog-javascript.html)
+ [Gitter Channel](https://gitter.im/aws/aws-sdk-js)
+ [Stack Overflow](https://stackoverflow.com/search?tab=newest&q=aws-sdk-js)
+ [Stack Overflow questions tagged aws\-sdk\-js](https://stackoverflow.com/questions/tagged/aws-sdk-js?sort=newest)
+ GitHub
  + [SDK Source](https://github.com/aws/aws-sdk-js-v3/)
  + [Documentation Source](https://github.com/awsdocs/aws-sdk-for-javascript-v3)