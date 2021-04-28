--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Amazon Transcribe medical examples<a name="transcribe-medical-examples-section"></a>

In this example, a series of Node\.js modules are used to create,list, and delete medical transcription jobs using the following methods of the `TranscribeService` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/starttranscriptionjobcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/starttranscriptionjobcommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/listtranscriptionjobscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/listtranscriptionjobscommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/deletetranscriptionjobcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/deletetranscriptionjobcommand.html)

For more information about Amazon Transcribe users, see the [Amazon Transcribe developer guide](https://docs.aws.amazon.com//transcribe/latest/dg/what-is-transcribe.html)\.

## Prerequisite tasks<a name="transcribe-example-transcription-medical-jobs"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/transcribe/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypeScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these examples can also be run in JavaScript\. For more information, see [this article](https://aws.amazon.com/blogs/developer/first-class-typescript-support-in-modular-aws-sdk-for-javascript/) in the AWS Developer Blog\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Starting an Amazon Transcribe medical transcription job<a name="transcribe-start-medical-transcription"></a>

This example demonstrates how to start a Amazon Transcribe medical transcription job using the AWS SDK for JavaScript\. For more information, see [startMedicalTranscriptionJob](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/TranscribeService.html#startMedicalTranscriptionJob-property)\.

Create a Node\.js module with the file name `transcribe-create-medical-job.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `TranscribeService` client service object\. Create a parameters object, specifying the required parameters\. Create an Amazon Transcribe service client object, and start the medical job using the `StartMedicalTranscriptionJobCommand` command\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *MEDICAL\_JOB\_NAME* with a name for the medical transcription job\. For *OUTPUT\_BUCKET\_NAME* specify the Amazon S3 bucket where the output is saved\. For *JOB\_TYPE* specify types of job\. For *SOURCE\_LOCATION* specify the location of the source file\. For *SOURCE\_FILE\_LOCATION* specify the location of the input media file\.

```
// Import the required AWS SDK clients and commands for Node.js
const {
  TranscribeClient,
  StartMedicalTranscriptionJobCommand,
} = require("@aws-sdk/client-transcribe");

// Set the AWS Region
const REGION = "REGION"; // For example, "us-east-1"

// Set the parameters
const params = {
  MedicalTranscriptionJobName: 'MEDICAL_JOB_NAME', // Required
  OutputBucketName: 'OUTPUT_BUCKET_NAME', // Required
  Specialty: "PRIMARYCARE", // Required. Possible values are 'PRIMARYCARE'
  Type: "JOB_TYPE", // Required. Possible values are 'CONVERSATION' and 'DICTATION'
  LanguageCode: "LANGUAGE_CODE", // For example, 'en-US'
  MediaFormat: "SOURCE_FILE_FORMAT", // For example, 'wav'
  Media: {
    MediaFileUri: "SOURCE_FILE_LOCATION",
    // The S3 object location of the input media file. The URI must be in the same region
    // as the API endpoint that you are calling.For example,
    // "https://transcribe-demo.s3-REGION.amazonaws.com/hello_world.wav"
  }
};

// Create an Amazon Transcribe service client object
const client = new TranscribeClient({ region: REGION });

const run = async () => {
  try {
    const data = await client.send(
      new StartMedicalTranscriptionJobCommand(params)
    );
    console.log("Success - put", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node transcribe-create-medical-job.ts  // If you prefer JavaScript, enter 'node transcribe-create-medical-job.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/transcribe/src/transcribe_create_medical_job.ts)\.

## Listing Amazon Transcribe medical jobs<a name="transcribe-list-medical-jobs"></a>

This example shows how to list the Amazon Transcribe transcription jobs using the AWS SDK for JavaScript\. For more information, see [ListTranscriptionMedicalJobsCommand](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/TranscribeService.html#listTranscriptionMedicalJobs-property)\.

Create a Node\.js module with the file name `transcribe-list-medical-jobs.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `TranscribeService` client service object\. Create a paramters object with the required parameters, and list the medical jobs using the `ListMedicalTranscriptionJobsCommand` command\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *KEYWORD* with a keyword that the returned jobs name must contain\.

```
// Import the required AWS SDK clients and commands for Node.js

const {
  TranscribeClient,
  ListMedicalTranscriptionJobsCommand,
} = require("@aws-sdk/client-transcribe");

// Set the AWS Region
const REGION = "REGION"; // For example, "us-east-1"

// Set the parameters
const params = {
  JobNameContains: "KEYWORD", // Returns only transcription job names containing this string
};

// Create an Amazon Transcribe service client object
const client = new TranscribeClient({ region: REGION });

const run = async () => {
  try {
    const data = await client.send(
      new ListMedicalTranscriptionJobsCommand(params)
    );
    console.log("Success", data.MedicalTranscriptionJobName);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node transcribe-list-medical-jobs.ts  // If you prefer JavaScript, enter 'node transcribe-list-medical-jobs.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/transcribe/src/transcribe_list_medical_jobs.ts)\.

## Deleting an Amazon Transcribe medical job<a name="transcribe-delete-medical-job"></a>

This example shows how to delete an Amazon Transcribe transcription job using the AWS SDK for JavaScript\. For more information about optional, see [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/TranscribeService.html#deleteTranscriptionMedicalJob-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/TranscribeService.html#deleteTranscriptionMedicalJob-property)\.

Create a Node\.js module with the file name `transcribe-delete-job.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create a `TranscribeService` client service object\. Create a parameters object with the required parameters, and delete the medical job using the `DeleteMedicalJobCommand` command\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *JOB\_NAME* with the name of the job to delete\. 

```
// Import the required AWS SDK clients and commands for Node.js
const {
  TranscribeClient,
  DeleteMedicalTranscriptionJobCommand,
} = require("@aws-sdk/client-transcribe");

// Set the AWS Region
const REGION = "REGION"; // For example, "us-east-1"

// Set the parameters
const params = {
  MedicalTranscriptionJobName: "MEDICAL_JOB_NAME" // For example, 'medical_transciption_demo'
};

// Create an Amazon Transcribe service client object
const client = new TranscribeClient({ region: REGION });

const run = async () => {
  try {
    const data = await client.send(
      new DeleteMedicalTranscriptionJobCommand(params)
    );
    console.log("Success - deleted");
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node transcribe-delete-medical-job.ts  // If you prefer JavaScript, enter 'node transcribe-delete-medical-job.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/transcribe/src/transcribe_delete_medical_job.ts)\.