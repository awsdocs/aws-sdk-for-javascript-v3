--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Creating and managing transcoding jobs in MediaConvert<a name="emc-examples-jobs"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to specify the account\-specific endpoint to use with MediaConvert\.
+ How to create transcoding jobs in MediaConvert\.
+ How to cancel a transcoding job\.
+ How to retrieve the JSON for a completed transcoding job\.
+ How to retrieve a JSON array for up to 20 of the most recently created jobs\.

## The scenario<a name="emc-examples-jobs-scenario"></a>

In this example, you use a Node\.js module to call MediaConvert to create and manage transcoding jobs\. The code uses the SDK for JavaScript to do this by using these methods of the MediaConvert client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-mediaconvert/classes/createjobcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-mediaconvert/classes/createjobcommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-mediaconvert/classes/canceljobcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-mediaconvert/classes/canceljobcommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-mediaconvert/classes/getjobcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-mediaconvert/classes/getjobcommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-mediaconvert/classes/listjobscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-mediaconvert/classes/listjobscommand.html)

## Prerequisite tasks<a name="emc-examples-jobs-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/mediaconvert/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create and configure Amazon S3 buckets that provide storage for job input files and output files\. For details, see [Create storage for files](https://docs.aws.amazon.com/mediaconvert/latest/ug/set-up-file-locations.html) in the *AWS Elemental MediaConvert User Guide*\.
+ Upload the input video to the Amazon S3 bucket you provisioned for input storage\. For a list of supported input video codecs and containers, see [Supported input codecs and containers](https://docs.aws.amazon.com/mediaconvert/latest/ug/reference-codecs-containers-input.html) in the *AWS Elemental MediaConvert User Guide*\.
+ Create an IAM role that gives MediaConvert access to your input files and the Amazon S3 buckets where your output files are stored\. For details, see [Set up IAM permissions](https://docs.aws.amazon.com/mediaconvert/latest/ug/iam-role.html) in the *AWS Elemental MediaConvert User Guide*\.

**Important**  
This example uses ECMAScript6 \(ES6\)\. This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.  
However, if you prefer to use CommonJS sytax, please refer to [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)

## Configuring the SDK<a name="emc-examples-jobs-configure-sdk"></a>

Configure the SDK as previously shown, including downloading the required clients and packages\. Because MediaConvert uses custom endpoints for each account, you must also configure the `MediaConvert` client class to use your account\-specific endpoint\. To do this, set the `endpoint` parameter on `mediaconvert(endpoint)`\.

```
// Import required AWS-SDK clients and commands for Node.js
import { CreateJobCommand } from "@aws-sdk/client-mediaconvert";
import { emcClient }  from  "./libs/emcClient.js";
```

## Defining a simple transcoding job<a name="emc-examples-jobs-spec"></a>

Create a `libs` directory, and create a Node\.js module with the file name `emcClient.js`\. Copy and paste the code below into it, which creates the MediaConvert client object\. Replace *REGION* with your AWS Region\. Replace *ENDPOINT* with your MediaConvert account endpoint, which you can on the **Account** page in the MediaConvert console\. 

```
import { MediaConvertClient } from "@aws-sdk/client-mediaconvert";
// Set the AWS Region.
const REGION = "REGION";
// Set the account end point.
const ENDPOINT = { endpoint: "https://ENDPOINT_UNIQUE_STRING.mediaconvert.REGION.amazonaws.com" };
// Set the MediaConvert Service Object
const emcClient = new MediaConvertClient(ENDPOINT);
export { emcClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/mediaconvert/src/libs/emcClient.js)\.

Create a Node\.js module with the file name `emc_createjob.js`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\. Create the JSON that defines the transcode job parameters\.

These parameters are quite detailed\. You can use the [AWS Elemental MediaConvert console](https://console.aws.amazon.com/mediaconvert/) to generate the JSON job parameters by choosing your job settings in the console, and then choosing **Show job JSON** at the bottom of the **Job** section\. This example shows the JSON for a simple job\.

**Note**  
Replace *JOB\_QUEUE\_ARN* with the MediaConvert job queue, *IAM\_ROLE\_ARN* with the Amazon Resource Name \(ARN\) of the IAM role, *OUTPUT\_BUCKET\_NAME* with the destination bucket name \- for example, "s3://OUTPUT\_BUCKET\_NAME/", and *INPUT\_BUCKET\_AND\_FILENAME* with the input bucket and filename \- for example, "s3://INPUT\_BUCKET/FILE\_NAME"\.

```
const params = {
  Queue: "JOB_QUEUE_ARN", //JOB_QUEUE_ARN
  UserMetadata: {
    Customer: "Amazon",
  },
  Role: "IAM_ROLE_ARN", //IAM_ROLE_ARN
  Settings: {
    OutputGroups: [
      {
        Name: "File Group",
        OutputGroupSettings: {
          Type: "FILE_GROUP_SETTINGS",
          FileGroupSettings: {
            Destination: "OUTPUT_BUCKET_NAME", //OUTPUT_BUCKET_NAME, e.g., "s3://BUCKET_NAME/"
          },
        },
        Outputs: [
          {
            VideoDescription: {
              ScalingBehavior: "DEFAULT",
              TimecodeInsertion: "DISABLED",
              AntiAlias: "ENABLED",
              Sharpness: 50,
              CodecSettings: {
                Codec: "H_264",
                H264Settings: {
                  InterlaceMode: "PROGRESSIVE",
                  NumberReferenceFrames: 3,
                  Syntax: "DEFAULT",
                  Softness: 0,
                  GopClosedCadence: 1,
                  GopSize: 90,
                  Slices: 1,
                  GopBReference: "DISABLED",
                  SlowPal: "DISABLED",
                  SpatialAdaptiveQuantization: "ENABLED",
                  TemporalAdaptiveQuantization: "ENABLED",
                  FlickerAdaptiveQuantization: "DISABLED",
                  EntropyEncoding: "CABAC",
                  Bitrate: 5000000,
                  FramerateControl: "SPECIFIED",
                  RateControlMode: "CBR",
                  CodecProfile: "MAIN",
                  Telecine: "NONE",
                  MinIInterval: 0,
                  AdaptiveQuantization: "HIGH",
                  CodecLevel: "AUTO",
                  FieldEncoding: "PAFF",
                  SceneChangeDetect: "ENABLED",
                  QualityTuningLevel: "SINGLE_PASS",
                  FramerateConversionAlgorithm: "DUPLICATE_DROP",
                  UnregisteredSeiTimecode: "DISABLED",
                  GopSizeUnits: "FRAMES",
                  ParControl: "SPECIFIED",
                  NumberBFramesBetweenReferenceFrames: 2,
                  RepeatPps: "DISABLED",
                  FramerateNumerator: 30,
                  FramerateDenominator: 1,
                  ParNumerator: 1,
                  ParDenominator: 1,
                },
              },
              AfdSignaling: "NONE",
              DropFrameTimecode: "ENABLED",
              RespondToAfd: "NONE",
              ColorMetadata: "INSERT",
            },
            AudioDescriptions: [
              {
                AudioTypeControl: "FOLLOW_INPUT",
                CodecSettings: {
                  Codec: "AAC",
                  AacSettings: {
                    AudioDescriptionBroadcasterMix: "NORMAL",
                    RateControlMode: "CBR",
                    CodecProfile: "LC",
                    CodingMode: "CODING_MODE_2_0",
                    RawFormat: "NONE",
                    SampleRate: 48000,
                    Specification: "MPEG4",
                    Bitrate: 64000,
                  },
                },
                LanguageCodeControl: "FOLLOW_INPUT",
                AudioSourceName: "Audio Selector 1",
              },
            ],
            ContainerSettings: {
              Container: "MP4",
              Mp4Settings: {
                CslgAtom: "INCLUDE",
                FreeSpaceBox: "EXCLUDE",
                MoovPlacement: "PROGRESSIVE_DOWNLOAD",
              },
            },
            NameModifier: "_1",
          },
        ],
      },
    ],
    AdAvailOffset: 0,
    Inputs: [
      {
        AudioSelectors: {
          "Audio Selector 1": {
            Offset: 0,
            DefaultSelection: "NOT_DEFAULT",
            ProgramSelection: 1,
            SelectorType: "TRACK",
            Tracks: [1],
          },
        },
        VideoSelector: {
          ColorSpace: "FOLLOW",
        },
        FilterEnable: "AUTO",
        PsiControl: "USE_PSI",
        FilterStrength: 0,
        DeblockFilter: "DISABLED",
        DenoiseFilter: "DISABLED",
        TimecodeSource: "EMBEDDED",
        FileInput: "INPUT_BUCKET_AND_FILENAME", //INPUT_BUCKET_AND_FILENAME, e.g., "s3://BUCKET_NAME/FILE_NAME"
      },
    ],
    TimecodeConfig: {
      Source: "EMBEDDED",
    },
  },
};
```

## Creating a transcoding job<a name="emc-examples-jobs-create"></a>

After creating the job parameters JSON, call the asynchronous `run` method to invoke a `MediaConvert` client service object, passing the parameters\. The ID of the job created is returned in the response `data`\.

```
const run = async () => {
  try {
    const data = await emcClient.send(new CreateJobCommand(params));
    console.log("Job created!", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node emc_createjob.js 
```

This full example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/mediaconvert/src/emc_createjob.js)\.

## Canceling a transcoding job<a name="emc-examples-jobs-cancel"></a>

Create a `libs` directory, and create a Node\.js module with the file name `emcClient.js`\. Copy and paste the code below into it, which creates the MediaConvert client object\. Replace *REGION* with your AWS Region\. Replace *ENDPOINT* with your MediaConvert account endpoint, which you can on the **Account** page in the MediaConvert console\. 

```
import { MediaConvertClient } from "@aws-sdk/client-mediaconvert";
// Set the AWS Region.
const REGION = "REGION";
// Set the account end point.
const ENDPOINT = { endpoint: "https://ENDPOINT_UNIQUE_STRING.mediaconvert.REGION.amazonaws.com" };
// Set the MediaConvert Service Object
const emcClient = new MediaConvertClient(ENDPOINT);
export { emcClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/mediaconvert/src/libs/emcClient.js)\.

Create a Node\.js module with the file name `emc_canceljob.js`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. Create the JSON that includes the ID of the job to cancel\. Then call the `CancelJobCommand` method by creating a promise for invoking an `MediaConvert` client service object, passing the parameters\. Handle the response in the promise callback\.

**Note**  
Replace *JOB\_ID* with the ID of the job to cancel\.

```
// Import required AWS-SDK clients and commands for Node.js
import { CancelJobCommand } from "@aws-sdk/client-mediaconvert";
import { emcClient } from  "./libs/emcClient.js";

// Set the parameters
const params = { Id: "JOB_ID" }; //JOB_ID

const run = async () => {
  try {
    const data = await emcClient.send(new CancelJobCommand(params));
    console.log("Job  " + params.Id + " is canceled");
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node ec2_canceljob.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/mediaconvert/src/emc_canceljob.js)\.

## Listing recent transcoding jobs<a name="emc-examples-jobs-listing"></a>

Create a `libs` directory, and create a Node\.js module with the file name `emcClient.js`\. Copy and paste the code below into it, which creates the MediaConvert client object\. Replace *REGION* with your AWS Region\. Replace *ENDPOINT* with your MediaConvert account endpoint, which you can on the **Account** page in the MediaConvert console\. 

```
import { MediaConvertClient } from "@aws-sdk/client-mediaconvert";
// Set the AWS Region.
const REGION = "REGION";
// Set the account end point.
const ENDPOINT = { endpoint: "https://ENDPOINT_UNIQUE_STRING.mediaconvert.REGION.amazonaws.com" };
// Set the MediaConvert Service Object
const emcClient = new MediaConvertClient(ENDPOINT);
export { emcClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/mediaconvert/src/libs/emcClient.js)\.

Create a Node\.js module with the file name `emc_listjobs.js`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\.

Create the parameters JSON, including values to specify whether to sort the list in `ASCENDING`, or `DESCENDING` order, the Amazon Resource Name \(ARN\) of the job queue to check, and the status of jobs to include\. Then call the `ListJobsCommand` method by creating a promise for invoking an `MediaConvert` client service object, passing the parameters\. 

**Note**  
Replace *QUEUE\_ARN* with the Amazon Resource Name \(ARN\) of the job queue to check, and *STATUS* with the status of the queue\.

```
// Import required AWS-SDK clients and commands for Node.js
import { ListJobsCommand } from  "@aws-sdk/client-mediaconvert";
import { emcClient }  from   "./libs/emcClient.js";

// Set the parameters
const params = {
  MaxResults: 10,
  Order: "ASCENDING",
  Queue: "QUEUE_ARN",
  Status: "SUBMITTED" // e.g., "SUBMITTED"
};

const run = async () => {
  try {
    const data = await emcClient.send(new ListJobsCommand(params));
    console.log("Success. Jobs: ", data.Jobs);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node emc_listjobs.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/mediaconvert/src/emc_listjobs.js)\.