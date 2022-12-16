--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Migrating your code to SDK for JavaScript V3<a name="migrating-to-v3"></a>

AWS SDK for JavaScript version 3 \(v3\) also comes with modernized interfaces for client configurations and utilities, which include credentials, Amazon S3 multipart upload, DynamoDB document client, waiters, and so forth\)\. You can find what changed in v2 and the v3 equivalents for each change in the [migration guide on the AWS SDK for JavaScript GitHub repo](https://github.com/aws/aws-sdk-js-v3/blob/main/UPGRADING.md)\.

The experimental collection of codemod scripts in [aws-sdk-js-codemod](https://www.npmjs.com/package/aws-sdk-js-codemod) helps migrate your existing AWS SDK for JavaScript (v2) application to use v3 APIs\. You can run the transform as follows:

```console
$ npx aws-sdk-js-codemod -t v2-to-v3 PATH...
```

For example, consider you have the following code, which creates a Amazon DynamoDB client from v2 and calls listTables operation:

```ts
// example.ts
import AWS from "aws-sdk";

const region = "us-west-2";
const client = new AWS.DynamoDB({ region });
client.listTables({}, (err, data) => {
  if (err) console.log(err, err.stack);
  else console.log(data);
});
```

You can run our `v2-to-v3` transform on `example.ts` as follows:

```console
$ npx aws-sdk-js-codemod -t v2-to-v3 example.ts
```

The transform will convert the DynamoDB import to v3, create v3 client and call listTables operation as follows:

```ts
// example.ts
import { DynamoDB } from "@aws-sdk/client-dynamodb";

const region = "us-west-2";
const client = new DynamoDB({ region });
client.listTables({}, (err, data) => {
  if (err) console.log(err, err.stack);
  else console.log(data);
});
```

We’ve implemented transforms for common use-cases\. If your code doesn’t transform correctly, please create a [bug report](https://github.com/awslabs/aws-sdk-js-codemod/issues/new?assignees=&labels=bug%2Ctriage&template=bug_report.yml&title=%5BBug%3F%5D%3A+) or [feature request](https://github.com/awslabs/aws-sdk-js-codemod/issues/new?assignees=&labels=enhancement&template=feature_request.yml&title=%5BFeature%5D%3A+) with example input code and observed/expected output code\. If your specific use case is already reported in [an existing issue](https://github.com/awslabs/aws-sdk-js-codemod/issues?q=is%3Aissue+is%3Aopen+sort%3Aupdated-desc), show your support by an up-vote\.
