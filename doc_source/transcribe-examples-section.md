--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Amazon Transcribe examples<a name="transcribe-examples-section"></a>

In this example, a series of Node\.js modules are used to create,list, and delete transcription jobs using the following methods of the `TranscribeService` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/starttranscriptionjobcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/starttranscriptionjobcommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/listtranscriptionjobscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/listtranscriptionjobscommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/deletetranscriptionjobcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/deletetranscriptionjobcommand.html)

For more information about Amazon Transcribe users, see the [Amazon Transcribe developer guide](https://docs.aws.amazon.com//transcribe/latest/dg/what-is-transcribe.html)\.

## Prerequisite tasks<a name="transcribe-example-transcription-jobs"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/transcribe/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)

## Starting an Amazon Transcribe job<a name="transcribe-start-transcription"></a>

This example demonstrates how to start a Amazon Transcribe transcription job using the AWS SDK for JavaScript\. For more information, see [StartTranscriptionJobCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/starttranscriptionjobcommand.html)\.

Create a `libs` directory, and create a Node\.js module with the file name `transcribeClient.js`\. Copy and paste the code below into it, which creates the Amazon Transcribe client object\. Replace *REGION* with your AWS Region\.

```
const { TranscribeClient } = require("@aws-sdk/client-transcribe");
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create anAmazon EC2 service client object.
const transcribeClient = new TranscribeClient({ region: REGION });
export { transcribeClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/transcribe/src/libs/transcribeClient.js)\.

Create a Node\.js module with the file name `transcribe-create-job.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create a parameters object, specifying the required parameters\. Start the job using the `StartMedicalTranscriptionJobCommand` command\.

**Note**  
Replace *MEDICAL\_JOB\_NAME* with a name for the transcription job\. For *OUTPUT\_BUCKET\_NAME* specify the Amazon S3 bucket where the output is saved\. For *JOB\_TYPE* specify types of job\. For *SOURCE\_LOCATION* specify the location of the source file\. For *SOURCE\_FILE\_LOCATION* specify the location of the input media file\.

```
// Import the required AWS SDK clients and commands for Node.js
import { StartTranscriptionJobCommand } from "@aws-sdk/client-transcribe";
import { transcribeClient } from "./libs/transcribeClient.js";

// Set the parameters
export const params = {
  TranscriptionJobName: "JOB_NAME",
  LanguageCode: "LANGUAGE_CODE", // For example, 'en-US'
  MediaFormat: "SOURCE_FILE_FORMAT", // For example, 'wav'
  Media: {
    MediaFileUri: "SOURCE_LOCATION",
    // For example, "https://transcribe-demo.s3-REGION.amazonaws.com/hello_world.wav"
  },
};

export const run = async () => {
  try {
    const data = await transcribeClient.send(
      new StartTranscriptionJobCommand(params)
    );
    console.log("Success - put", data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node transcribe-create-job.js
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/transcribe/src/transcribe_create_job.js)\.

## List Amazon Transcribe jobs<a name="transcribe-list-jobs"></a>

This example shows how list the Amazon Transcribe transcription jobs using the AWS SDK for JavaScript\. For more information about what other setting you can modify, see [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/listtranscriptionjobscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/listtranscriptionjobscommand.html)\.

Create a `libs` directory, and create a Node\.js module with the file name `transcribeClient.js`\. Copy and paste the code below into it, which creates the Amazon Transcribe client object\. Replace *REGION* with your AWS Region\.

```
const { TranscribeClient } = require("@aws-sdk/client-transcribe");
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create anAmazon EC2 service client object.
const transcribeClient = new TranscribeClient({ region: REGION });
export { transcribeClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/transcribe/src/libs/transcribeClient.js)\.

Create a Node\.js module with the file name `transcribe-list-jobs.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create a parameters object with the required parameters\.

**Note**  
Replace *KEY\_WORD* with a keyword that the returned jobs name must contain\.

```
// Import the required AWS SDK clients and commands for Node.js

import { ListTranscriptionJobsCommand } from "@aws-sdk/client-transcribe";
import { transcribeClient } from "./libs/transcribeClient.js";

// Set the parameters
export const params = {
  JobNameContains: "KEYWORD", // Not required. Returns only transcription
  // job names containing this string
};

export const run = async () => {
  try {
    const data = await transcribeClient.send(
      new ListTranscriptionJobsCommand(params)
    );
    console.log("Success", data.TranscriptionJobSummaries);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node transcribe-list-jobs.js
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/transcribe/src/transcribe_list_jobs.js)\.

## Deleting a Amazon Transcribe job<a name="transcribe-delete-job"></a>

This example shows how to delete an Amazon Transcribe transcription job using the AWS SDK for JavaScript\. For more information about optional, see [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/deletetranscriptionjobcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/deletetranscriptionjobcommand.html)\.

Create a `libs` directory, and create a Node\.js module with the file name `transcribeClient.js`\. Copy and paste the code below into it, which creates the Amazon Transcribe client object\. Replace *REGION* with your AWS Region\.

```
import  { TranscribeClient }  from  "@aws-sdk/client-transcribe";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create Transcribe service object.
const transcribeClient = new TranscribeClient({ region: REGION });
export { transcribeClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/transcribe/src/libs/transcribeClient.js)\.

Create a Node\.js module with the file name `transcribe-delete-job.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Specify the AWS Region, and the name of the job you want to delete\.

**Note**  
Replace *JOB\_NAME* with the name of the job to delete\. 

```
// Import the required AWS SDK clients and commands for Node.js
import { DeleteTranscriptionJobCommand } from "@aws-sdk/client-transcribe";
import { transcribeClient } from "./libs/transcribeClient.js";

// Set the parameters
export const params = {
  TranscriptionJobName: "JOB_NAME", // Required. For example, 'transcription_demo'
};

export const run = async () => {
  try {
    const data = await transcribeClient.send(
      new DeleteTranscriptionJobCommand(params)
    );
    console.log("Success - deleted");
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node transcribe-delete-job.js  
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/transcribe/src/transcribe_delete_job.js)\.