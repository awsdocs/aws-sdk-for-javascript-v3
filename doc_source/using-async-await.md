--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Using async/await<a name="using-async-await"></a>

Rather than using promises, you should consider using async/await\. Async functions are simpler and take less boilerplate than using promises\. Await can only be used in an async function to asynchronously wait for a value\.

The following example uses async/await to list all of your Amazon DynamoDB tables in `us-west-2`\.

```
(async function () {
  const DDB = require("@aws-sdk/client-dynamodb");
  const dbClient = new DDB.DynamoDBClient({ region: "us-west-2" });
  const command = new DDB.ListTablesCommand({});

  try {
    const results = await dbClient.send(command);
    console.log(results.TableNames.join('\n'));
  } catch (err) {
    console.error(err)
  }
})();
```

**Note**  
 Not all browsers support async/await\. See [Async functions](https://caniuse.com/#feat=async-functions) for a list of browsers with async/await support\. 