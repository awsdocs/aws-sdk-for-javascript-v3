# Creating and Managing Transcoding Jobs in MediaConvert<a name="emc-examples-jobs"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to specify the account\-specific endpoint to use with MediaConvert\.
+ How to create transcoding jobs in MediaConvert\.
+ How to cancel a transcoding job\.
+ How to retrieve the JSON for a completed transcoding job\.
+ How to retrieve a JSON array for up to 20 of the most recently created jobs\.

## The Scenario<a name="emc-examples-jobs-scenario"></a>

In this example, you use a Node\.js module to call MediaConvert to create and manage transcoding jobs\. The code uses the SDK for JavaScript to do this by using these methods of the MediaConvert client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#createJob-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#createJob-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#cancelJob-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#cancelJob-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#getJob-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#getJob-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#listJobs-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#listJobs-property)

## Prerequisite Tasks<a name="emc-examples-jobs-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Install Node\.js\. For more information, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.
+ Create and configure Amazon S3 buckets that provide storage for job input files and output files\. For details, see [Create Storage for Files](https://docs.aws.amazon.com/mediaconvert/latest/ug/set-up-file-locations.html) in the *AWS Elemental MediaConvert User Guide*\.
+ Upload the input video to the Amazon S3 bucket you provisioned for input storage\. For a list of supported input video codecs and containers, see [Supported Input Codecs and Containers](https://docs.aws.amazon.com/mediaconvert/latest/ug/reference-codecs-containers-input.html) in the *AWS Elemental MediaConvert User Guide*\.
+ Create an IAM role that gives MediaConvert access to your input files and the Amazon S3 buckets where your output files are stored\. For details, see [Set Up IAM Permissions](https://docs.aws.amazon.com/mediaconvert/latest/ug/iam-role.html) in the *AWS Elemental MediaConvert User Guide*\.

## Configuring the SDK<a name="emc-examples-jobs-configure-sdk"></a>

Configure the SDK for JavaScript by creating a global configuration object, and then setting the Region for your code\. In this example, the Region is set to `us-west-2`\. Because MediaConvert uses custom endpoints for each account, you must also configure the `AWS.MediaConvert` client class to use your account\-specific endpoint\. To do this, set the `endpoint` parameter on `AWS.config.mediaconvert`\.

```
// Load the SDK for JavaScript
var AWS = require('aws-sdk');
// Set the Region 
AWS.config.update({region: 'us-west-2'});
// Set the custom endpoint for your account
AWS.config.mediaconvert = {endpoint : 'ACCOUNT_ENDPOINT'};
```

## Defining a Simple Transcoding Job<a name="emc-examples-jobs-spec"></a>

Create a Node\.js module with the file name `emc_createjob.js`\. Be sure to configure the SDK as previously shown\. Create the JSON that defines the transcode job parameters\.

These parameters are quite detailed\. You can use the [AWS Elemental MediaConvert console](https://console.aws.amazon.com/mediaconvert/) to generate the JSON job parameters by choosing your job settings in the console, and then choosing **Show job JSON** at the bottom of the **Job** section\. This example shows the JSON for a simple job\.

```
var params = {
  "Queue": "JOB_QUEUE_ARN",
  "UserMetadata": {
    "Customer": "Amazon"
  },
  "Role": "IAM_ROLE_ARN",
  "Settings": {
    "OutputGroups": [
      {
        "Name": "File Group",
        "OutputGroupSettings": {
          "Type": "FILE_GROUP_SETTINGS",
          "FileGroupSettings": {
            "Destination": "s3://OUTPUT_BUCKET_NAME/"
          }
        },
        "Outputs": [
          {
            "VideoDescription": {
              "ScalingBehavior": "DEFAULT",
              "TimecodeInsertion": "DISABLED",
              "AntiAlias": "ENABLED",
              "Sharpness": 50,
              "CodecSettings": {
                "Codec": "H_264",
                "H264Settings": {
                  "InterlaceMode": "PROGRESSIVE",
                  "NumberReferenceFrames": 3,
                  "Syntax": "DEFAULT",
                  "Softness": 0,
                  "GopClosedCadence": 1,
                  "GopSize": 90,
                  "Slices": 1,
                  "GopBReference": "DISABLED",
                  "SlowPal": "DISABLED",
                  "SpatialAdaptiveQuantization": "ENABLED",
                  "TemporalAdaptiveQuantization": "ENABLED",
                  "FlickerAdaptiveQuantization": "DISABLED",
                  "EntropyEncoding": "CABAC",
                  "Bitrate": 5000000,
                  "FramerateControl": "SPECIFIED",
                  "RateControlMode": "CBR",
                  "CodecProfile": "MAIN",
                  "Telecine": "NONE",
                  "MinIInterval": 0,
                  "AdaptiveQuantization": "HIGH",
                  "CodecLevel": "AUTO",
                  "FieldEncoding": "PAFF",
                  "SceneChangeDetect": "ENABLED",
                  "QualityTuningLevel": "SINGLE_PASS",
                  "FramerateConversionAlgorithm": "DUPLICATE_DROP",
                  "UnregisteredSeiTimecode": "DISABLED",
                  "GopSizeUnits": "FRAMES",
                  "ParControl": "SPECIFIED",
                  "NumberBFramesBetweenReferenceFrames": 2,
                  "RepeatPps": "DISABLED",
                  "FramerateNumerator": 30,
                  "FramerateDenominator": 1,
                  "ParNumerator": 1,
                  "ParDenominator": 1
                }
              },
              "AfdSignaling": "NONE",
              "DropFrameTimecode": "ENABLED",
              "RespondToAfd": "NONE",
              "ColorMetadata": "INSERT"
            },
            "AudioDescriptions": [
              {
                "AudioTypeControl": "FOLLOW_INPUT",
                "CodecSettings": {
                  "Codec": "AAC",
                  "AacSettings": {
                    "AudioDescriptionBroadcasterMix": "NORMAL",
                    "RateControlMode": "CBR",
                    "CodecProfile": "LC",
                    "CodingMode": "CODING_MODE_2_0",
                    "RawFormat": "NONE",
                    "SampleRate": 48000,
                    "Specification": "MPEG4",
                    "Bitrate": 64000
                  }
                },
                "LanguageCodeControl": "FOLLOW_INPUT",
                "AudioSourceName": "Audio Selector 1"
              }
            ],
            "ContainerSettings": {
              "Container": "MP4",
              "Mp4Settings": {
                "CslgAtom": "INCLUDE",
                "FreeSpaceBox": "EXCLUDE",
                "MoovPlacement": "PROGRESSIVE_DOWNLOAD"
              }
            },
            "NameModifier": "_1"
          }
        ]
      }
    ],
    "AdAvailOffset": 0,
    "Inputs": [
      {
        "AudioSelectors": {
          "Audio Selector 1": {
            "Offset": 0,
            "DefaultSelection": "NOT_DEFAULT",
            "ProgramSelection": 1,
            "SelectorType": "TRACK",
            "Tracks": [
              1
            ]
          }
        },
        "VideoSelector": {
          "ColorSpace": "FOLLOW"
        },
        "FilterEnable": "AUTO",
        "PsiControl": "USE_PSI",
        "FilterStrength": 0,
        "DeblockFilter": "DISABLED",
        "DenoiseFilter": "DISABLED",
        "TimecodeSource": "EMBEDDED",
        "FileInput": "s3://INPUT_BUCKET_AND_FILE_NAME"
      }
    ],
    "TimecodeConfig": {
      "Source": "EMBEDDED"
    }
  }
};
```

## Creating a Transcoding Job<a name="emc-examples-jobs-create"></a>

After creating the job parameters JSON, call the `createJob` method by creating a promise for invoking an `AWS.MediaConvert` service object, passing the parameters\. Then handle the response in the promise callback\. The ID of the job created is returned in the response `data`\.

```
// Create a promise on a MediaConvert object
var endpointPromise = new AWS.MediaConvert({apiVersion: '2017-08-29'}).createJob(params).promise();

// Handle promise's fulfilled/rejected status
endpointPromise.then(
  function(data) {
    console.log("Job created! ", data);
  },
  function(err) {
    console.log("Error", err);
  }
);
```

To run the example, type the following at the command line\.

```
node emc_createjob.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/mediaconvert/emc_createjob.js)\.

## Canceling a Transcoding Job<a name="emc-examples-jobs-cancel"></a>

Create a Node\.js module with the file name `emc_canceljob.js`\. Be sure to configure the SDK as previously shown\. Create the JSON that includes the ID of the job to cancel\. Then call the `cancelJob` method by creating a promise for invoking an `AWS.MediaConvert` service object, passing the parameters\. Handle the response in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the Region 
AWS.config.update({region: 'us-west-2'});
// Set MediaConvert to customer endpoint
AWS.config.mediaconvert = {endpoint : 'ACCOUNT_ENDPOINT'};


var params = {
  Id: 'JOB_ID' /* required */
};


// Create a promise on a MediaConvert object
var endpointPromise = new AWS.MediaConvert({apiVersion: '2017-08-29'}).cancelJob(params).promise();

// Handle promise's fulfilled/rejected status
endpointPromise.then(
  function(data) {
    console.log("Job  " + params.Id + " is canceled");
  },
  function(err) {
    console.log("Error", err);
  }
);
```

To run the example, type the following at the command line\.

```
node ec2_canceljob.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/mediaconvert/emc_canceljob.js)\.

## Listing Recent Transcoding Jobs<a name="emc-examples-jobs-listing"></a>

Create a Node\.js module with the file name `emc_listjobs.js`\. Be sure to configure the SDK as previously shown\.

Create the parameters JSON, including values to specify whether to sort the list in `ASCENDING`, or `DESCENDING` order, the ARN of the job queue to check, and the status of jobs to include\. Then call the `listJobs` method by creating a promise for invoking an `AWS.MediaConvert` service object, passing the parameters\. Handle the response in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the Region 
AWS.config.update({region: 'us-west-2'});
// Set the customer endpoint
AWS.config.mediaconvert = {endpoint : 'ACCOUNT_ENDPOINT'};


var params = {
  MaxResults: 10,
  Order: 'ASCENDING',
  Queue: 'QUEUE_ARN',
  Status: 'SUBMITTED'
};

// Create a promise on a MediaConvert object
var endpointPromise = new AWS.MediaConvert({apiVersion: '2017-08-29'}).listJobs(params).promise();

// Handle promise's fulfilled/rejected status
endpointPromise.then(
  function(data) {
    console.log("Jobs: ", data);
  },
  function(err) {
    console.log("Error", err);
  }
);
```

To run the example, type the following at the command line\.

```
node emc_listjobs.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/mediaconvert/emc_listjobs.js)\.