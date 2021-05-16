--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Building the SDK for Browsers<a name="building-sdk-for-browsers"></a>

Unlike SDK for JavaScript version 2 \(V2\), V3 is not provided as a JavaScript file with support included for a default set of services\. Instead V3 enables you to bundle and include in the browser only the SDK for JavaScript files you require, reducing overhead\. We recommend using Webpack to bundle the required SDK for JavaScript files, and any additional third\-party packages your require, into a single `Javascript` file, and load it into browser scripts using a `<script>` tag\. For more information about Webpack, see [Bundling applications with webpack](webpack.md)\. For an example tha uses Webpack to load V3 SDK for JavaScript into a browser, see [Build an app to submit data to DynamoDB](cross-service-example-submitting-data.md)\.

If you work with the SDK outside of an environment that enforces CORS in your browser and if you want access to all services provided by the SDK for JavaScript, you can build a custom copy of the SDK locally by cloning the repository and running the same build tools that build the default hosted version of the SDK\. The following sections describe the steps to build the SDK with extra services and API versions\.

## Using the SDK Builder to Build the SDK for JavaScript<a name="using-the-sdk-builder"></a>

**Note**  
Amazon Web Services version 3 \(V3\) no longer supports Browser Builder\. To mimimize bandwidth usage of browser applications, we recommend you import named modules, and bundle them to reduce size\. For more information about bundling, see [Bundling applications with webpack](webpack.md)\.
