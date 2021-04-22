--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Bundling applications with webpack<a name="webpack"></a>

The use of code modules by web applications in browser scripts or Node\.js creates dependencies\. These code modules can have dependencies of their own, resulting in a collection of interconnected modules that your application requires to function\. To manage dependencies, you can use a module bundler like `webpack`\.

The `webpack` module bundler parses your application code, searching for `import` or `require` statements, to create bundles that contain all the assets your application needs\. This is so that the assets can be easily served through a webpage\. The SDK for JavaScript can be included in `webpack` as one of the dependencies to include in the output bundle\.

![\[Process by which webpack bundles application module dependencies into a static asset\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/webpack.png)

For more information about `webpack`, see the [webpack module bundler](https://webpack.github.io/) on GitHub\.

## Installing webpack<a name="webpack-installing"></a>

To install the `webpack` module bundler, you must first have npm, the Node\.js package manager, installed\. Type the following command to install the `webpack` CLI and JavaScript module\.

```
npm install --save-dev webpack
```

To use the `path` module for working with file and directory paths, which is installed automatically with webpack, you might need to install the Node\.js `path-browserify` package\. 

```
npm install --save-dev path-browserify
```

## Configuring webpack<a name="webpack-configuring"></a>

By default, Webpack searches for a JavaScript file named `webpack.config.js` in your project's root directory\. This file specifies your configuration options\. The following is an example of a `webpack.config.js` configuration file for WebPack version 5\.0\.0 and later\.

**Note**  
Webpack configuration requirments vary depending on the version of Webpack you install\. For more information, see the [Webpack documentation](https://webpack.js.org/configuration/)\. 

```
// Import path for resolving file paths
var path = require("path");
module.exports = {
  // Specify the entry point for our app.
  entry: [path.join(__dirname, "browser.js")],
  // Specify the output file containing our bundled code.
  output: {
    path: __dirname,
    filename: 'bundle.js'
  },
   // Enable WebPack to use the 'path' package.
  resolve:{
    fallback: { path: require.resolve("path-browserify")}
  }
   /**
   * In Webpack version v2.0.0 and earlier, you must tell 
   * webpack how to use "json-loader" to load 'json' files.
   * To do this Enter 'npm --save-dev install json-loader' at the 
   * command line to install the "json-loader' package, and include the 
   * following entry in your webpack.config.js.
   module: {
    rules: [{test: /\.json$/, use: use: "json-loader"}]
  }
  **/
};
```

In this example, `browser.js` is specified as the *entry point*\. The *entry point* is the file `webpack` uses to begin searching for imported modules\. The file name of the output is specified as `bundle.js`\. This output file will contain all the JavaScript the application needs to run\. If the code specified in the entry point imports or requires other modules, such as the SDK for JavaScript, that code is bundled without needing to specify it in the configuration\.

## Running webpack<a name="webpack-running"></a>

To build an application to use `webpack`, add the following to the `scripts` object in your `package.json` file\.

```
"build": "webpack"
```

The following is an example `package.json` file that demonstrates adding `webpack`\.

```
{
  "name": "aws-webpack",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "@aws-sdk/client-iam": "^3.0.0",
    "@aws-sdk/client-s3": "^3.3.0"
  },
  "devDependencies": {
    "webpack": "^5.0.0"
  }
}
```

To build your application, enter the following command\.

```
npm run build
```

The `webpack` module bundler then generates the JavaScript file you specified in your project's root directory\.

## Using the webpack bundle<a name="webpack-using-bundle"></a>

To use the bundle in a browser script, you can incorporate the bundle using a `<script>` tag, as shown in the following example\.

```
<!DOCTYPE html>
<html>
    <head>
        <title>AWS SDK with webpack</title>
    </head> 
    <body>
        <div id="list"></div>
        <script src="bundle.js"></script>
    </body>
</html>
```

## Bundling for Node\.js<a name="webpack-nodejs-bundles"></a>

You can use `webpack` to generate bundles that run in Node\.js by specifying `webpack` as a target in the configuration\.

```
target: "node"
```

This is useful when running a Node\.js application in an environment where disk space is limited\. Here is an example `webpack.config.js` configuration with Node\.js specified as the output target\.

```
// Import path for resolving file paths
var path = require("path");
module.exports = {
  // Specify the entry point for our app.
  entry: [path.join(__dirname, "browser.js")],
  // Specify the output file containing our bundled code.
  output: {
    path: __dirname,
    filename: 'bundle.js'
  },
  // Let webpack know to generate a Node.js bundle.
  target: "node",
   // Enable WebPack to use the 'path' package.
  resolve:{
    fallback: { path: require.resolve("path-browserify")}
  }
   /**
   * In Webpack version v2.0.0 and earlier, you must tell 
   * webpack how to use "json-loader" to load 'json' files.
   * To do this Enter 'npm --save-dev install json-loader' at the 
   * command line to install the "json-loader' package, and include the 
   * following entry in your webpack.config.js.
   module: {
    rules: [{test: /\.json$/, use: use: "json-loader"}]
  }
  **/
};
```
