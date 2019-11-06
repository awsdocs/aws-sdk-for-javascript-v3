# What Is the AWS SDK for JavaScript?<a name="welcome"></a>

The [AWS SDK for JavaScript](aws-jsdk-reference.md) provides a JavaScript API for AWS services\. You can use the JavaScript API to build libraries or applications for [Node\.js](https://nodejs.org/en/) or the browser\.



Not all services are immediately available in the SDK\. To find out which services are currently supported by the AWS SDK for JavaScript, see [ https://github\.com/aws/aws\-sdk\-js/blob/master/SERVICES\.md]( https://github.com/aws/aws-sdk-js/blob/master/SERVICES.md)\. For information about the SDK for JavaScript on GitHub, see [Additional Resources](resources.md)\.

## What's New in Version 3<a name="welcome_whats_new_v3"></a>

Version 3 of the SDK for JavaScript \(V3\) contains three new features:

Modularized Packages  
Users can now use a separate package for each service\.

New Middleware Stack  
Users can now use a middleware stack to control the lifecycle of an operation call\.

Separate Node\.js and Browser Packages  
Each package has both a browser and Node\.js version\.

### Modularized Packages<a name="welcome_whats_new_v3_modularized_packages"></a>

Version 2 of the SDK for JavaScript \(V2\) required you to use the entire AWS SDK, as follows\.

```
var AWS = require("aws-sdk");
```

Loading the entire SDK isn’t an issue if your application is using many AWS services\. However, if you need to use only a few services, it means increasing the size of your application with code you don't need or use\.

In V3, you can load and use only the individual services you need\. This is shown in the following example, which only gives you access to Amazon S3\.

```
const s3 = require("@aws-sdk/client-s3");
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

The V3 version looks like the following:

```
(async function () {
  const S3 = require('@aws-sdk/client-s3-node')
  const S3Client = new S3.S3Client({
    region: 'us-west-2'
  })

  const command = new S3.ListBucketsCommand({})

  try {
    const results = await S3Client.send(command)
    results.Buckets.forEach(function (item, index) {
      console.log(item.Name)
    })
  } catch (err) {
    console.error(err)
  }
})()
```

The `aws-sdk` package adds about 40 MB to your application\. By replacing `var AWS = require("aws-sdk")` with `const s3 = require("@aws-sdk/client-s3")` reduces that overhead to about ??MB\.

### New Middleware Stack<a name="welcome_whats_new_v3_middleware_stack"></a>

V2 enabled modifying a request throughout the multiple stages of its lifecycle by attaching event listeners to the request\. This approach can make if difficult to debug what went wrong during a request’s lifecycle\.

In V3, you can use a new middleware stack to control the lifecycle of an operation call\. This approach provides a couple of benefits\. Each middleware stage in the stack calls the next middleware stage after making any changes to the request object\. This also makes debugging issues in the stack much easier, because you can see exactly which middleware stages were called leading up to the error\.

The following example adds a custom header to an AWS Lambda client using middleware\. The first argument is a function that accepts next, which is the next middleware stage in the stack to call, and context, which is an object that contains some information about the operation being called\. The function returns a function that accepts args, which is an object that contains the parameters passed to the operation and the request, and returns the result from calling the next middleware with args\.

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

### Separate Node\.js and Browser Packages<a name="welcome_whats_new_v3_separate_packages"></a>

V2 used the browser field in `package.json` to enable writing code against the aws\-sdk package for use in either browsers or Node\.js\.

This approach had a few limitations\.

To import the SDK into a project, you had to use build tools that knew how to parse the browser field to get the browser version of the SDK\. This caused problems when build tools created a JavaScript bundle using the browser version of the SDK, but applications required the Node\.js version of the SDK\. In addition, to support TypeScript, both the Node\.js and DOM typings had to be present\.

In V3, each package has both a browser version and a Node\.js version\. By publishing separate packages for Node\.js and browser environments, the SDK has removed the guesswork around which version your builds use\. This also enables the SDK to use environment\-specific typings\.

For example, the SDK implements streams with different interfaces in Node\.js and browsers\. Depending on which package you’re using, the typings reflect the streams for the environment you’ve chosen\.

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