--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

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
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#createJob-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#createJob-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#cancelJob-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#cancelJob-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#getJob-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#getJob-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#listJobs-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#listJobs-property)

## Prerequisite tasks<a name="emc-examples-jobs-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/mediaconvert/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these example can also be run in JavaScript\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create and configure Amazon S3 buckets that provide storage for job input files and output files\. For details, see [Create storage for files](https://docs.aws.amazon.com/mediaconvert/latest/ug/set-up-file-locations.html) in the *AWS Elemental MediaConvert User Guide*\.
+ Upload the input video to the Amazon S3 bucket you provisioned for input storage\. For a list of supported input video codecs and containers, see [Supported input codecs and containers](https://docs.aws.amazon.com/mediaconvert/latest/ug/reference-codecs-containers-input.html) in the *AWS Elemental MediaConvert User Guide*\.
+ Create an IAM role that gives MediaConvert access to your input files and the Amazon S3 buckets where your output files are stored\. For details, see [Set up IAM permissions](https://docs.aws.amazon.com/mediaconvert/latest/ug/iam-role.html) in the *AWS Elemental MediaConvert User Guide*\.

## Configuring the SDK<a name="emc-examples-jobs-configure-sdk"></a>

Configure the SDK as previously shown, including downloading the required clients and packages\. Because MediaConvert uses custom endpoints for each account, you must also configure the `MediaConvert` client class to use your account\-specific endpoint\. To do this, set the `endpoint` parameter on `mediaconvert(endpoint)`\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Import required AWS-SDK clients and commands for Node.js
const {
  MediaConvertClient,
  CreateJobCommand
} = require("@aws-sdk/client-mediaconvert");
// Create a new service object and set MediaConvert to customer endpoint
const endpoint = { endpoint: "ACCOUNT_ENDPOINT" }; //ACCOUNT_ENDPOINT
```

## Defining a simple transcoding job<a name="emc-examples-jobs-spec"></a>

Create a Node\.js module with the file name `emc_createjob.ts`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\. Create the JSON that defines the transcode job parameters\.

These parameters are quite detailed\. You can use the [AWS Elemental MediaConvert console](https://console.aws.amazon.com/mediaconvert/) to generate the JSON job parameters by choosing your job settings in the console, and then choosing **Show job JSON** at the bottom of the **Job** section\. This example shows the JSON for a simple job\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *ACCOUNT\_ENDPOINT* with your AWS Region, *JOB\_QUEUE\_ARN* with the MediaConvert job queue, *IAM\_ROLE\_ARN* with the Amazon Resource Name \(ARN\) of the IAM role, *OUTPUT\_BUCKET\_NAME* with the destination bucket name \- for example, "s3://OUTPUT\_BUCKET\_NAME/", and *INPUT\_BUCKET\_AND\_FILENAME* with the input bucket and filename \- for example, "s3://INPUT\_BUCKET/FILE\_NAME"\.

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

//Set the MediaConvert Service Object
const mediaconvert = new MediaConvertClient(endpoint);
```

## Creating a transcoding job<a name="emc-examples-jobs-create"></a>

After creating the job parameters JSON, call the asynchronous `run` method to invoke a `MediaConvert` client service object, passing the parameters\. The ID of the job created is returned in the response `data`\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
const run = async () => {
  try {
    const data = await mediaconvert.send(new CreateJobCommand(params));
    console.log("Job created!", data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node emc_createjob.ts // If you prefer JavaScript, enter 'node emc_createjob.js'
```

This full example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/mediaconvert/src/emc_createjob.ts)\.

## Canceling a transcoding job<a name="emc-examples-jobs-cancel"></a>

Create a Node\.js module with the file name `emc_canceljob.ts`\. Be sure to configure the SDK as previously shown, including downloading the required clients and packages\. Create the JSON that includes the ID of the job to cancel\. Then call the `CancelJobCommand` method by creating a promise for invoking an `MediaConvert` client service object, passing the parameters\. Handle the response in the promise callback\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *ACCOUNT\_ENDPOINT* with the account endpoint, and *JOB\_ID* with the ID of the job to cancel\.

```
// Import required AWS-SDK clients and commands for Node.js
const {
  MediaConvertClient,
  CancelJobCommand
} = require("@aws-sdk/client-mediaconvert");

// Set the parameters
const endpoint = { endpoint: "ACCOUNT_ENDPOINT" }; //ACCOUNT_ENDPOINT
const params = { Id: "JOB_ID" }; //JOB_ID

// Create MediaConvert service object
const mediaconvert = new MediaConvertClient(endpoint);

const run = async () => {
  try {
    const data = await mediaconvert.send(new CancelJobCommand(params));
    console.log("Job  " + params.Id + " is canceled");
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node ec2_canceljob.ts // If you prefer JavaScript, enter 'node ec2_canceljob.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/mediaconvert/src/emc_canceljob.ts)\.

## Listing recent transcoding jobs<a name="emc-examples-jobs-listing"></a>

Create a Node\.js module with the file name `emc_listjobs.ts`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\.

Create the parameters JSON, including values to specify whether to sort the list in `ASCENDING`, or `DESCENDING` order, the Amazon Resource Name \(ARN\) of the job queue to check, and the status of jobs to include\. Then call the `ListJobsCommand` method by creating a promise for invoking an `MediaConvert` client service object, passing the parameters\. 

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *ACCOUNT\_ENDPOINT* with the account endpoint, *QUEUE\_ARN* with the Amazon Resource Name \(ARN\) of the job queue to check, and *STATUS* with the status of the queue\.

```
// Import required AWS-SDK clients and commands for Node.js
const {
  MediaConvertClient,
  ListJobsCommand
} = require("@aws-sdk/client-mediaconvert");

// Set the parameters
const endpoint = { endpoint: "ACCOUNT_END_POINT" }; //ACCOUNT_END_POINT
const params = {
  MaxResults: 10,
  Order: "ASCENDING",
  Queue: "QUEUE_ARN",
  Status: "STATUS", // e.g., "SUBMITTED"
};

//Set the MediaConvert Service Object
const mediaconvert = new MediaConvertClient(endpoint);

const run = async () => {
  try {
    const data = await mediaconvert.send(new ListJobsCommand(params));
    console.log("Success. Jobs: ", data.Jobs);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node emc_listjobs.ts // If you prefer JavaScript, enter 'node emc_listjobs.js'
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/mediaconvert/src/emc_listjobs.ts)\.