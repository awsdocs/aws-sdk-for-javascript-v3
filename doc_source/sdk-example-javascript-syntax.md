--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# JavaScript ES6/CommonJS syntax<a name="sdk-example-javascript-syntax"></a>

The AWS SDK for JavaScript code examples are written in ECMAScript 6 \(ES6\)\. ES6 brings new syntax and new features to make your code more modern and readable, and do more\. 

ES6 requires you use Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download) However, if you prefer, you can convert any of our examples to CommonJS syntax using the following guidelines:
+ Remove `"type" : "module"` from the `package.json` in your project environment\.
+ Convert all ES6 `import` statements to CommonJS `require` statements\. For example, convert:

  ```
  import { CreateBucketCommand } from "@aws-sdk/client-s3";
  import { s3 } from "./libs/s3Client.js";
  ```

  To its CommonJS equivalent:

  ```
  const { CreateBucketCommand } = require("@aws-sdk/client-s3");
  const { s3 } = require("./libs/s3Client.js");
  ```
+ Convert all ES6 `export` statements to CommonJS `module.exports` statements\. For example, convert:

  ```
  export { s3 }
  ```

  To its CommonJS equivalent:

  ```
  module.exports = { s3 }
  ```

The following example demonstrates the code example for creating an Amazon S3 bucket in both ES6 and CommonJS\.

------
#### [ ES6 ]

libs/s3Client\.js

```
// Create service client module using ES6 syntax.
import { S3Client } from "@aws-sdk/client-s3";
// Set the AWS region
const REGION = "eu-west-1"; //e.g. "us-east-1"
// Create Amazon S3 service object.
const s3 = new S3Client({ region: REGION });
// Export 's3' constant.
export { s3 };
```

s3\_createbucket\.js

```
// Get service clients module and commands using ES6 syntax.
import { CreateBucketCommand } from "@aws-sdk/client-s3";
import { s3 } from "./libs/s3Client.js";

// Set the bucket parameters
const bucketParams = { Bucket: "BUCKET_NAME" };

// Create the Amazon S3 bucket.
const run = async () => {
  try {
    const data = await s3.send(new CreateBucketCommand(bucketParams));
    console.log("Success", data.Location);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};

run();
```

------
#### [ CommonJS ]

libs/s3Client\.js

```
// Create service client module using CommonJS syntax.
const { S3Client } = require("@aws-sdk/client-s3");
// Set the AWS Region.
const REGION = "eu-west-1"; //e.g. "us-east-1"
// Create Amazon S3 service object.
const s3 = new S3Client({ region: REGION });
// Export 's3' constant.
module.exports = { s3 };
```

s3\_createbucket\.js

```
// Get service clients module and commands using CommonJS syntax.
const { CreateBucketCommand } = require("@aws-sdk/client-s3");
const { s3 } = require("./libs/s3Client.js");

// Set the bucket parameters
const bucketParams = { Bucket: "BUCKET_NAME" };

// Create the Amazon S3 bucket.
const run = async () => {
  try {
    const data = await s3.send(new CreateBucketCommand(bucketParams));
    console.log("Success", data.Location);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};

run();
```

------
