# Installing the SDK for JavaScript<a name="installing-jssdk"></a>

Whether and how you install the AWS SDK for JavaScript depends whether the code executes in Node\.js modules or browser scripts\.

Not all services are immediately available in the SDK\. To find out which services are currently supported by the AWS SDK for JavaScript, see [https://github\.com/aws/aws\-sdk\-js/blob/master/SERVICES\.md](https://github.com/aws/aws-sdk-js/blob/master/SERVICES.md)

------
#### [ Node ]

The preferred way to install the AWS SDK for JavaScript for Node\.js is to use [npm, the Node\.js package manager](https://www.npmjs.com/)\. To do so, type this at the command line\.

```
npm install aws-sdk
```

In the event you see this error message:

```
npm WARN deprecated node-uuid@1.4.8: Use uuid module instead
```

Type these commands at the command line:

```
npm uninstall --save node-uuid
npm install --save uuid
```

------
#### [ Browser ]

To use the SDK in the browser, simply add the following script tag to your HTML pages:

```
<script src="https://sdk.amazonaws.com/js/aws-sdk-2.554.0.min.js"></script>
```

See [In the Browser](https://github.com/aws/aws-sdk-js#in-the-browser) in the SDK's GitHub repository for details\. 

------