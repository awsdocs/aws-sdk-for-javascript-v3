--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Using async/await<a name="using-async-await"></a>

Rather than using promises, you should consider using async/await\. Async functions are simpler and take less boilerplate than using promises\. Await can only be used in an async function to asynchronously wait for a value\.

The following example uses async/await to list all of your Amazon DynamoDB tables in `us-west-2`\.

**Note**  
For this example to run:  
Install the AWS SDK for JavaScript DynamoDB client by entering `npm install @aws-sdk/client-dynamodb` in the command line of your project\.
Ensure you have configured your AWS credentials correctly\. For more information, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\. 

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