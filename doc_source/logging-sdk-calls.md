# Logging AWS SDK for JavaScript Calls<a name="logging-sdk-calls"></a>

The AWS SDK for JavaScript is instrumented with a built\-in logger so you can log API calls you make with the SDK for JavaScript\.

To turn on the logger and print log entries in the console, add the following statement to your code\.

```
AWS.config.logger = console;
```

Here is an example of the log output\.

```
[AWS s3 200 0.185s 0 retries] createMultipartUpload({ Bucket: 'js-sdk-test-bucket', Key: 'issues_1704' })
```

## Using a Third\-Party Logger<a name="third-party-logger"></a>

You can also use a third\-party logger, provided it has `log()` or `write()` operations to write to a log file or server\. You must install and set up your custom logger as instructed before you can use it with the SDK for JavaScript\.

One such logger you can use in either browser scripts or in Node\.js is logplease\. In Node\.js, you can configure logplease to write log entries to a log file\.

When using a third\-party logger, set all options before assigning the logger to `AWS.Config.logger`\. For example, the following specifies an external log file and sets the log level for logplease

```
// Require AWS Node.js SDK
const AWS = require('aws-sdk')
// Require logplease
const logplease = require('logplease');
// Set external log file option
logplease.setLogfile('debug.log');
// Set log level
logplease.setLogLevel('DEBUG');
// Create logger
const logger = logplease.create('logger name');
// Assign logger to SDK
AWS.config.logger = logger;
```

For more information about logplease, see the [logplease Simple JavaScript Logger](https://github.com/haadcode/logplease) on GitHub\.