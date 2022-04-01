--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Amazon Transcribe examples using SDK for JavaScript V3<a name="javascript_transcribe_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for JavaScript V3 with Amazon Transcribe\.

*Actions* are code excerpts that show you how to call individual Amazon Transcribe functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple Amazon Transcribe functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w362aac23b9c35c13)

## Actions<a name="w362aac23b9c35c13"></a>

### Delete a medical transcription job<a name="transcribe_DeleteMedicalTranscriptionJob_javascript_topic"></a>

The following code example shows how to delete an Amazon Transcribe Medical transcription job\.

**SDK for JavaScript V3**  
Create the client\.  

```
const { TranscribeClient } = require("@aws-sdk/client-transcribe");
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create anAmazon EC2 service client object.
const transcribeClient = new TranscribeClient({ region: REGION });
export { transcribeClient };
```
Delete a medical transcription job\.  

```
// Import the required AWS SDK clients and commands for Node.js
import { DeleteMedicalTranscriptionJobCommand } from "@aws-sdk/client-transcribe";
import { transcribeClient } from "./libs/transcribeClient.js";

// Set the parameters
export const params = {
  MedicalTranscriptionJobName: "MEDICAL_JOB_NAME", // For example, 'medical_transciption_demo'
};

export const run = async () => {
  try {
    const data = await transcribeClient.send(
      new DeleteMedicalTranscriptionJobCommand(params)
    );
    console.log("Success - deleted");
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/transcribe#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/transcribe-medical-examples-section.html#transcribe-delete-medical-job)\. 
+  For API details, see [DeleteMedicalTranscriptionJob](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/deletemedicaltranscriptionjobcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Delete a transcription job<a name="transcribe_DeleteTranscriptionJob_javascript_topic"></a>

The following code example shows how to delete an Amazon Transcribe transcription job\.

**SDK for JavaScript V3**  
Create the client\.  

```
const { TranscribeClient } = require("@aws-sdk/client-transcribe");
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create anAmazon EC2 service client object.
const transcribeClient = new TranscribeClient({ region: REGION });
export { transcribeClient };
```
Delete a transcription job\.  

```
// Import the required AWS SDK clients and commands for Node.js
import { DeleteTranscriptionJobCommand } from "@aws-sdk/client-transcribe";
import { transcribeClient } from "./libs/transcribeClient.js";

// Set the parameters
export const params = {
  TranscriptionJobName: "JOB_NAME", // Required. For example, 'transciption_demo'
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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/transcribe#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/transcribe-examples-section.html#transcribe-delete-job)\. 
+  For API details, see [DeleteTranscriptionJob](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/deletetranscriptionjobcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### List medical transcription jobs<a name="transcribe_ListMedicalTranscriptionJobs_javascript_topic"></a>

The following code example shows how to list Amazon Transcribe Medical transcription jobs\.

**SDK for JavaScript V3**  
Create the client\.  

```
const { TranscribeClient } = require("@aws-sdk/client-transcribe");
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create anAmazon EC2 service client object.
const transcribeClient = new TranscribeClient({ region: REGION });
export { transcribeClient };
```
List medical transcription jobs\.  

```
// Import the required AWS SDK clients and commands for Node.js
import { StartMedicalTranscriptionJobCommand } from "@aws-sdk/client-transcribe";
import { transcribeClient } from "./libs/transcribeClient.js";

// Set the parameters
export const params = {
  MedicalTranscriptionJobName: "MEDICAL_JOB_NAME", // Required
  OutputBucketName: "OUTPUT_BUCKET_NAME", // Required
  Specialty: "PRIMARYCARE", // Required. Possible values are 'PRIMARYCARE'
  Type: "JOB_TYPE", // Required. Possible values are 'CONVERSATION' and 'DICTATION'
  LanguageCode: "LANGUAGE_CODE", // For example, 'en-US'
  MediaFormat: "SOURCE_FILE_FORMAT", // For example, 'wav'
  Media: {
    MediaFileUri: "SOURCE_FILE_LOCATION",
    // The S3 object location of the input media file. The URI must be in the same region
    // as the API endpoint that you are calling.For example,
    // "https://transcribe-demo.s3-REGION.amazonaws.com/hello_world.wav"
  },
};

export const run = async () => {
  try {
    const data = await transcribeClient.send(
      new StartMedicalTranscriptionJobCommand(params)
    );
    console.log("Success - put", data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/transcribe#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/transcribe-medical-examples-section.html#transcribe-list-medical-jobs)\. 
+  For API details, see [ListMedicalTranscriptionJobs](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/listmedicaltranscriptionjobscommand.html) in *AWS SDK for JavaScript API Reference*\. 

### List transcription jobs<a name="transcribe_ListTranscriptionJobs_javascript_topic"></a>

The following code example shows how to list Amazon Transcribe transcription jobs\.

**SDK for JavaScript V3**  
Create the client\.  

```
const { TranscribeClient } = require("@aws-sdk/client-transcribe");
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create anAmazon EC2 service client object.
const transcribeClient = new TranscribeClient({ region: REGION });
export { transcribeClient };
```
List transcription jobs\.  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/transcribe#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/transcribe-examples-section.html#transcribe-list-jobs)\. 
+  For API details, see [ListTranscriptionJobs](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/listtranscriptionjobscommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Start a medical transcription job<a name="transcribe_StartMedicalTranscriptionJob_javascript_topic"></a>

The following code example shows how to start an Amazon Transcribe Medical transcription job\.

**SDK for JavaScript V3**  
Create the client\.  

```
const { TranscribeClient } = require("@aws-sdk/client-transcribe");
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create anAmazon EC2 service client object.
const transcribeClient = new TranscribeClient({ region: REGION });
export { transcribeClient };
```
Start a medical transcription job\.  

```
// Import the required AWS SDK clients and commands for Node.js
import { StartMedicalTranscriptionJobCommand } from "@aws-sdk/client-transcribe";
import { transcribeClient } from "./libs/transcribeClient.js";

// Set the parameters
export const params = {
  MedicalTranscriptionJobName: "MEDICAL_JOB_NAME", // Required
  OutputBucketName: "OUTPUT_BUCKET_NAME", // Required
  Specialty: "PRIMARYCARE", // Required. Possible values are 'PRIMARYCARE'
  Type: "JOB_TYPE", // Required. Possible values are 'CONVERSATION' and 'DICTATION'
  LanguageCode: "LANGUAGE_CODE", // For example, 'en-US'
  MediaFormat: "SOURCE_FILE_FORMAT", // For example, 'wav'
  Media: {
    MediaFileUri: "SOURCE_FILE_LOCATION",
    // The S3 object location of the input media file. The URI must be in the same region
    // as the API endpoint that you are calling.For example,
    // "https://transcribe-demo.s3-REGION.amazonaws.com/hello_world.wav"
  },
};

export const run = async () => {
  try {
    const data = await transcribeClient.send(
      new StartMedicalTranscriptionJobCommand(params)
    );
    console.log("Success - put", data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/transcribe#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/transcribe-medical-examples-section.html#transcribe-start-medical-transcription)\. 
+  For API details, see [StartMedicalTranscriptionJob](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/startmedicaltranscriptionjobcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Start a transcription job<a name="transcribe_StartTranscriptionJob_javascript_topic"></a>

The following code example shows how to start an Amazon Transcribe transcription job\.

**SDK for JavaScript V3**  
Create the client\.  

```
const { TranscribeClient } = require("@aws-sdk/client-transcribe");
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create anAmazon EC2 service client object.
const transcribeClient = new TranscribeClient({ region: REGION });
export { transcribeClient };
```
Start a transcription job\.  

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
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/transcribe#code-examples)\. 
+  For more information, see [AWS SDK for JavaScript Developer Guide](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/transcribe-examples-section.html#transcribe-start-transcription)\. 
+  For API details, see [StartTranscriptionJob](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-transcribe/classes/starttranscriptionjobcommand.html) in *AWS SDK for JavaScript API Reference*\. 