# Creating Service Client Requests<a name="the-request-object"></a>

Making requests to AWS service clients is straightforward\.

**To send a request:**

1. Initialize a client object with the desired configuration, such as a specific AWS Region\.

1. \(Optional\) Create a request JSON object with the values for the request, such as the name of a specific Amazon S3 bucket\. You can examine the parameters for the request by looking at the API reference topic for the interface with the name associated with the client method\. For example, if you use the *AbcCommand* client method, the request interface is *AbcInput*\.

1. Initialize a service command, optionally, with the request object as input\.

1. Call `send` on the client with the command object as input\.

For example, to list your Amazon DynamoDB tables in `us-west-2`, you can do it with async/await\.

```
(async function() {
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