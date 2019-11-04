# Calling Services Asychronously<a name="calling-services-asynchronously"></a>

All requests made through the SDK are asynchronous\. This is important to keep in mind when writing browser scripts\. JavaScript running in a web browser typically has just a single execution thread\. After making an asynchronous call to an AWS service, the browser script continues running and in the process can try to execute code that depends on that asynchronous result before it returns\.

Making asynchronous calls to an AWS service includes managing those calls so your code doesn't try to use data before the data is available\. The topics in this section explain the need to manage asynchronous calls and detail different techniques you can use to manage them\.

Although you can use any of these techniques to manage asynchronous calls, we recommend you consider them in the following order:

async/await  
Since all of the SDK client calls return a promise, use async/await to wait for the promise to resolve and evaluate the resolved value\.

promise  
Some old browsers do not yet support aynch/await\.

callback  
Avoid using callbacks except in very simple cases\. It can be difficult to debug chained callbacks\.

**Topics**
+ [Managing Asychronous Calls](making-asynchronous-calls.md)
+ [Using an Anonymous Callback Function](using-a-callback-function.md)
+ [Using JavaScript Promises](using-promises.md)
+ [Using async/await](#using-async-await)
+ [Requests With a Node\.js Stream Object](requests-using-stream-objects.md)

## Using async/await<a name="using-async-await"></a>

Rather than using promises, you should consider using async/await\. Async functions are simpler and take less boilerplate than using promises\. Await can only be used in an async function to asynchronously wait for a value\.

The following example uses async/await to list all of your DynamoDB tables in `us-west-2`\.

```
(async function () {
  const DDB = require('@aws-sdk/client-dynamodb')
  const dbClient = new DDB.DynamoDBClient({ region: 'us-west-2' })
  const command = new DDB.ListTablesCommand({})

  try {
    const results = await dbClient.send(command)
    console.log(results.TableNames.join('\n'))
  } catch (err) {
    console.error(err)
  }
})()
```

**Note**  
 Not all browsers support async/await\. See [Async functions](https://caniuse.com/#feat=async-functions) for a list of browsers with async/await support\. 