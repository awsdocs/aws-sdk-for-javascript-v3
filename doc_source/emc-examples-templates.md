--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Using job templates in MediaConvert<a name="emc-examples-templates"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create AWS Elemental MediaConvert job templates\.
+ How to use a job template to create a transcoding job\.
+ How to list all your job templates\.
+ How to delete job templates\.

## The scenario<a name="emc-examples-templates-scenario"></a>

The JSON required to create a transcoding job in MediaConvert is detailed, containing a large number of settings\. You can greatly simplify job creation by saving known\-good settings in a job template that you can use to create subsequent jobs\. In this example, you use a Node\.js module to call MediaConvert to create, use, and manage job templates\. The code uses the SDK for JavaScript to do this by using these methods of the MediaConvert client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-mediaconvert/classes/createjobtemplatecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-mediaconvert/classes/createjobtemplatecommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-mediaconvert/classes/createjobcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-mediaconvert/classes/createjobcommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-mediaconvert/classes/deletejobtemplatecommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-mediaconvert/classes/deletejobtemplatecommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-mediaconvert/classes/listjobtemplatescommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-mediaconvert/classes/listjobtemplatescommand.html)

## Prerequisite tasks<a name="emc-example-templates-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/mediaconvert/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.
+ Create an IAM role that gives MediaConvert access to your input files and the Amazon S3 buckets where your output files are stored\. For details, see [Set up IAM permissions](https://docs.aws.amazon.com/mediaconvert/latest/ug/iam-role.html) in the *AWS Elemental MediaConvert User Guide*\.

**Important**  
These examples use ECMAScript6 \(ES6\)\. This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.  
However, if you prefer to use CommonJS syntax, please refer to [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)

## Creating a job template<a name="emc-examples-templates-create"></a>

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

Create a Node\.js module with the file name `emc_create_jobtemplate.js`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\.

Specify the parameters JSON for template creation\. You can use most of the JSON parameters from a previous successful job to specify the `Settings` values in the template\. This example uses the job settings from [Creating and managing transcoding jobs in MediaConvert](emc-examples-jobs.md)\.

Call the `CreateJobTemplateCommand` method by creating a promise for invoking an `MediaConvert` client service object, passing the parameters\.

**Note**  
Replace *JOB\_QUEUE\_ARN* with the Amazon Resource Name \(ARN\) of the job queue to check, and *BUCKET\_NAME* with the name of the destination Amazon S3 bucket \- for example, "s3://BUCKET\_NAME/"\. 

```
// Import required AWS-SDK clients and commands for Node.js
import { CreateJobTemplateCommand } from "@aws-sdk/client-mediaconvert";
import { emcClient }  from "./libs/emcClient.js";

const params = {
  Category: "YouTube Jobs",
  Description: "Final production transcode",
  Name: "DemoTemplate",
  Queue: "JOB_QUEUE_ARN", //JOB_QUEUE_ARN
  Settings: {
    OutputGroups: [
      {
        Name: "File Group",
        OutputGroupSettings: {
          Type: "FILE_GROUP_SETTINGS",
          FileGroupSettings: {
            Destination: "BUCKET_NAME", // BUCKET_NAME e.g., "s3://BUCKET_NAME/"
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
      },
    ],
    TimecodeConfig: {
      Source: "EMBEDDED",
    },
  },
};

const run = async () => {
  try {
    // Create a promise on a MediaConvert object
    const data = await emcClient.send(new CreateJobTemplateCommand(params));
    console.log("Success!", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node emc_create_jobtemplate.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/mediaconvert/src/emc_create_jobtemplate.js)\.

## Creating a transcoding job from a job template<a name="emc-examples-templates-createjob"></a>

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

Create a Node\.js module with the file name `emc_template_createjob.js`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\.

Create the job creation parameters JSON, including the name of the job template to use, and the `Settings` to use that are specific to the job you're creating\. Then call the `CreateJobsCommand` method by creating a promise for invoking an `MediaConvert` client service object, passing the parameters\. 

**Note**  
Replace *JOB\_QUEUE\_ARN* with the Amazon Resource Name \(ARN\) of the job queue to check, *KEY\_PAIR\_NAME* with, *TEMPLATE\_NAME* with, *ROLE\_ARN* with the Amazon Resource Name \(ARN\) of the role, and * INPUT\_BUCKET\_AND\_FILENAME* with the input bucket and filename \- for example, "s3://BUCKET\_NAME/FILE\_NAME"\.

```
// Import required AWS-SDK clients and commands for Node.js
import { CreateJobCommand } from  "@aws-sdk/client-mediaconvert";
import { emcClient } from  "./libs/emcClient.js";

const params = {
  Queue: "QUEUE_ARN", //QUEUE_ARN
  JobTemplate: "TEMPLATE_NAME", //TEMPLATE_NAME
  Role: "ROLE_ARN", //ROLE_ARN
  Settings: {
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
  },
};

const run = async () => {
  try {
    const data = await emcClient.send(new CreateJobCommand(params));
    console.log("Success! ", data);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node emc_template_createjob.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/mediaconvert/src/emc_template_createjob.js)\.

## Listing your job templates<a name="emc-examples-templates-listing"></a>

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

Create a Node\.js module with the file name `emc_listtemplates.js`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the request parameters for the `listTemplates` method of the `MediaConvert` client class\. Include values to determine what templates to list \(`NAME`, `CREATION DATE`, `SYSTEM`\), how many to list, and their sort order\. To call the `ListTemplatesCommand` method, create a promise for invoking an MediaConvert client service object, passing the parameters\. 

```
// Import required AWS-SDK clients and commands for Node.js
import { ListJobTemplatesCommand } from  "@aws-sdk/client-mediaconvert";
import { emcClient } from "./libs/emcClient.js";

const params = {
  ListBy: "NAME",
  MaxResults: 10,
  Order: "ASCENDING",
};

const run = async () => {
  try {
    const data = await emcClient.send(new ListJobTemplatesCommand(params));
    console.log("Success ", data.JobTemplates);
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node emc_listtemplates.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/mediaconvert/src/emc_template_createjob.js)\.

## Deleting a job template<a name="emc-examples-templates-delete"></a>

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

Create a Node\.js module with the file name `emc_deletetemplate.js`\. Be sure to configure the SDK as previously shown, including installing the required clients and packages\.

Create an object to pass the name of the job template you want to delete as parameters for the `DeleteJobTemplateCommand` method of the `MediaConvert` client class\. To call the `DeleteJobTemplateCommand` method, create a promise for invoking an MediaConvert client service object, passing the parameters\. 

```
// Import required AWS-SDK clients and commands for Node.js
import { DeleteJobTemplateCommand } from  "@aws-sdk/client-mediaconvert";
import { emcClient } from  "./libs/emcClient.js";

// Set the parameters
const params = { Name: "test" }; //TEMPLATE_NAME

const run = async () => {
  try {
    const data = await emcClient.send(new DeleteJobTemplateCommand(params));
    console.log(
      "Success, template deleted! Request ID:",
      data.$metadata.requestId
    );
    return data;
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node emc_deletetemplate.js 
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/mediaconvert/src/emc_deletetemplate.js)\.