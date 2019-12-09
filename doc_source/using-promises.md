# Using JavaScript Promises<a name="using-promises"></a>

Use the service client's `send` method to make the service call and manage asynchronous flow instead of using callbacks\. The following example shows how to get the names of your Amazon DynamoDB tables in `us-west-2`\.

```
const DDB = require('@aws-sdk/client-dynamodb')
const dbClient = new DDB.DynamoDBClient({ region: 'us-west-2' })

dbClient
  .send(new DDB.ListTablesCommand({}))
  .then(response => {
    console.log(response.TableNames.join('\n'))
  })
  .catch(error => {
    console.error(error)
  })
```

## Coordinating Multiple Promises<a name="multiple-promises"></a>

In some situations, your code must make multiple asynchronous calls that require action only when they have all returned successfully\. If you manage those individual asynchronous method calls with promises, you can create an additional promise that uses the `all` method\. 

This method fulfills this umbrella promise if and when the array of promises that you pass into the method are fulfilled\. The callback function is passed an array of the values of the promises passed to the `all` method\.

In the following example, an AWS Lambda function must make three asynchronous calls to Amazon DynamoDB but can only complete after the promises for each call are fulfilled\.

```
const values = await Promise.all([firstPromise, secondPromise, thirdPromise]);

console.log("Value 0 is " + values[0].toString);
console.log("Value 1 is " + values[1].toString);
console.log("Value 2 is " + values[2].toString);

return values;
```

## Browser and Node\.js Support for Promises<a name="browser-node-promise-support"></a>

Support for native JavaScript promises \(ECMAScript 2015\) depends on the JavaScript engine and version in which your code executes\. To help determine the support for JavaScript promises in each environment where your code needs to run, see the [ECMAScript Compatability Table](https://kangax.github.io/compat-table/es6/) on GitHub\.

## Using Other Promise Implementations<a name="using-other-promise-implementations"></a>

In addition to the native promise implementation in ECMAScript 2015, you can also use third\-party promise libraries, including:
+ [bluebird](http://bluebirdjs.com)
+ [RSVP](https://github.com/tildeio/rsvp.js/)
+ [Q](https://github.com/kriskowal/q)

These optional promise libraries can be useful if you need your code to run in environments that don't support the native promise implementation in ECMAScript 5 and ECMAScript 2015\.

To use a third\-party promise library, set a promises dependency on the SDK by calling the `setPromisesDependency` method of the global configuration object\. In browser scripts, make sure to load the third\-party promise library before loading the SDK\. In the following example, the SDK is configured to use the implementation in the `bluebird` promise library\.

```
AWS.config.setPromisesDependency(require('bluebird'));
```

To return to using the native promise implementation of the JavaScript engine, call `setPromisesDependency` again, passing a `null` instead of a library name\.