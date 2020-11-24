--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Amazon Transcribe examples<a name="transcribe-examples-section"></a>

In this example, a series of Node\.js modules are used to create,list, and delete transcription jobs using the following methods of the `TranscribeService` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/TranscribeService.html#startTranscriptionJob-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/TranscribeService.html#startTranscriptionJob-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/TranscribeService.html#listTranscriptionJobs-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/TranscribeService.html#listTranscriptionJobs-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/TranscribeService.html#deleteTranscriptionJob-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/TranscribeService.html#deleteTranscriptionJob-property)

For more information about Amazon Transcribe users, see the [Amazon Transcribe developer guide](https://docs.aws.amazon.com//transcribe/latest/dg/what-is-transcribe.html)\.

## Prerequisite tasks<a name="transcribe-example-transcription-jobs"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/transcribe/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Starting an Amazon Transcribe job<a name="transcribe-start-transcription"></a>

This example demonstrates how to start a Amazon Transcribe transcription job using the AWS SDK for JavaScript\. For more information, see [StartTranscriptionJobCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/TranscribeService.html#startTranscriptionJob-property)\.

Create a Node\.js module with the file name `transcribe-create-job.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `TranscribeService` client service object\. Create a parameters object, specifying the required parameters\. Create an Amazon Transcribe service client object, and start the job using the `StartMedicalTranscriptionJobCommand` command\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *MEDICAL\_JOB\_NAME* with a name for the transcription job\. For *OUTPUT\_BUCKET\_NAME* specify the Amazon S3 bucket where the output is saved\. For *JOB\_TYPE* specify types of job\. For *SOURCE\_LOCATION* specify the location of the source file\. For *SOURCE\_FILE\_LOCATION* specify the location of the input media file\.

```
const {
  TranscribeClient,
  StartTranscriptionJobCommand,
} = require("@aws-sdk/client-transcribe");

const client = new TranscribeClient({ region: "REGION" });
const params = {
  TranscriptionJobName: "JOB_NAME",
  LanguageCode: "LANGUAGE_CODE", // For example, 'en-US'
  MediaFormat: "SOURCE_FILE_FORMAT", // For example, 'wav'
  Media: {
    MediaFileUri: "SOURCE_LOCATION",
    // For example, "https://transcribe-demo.s3-REGION.amazonaws.com/hello_world.wav"
  },
};

const run = async () => {
  try {
    const data = await client.send(new StartTranscriptionJobCommand(params));
    console.log("Success - put", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node transcribe-create-job.ts  // If you prefer JavaScript, enter 'node transcribe-create-job.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/transcribe/src/transcribe_create_job.ts)\.

## List Amazon Transcribe jobs<a name="transcribe-list-jobs"></a>

This example shows how list the Amazon Transcribe transcription jobs using the AWS SDK for JavaScript\. For more information about what other setting you can modify, see [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/TranscribeService.html#listTranscriptionJobs-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/TranscribeService.html#listTranscriptionJobs-property)\.

Create a Node\.js module with the file name `transcribe-list-jobs.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `TranscribeService` client service object\. Create a parameters object with the required parameters\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *KEY\_WORD* with a keyword that the returned jobs name must contain\.

```
const {
  TranscribeClient,
  ListTranscriptionJobsCommand,
} = require("@aws-sdk/client-transcribe");

const client = new TranscribeClient({ region: "REGION" });
const params = {
  JobNameContains: "KEY_WORD" // Returns only transcription job names containing this string
};

const run = async () => {
  try {
    const data = await client.send(new ListTranscriptionJobsCommand(params));
    console.log("Success", data.TranscriptionJobSummaries);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node transcribe-list-jobs.ts  // If you prefer JavaScript, enter 'node transcribe-list-jobs.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/transcribe/src/transcribe_list_jobs.ts)\.

## Deleting a Amazon Transcribe job<a name="transcribe-delete-job"></a>

This example shows how to delete an Amazon Transcribe transcription job using the AWS SDK for JavaScript\. For more information about optional, see [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/TranscribeService.html#deleteTranscriptionJob-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/TranscribeService.html#deleteTranscriptionJob-property)\.

Create a Node\.js module with the file name `transcribe-delete-job.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create a `TranscribeService` client service object\. Specify the AWS Region, and the name of the job you want to delete\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *JOB\_NAME* with the name of the job to delete\. 

```
const {
  TranscribeClient,
  DeleteTranscriptionJobCommand,
} = require("@aws-sdk/client-transcribe");

const client = new TranscribeClient({ region: "eu-west-1" });
const params = {
  TranscriptionJobName: "JOB_NAME" // For example, 'transciption_demo'
};

const run = async () => {
  try {
    const data = await client.send(new DeleteTranscriptionJobCommand(params));
    console.log("Success - deleted");
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node transcribe-delete-job.ts  // If you prefer JavaScript, enter 'node transcribe-delete-job.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/transcribe/src/transcribe_delete_job.ts)\.