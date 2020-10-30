--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Using JavaScript promises<a name="using-promises"></a>

Use the service client's `send` method to make the service call and manage asynchronous flow instead of using callbacks\. The following example shows how to get the names of your Amazon DynamoDB tables in `us-west-2`\.

```
const { DynamoDBClient, 
        ListTablesCommand 
} = require('@aws-sdk/client-dynamodb');
const dbClient = new DynamoDBClient({ region: 'us-west-2' });

dbClient
  .listtables(new ListTablesCommand({}))
  .then(response => {
    console.log(response.TableNames.join('\n'));
  })
  .catch((error) => {
    console.error(error);
  });
```

## Coordinating multiple promises<a name="multiple-promises"></a>

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

## Browser and Node\.js support for promises<a name="browser-node-promise-support"></a>

Support for native JavaScript promises \(ECMAScript 2015\) depends on the JavaScript engine and version in which your code executes\. To help determine the support for JavaScript promises in each environment where your code needs to run, see the [ECMAScript compatability table](https://kangax.github.io/compat-table/es6/) on GitHub\.