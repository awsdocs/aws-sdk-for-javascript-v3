--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Creating service client requests<a name="the-request-object"></a>

Making requests to AWS service clients is straightforward\. Version 3 \(V3\) of the SDK for JavaScript enables you to send requests\. 

**Note**  
You can also perform operations using version 2 \(V2\) commands when using the V3 of the SDK for JavaScript\. For more information, see [Using V2 commands](welcome.md#using_v2_commands)\.

**To send a request:**

1. Initialize a client object with the desired configuration, such as a specific AWS Region\.

1. \(Optional\) Create a request JSON object with the values for the request, such as the name of a specific Amazon S3 bucket\. You can examine the parameters for the request by looking at the API Reference topic for the interface with the name associated with the client method\. For example, if you use the *AbcCommand* client method, the request interface is *AbcInput*\.

1. Initialize a service command, optionally, with the request object as input\.

1. Call `send` on the client with the command object as input\.

For example, to list your Amazon DynamoDB tables in `us-west-2`, you can do it with async/await\.

```
  import { 
  DynamoDBClient, 
  ListTablesCommand 
  } from "@aws-sdk/client-dynamodb";
  
  (async function() {
  const dbClient = new DynamoDBClient({ region: 'us-west-2' });
  const command = new ListTablesCommand({});

  try {
    const results = await dbClient.send(command);
    console.log(results.TableNames.join('\n'));
  } catch (err) {
    console.error(err);
  }
})();
```