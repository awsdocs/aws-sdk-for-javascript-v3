# Node\.js Considerations<a name="node-js-considerations"></a>

Although Node\.js code is JavaScript, using the AWS SDK for JavaScript in Node\.js can differ from using the SDK in browser scripts\. Some API methods work in Node\.js but not in browser scripts, as well as the other way around\. And successfully using some APIs depends on your familiarity with common Node\.js coding patterns, such as importing and using other Node\.js modules like the `File System (fs)` module\.

## Using Built\-In Node\.js Modules<a name="node-common-modules"></a>

Node\.js provides a collection of built\-in modules you can use without installing them\. To use these modules, create an object with the `require` method to specify the module name\. For example, to include the built\-in HTTP module, use the following\.

```
var http = require('http');
```

Invoke methods of the module as if they are methods of that object\. For example, here is code that reads an HTML file\.

```
// include File System module
var fs = require('fs'); 
// Invoke readFile method 
fs.readFile('index.html', function(err, data) {
  if (err) {
    throw err;
  } else {
    // Successful file read
  }
});
```

For a complete list of all built\-in modules that Node\.js provides, see [Node\.js v6\.11\.1 Documentation](https://nodejs.org/api/modules.html) on the Node\.js website\.

## Using npm Packages<a name="node-npm-packages"></a>

In addition to the built\-in modules, you can also include and incorporate third\-party code from `npm`, the Node\.js package manager\. This is a repository of open source Node\.js packages and a command\-line interface for installing those packages\. For more information about `npm` and a list of currently available packages, see [https://www\.npmjs\.com](https://www.npmjs.com)\. You can also learn about additional Node\.js packages you can use [here on GitHub](https://github.com/sindresorhus/awesome-nodejs)\.

**Topics**
+ [Using Built\-In Node\.js Modules](#node-common-modules)
+ [Using npm Packages](#node-npm-packages)
+ [Configuring maxSockets in Node\.js](node-configuring-maxsockets.md)
+ [Reusing Connections with Keep\-Alive in Node\.js](node-reusing-connections.md)
+ [Configuring Proxies for Node\.js](node-configuring-proxies.md)
+ [Registering Certificate Bundles in Node\.js](node-registering-certs.md)