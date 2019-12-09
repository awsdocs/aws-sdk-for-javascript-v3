# Using Job Templates in MediaConvert<a name="emc-examples-templates"></a>

![\[JavaScript code example that applies to Node.js execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/nodeicon.png)

**This Node\.js code example shows:**
+ How to create AWS Elemental MediaConvert job templates\.
+ How to use a job template to create a transcoding job\.
+ How to list all your job templates\.
+ How to delete job templates\.

## The Scenario<a name="emc-examples-templates-scenario"></a>

The JSON required to create a transcoding job in MediaConvert is detailed, containing a large number of settings\. You can greatly simplify job creation by saving known\-good settings in a job template that you can use to create subsequent jobs\. In this example, you use a Node\.js module to call MediaConvert to create, use, and manage job templates\. The code uses the SDK for JavaScript to do this by using these methods of the MediaConvert client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#createJobTemplate-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#createJobTemplate-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#createJob-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#createJob-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#deleteJobTemplate-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#deleteJobTemplate-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#listJobTemplates-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/MediaConvert.html#listJobTemplates-property)

## Prerequisite Tasks<a name="emc-example-templates-prerequisites"></a>

To set up and run this example, first complete these tasks:
+ Install Node\.js\. For more information, see the [Node\.js website](https://nodejs.org)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a shared credentials file, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md)\.
+ Create an IAM role that gives MediaConvert access to your input files and the Amazon S3 buckets where your output files are stored\. For details, see [Set Up IAM Permissions](https://docs.aws.amazon.com/mediaconvert/latest/ug/iam-role.html) in the *AWS Elemental MediaConvert User Guide*\.

## Creating a Job Template<a name="emc-examples-templates-create"></a>

Create a Node\.js module with the file name `emc_create_jobtemplate.js`\. Be sure to configure the SDK as previously shown\.

Specify the parameters JSON for template creation\. You can use most of the JSON parameters from a previous successful job to specify the `Settings` values in the template\. This example uses the job settings from [Creating and Managing Transcoding Jobs in MediaConvert](emc-examples-jobs.md)\.

Call the `createJobTemplate` method by creating a promise for invoking an `AWS.MediaConvert` service object, passing the parameters\. Then handle the response in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the Region 
AWS.config.update({region: 'us-west-2'});
// Set the custom endpoint for your account
AWS.config.mediaconvert = {endpoint : 'ACCOUNT_ENDPOINT'};

var params = {
  Category: 'YouTube Jobs',
  Description: 'Final production transcode',
  Name: 'DemoTemplate',
  Queue: 'JOB_QUEUE_ARN',
  "Settings": {
    "OutputGroups": [
      {
        "Name": "File Group",
        "OutputGroupSettings": {
          "Type": "FILE_GROUP_SETTINGS",
          "FileGroupSettings": {
            "Destination": "s3://BUCKET_NAME/"
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
      }
    ],
    "TimecodeConfig": {
      "Source": "EMBEDDED"
    }
  }
};

// Create a promise on a MediaConvert object
var templatePromise = new AWS.MediaConvert({apiVersion: '2017-08-29'}).createJobTemplate(params).promise();

// Handle promise's fulfilled/rejected status
templatePromise.then(
  function(data) {
    console.log("Success!", data);
  },
  function(err) {
    console.log("Error", err);
  }
);
```

To run the example, type the following at the command line\.

```
node emc_create_jobtemplate.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/mediaconvert/emc_create_jobtemplate.js)\.

## Creating a Transcoding Job from a Job Template<a name="emc-examples-templates-createjob"></a>

Create a Node\.js module with the file name `emc_template_createjob.js`\. Be sure to configure the SDK as previously shown\.

Create the job creation parameters JSON, including the name of the job template to use, and the `Settings` to use that are specific to the job you're creating\. Then call the `createJobs` method by creating a promise for invoking an `AWS.MediaConvert` service object, passing the parameters\. Handle the response in the promise callback\.

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the Region 
AWS.config.update({region: 'us-west-2'});
// Set the custom endpoint for your account
AWS.config.mediaconvert = {endpoint : 'ACCOUNT_ENDPOINT'};

var params = {
  "Queue": "QUEUE_ARN",
  "JobTemplate": "TEMPLATE_NAME",
  "Role": "ROLE_ARN",
  "Settings": {
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
        "FileInput": "s3://BUCKET_NAME/FILE_NAME"
      }
    ]
  }
};

// Create a promise on a MediaConvert object
var templateJobPromise = new AWS.MediaConvert({apiVersion: '2017-08-29'}).createJob(params).promise();

// Handle promise's fulfilled/rejected status
templateJobPromise.then(
  function(data) {
    console.log("Success! ", data);
  },
  function(err) {
    console.log("Error", err);
  }
);
```

To run the example, type the following at the command line\.

```
node emc_template_createjob.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/mediaconvert/emc_template_createjob.js)\.

## Listing Your Job Templates<a name="emc-examples-templates-listing"></a>

Create a Node\.js module with the file name `emc_listtemplates.js`\. Be sure to configure the SDK as previously shown\.

Create an object to pass the request parameters for the `listTemplates` method of the `AWS.MediaConvert` client class\. Include values to determine what templates to list \(`NAME`, `CREATION DATE`, `SYSTEM`\), how many to list, and their sort order\. To call the `listTemplates` method, create a promise for invoking an MediaConvert service object, passing the parameters\. Then handle the response in the promise callback\. 

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the Region 
AWS.config.update({region: 'us-west-2'});
// Set the customer endpoint
AWS.config.mediaconvert = {endpoint : 'ACCOUNT_ENDPOINT'};

var params = {
  ListBy: 'NAME',
  MaxResults: 10,
  Order: 'ASCENDING',
};

// Create a promise on a MediaConvert object
var listTemplatesPromise = new AWS.MediaConvert({apiVersion: '2017-08-29'}).listJobTemplates(params).promise();

// Handle promise's fulfilled/rejected status
listTemplatesPromise.then(
  function(data) {
    console.log("Success ", data);
  },
  function(err) {
    console.log("Error", err);
  }
);
```

To run the example, type the following at the command line\.

```
node emc_listtemplates.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/mediaconvert/emc_template_createjob.js)\.

## Deleting a Job Template<a name="emc-examples-templates-delete"></a>

Create a Node\.js module with the file name `emc_deletetemplate.js`\. Be sure to configure the SDK as previously shown\.

Create an object to pass the name of the job template you want to delete as parameters for the `deleteJobTemplate` method of the `AWS.MediaConvert` client class\. To call the `deleteJobTemplate` method, create a promise for invoking an MediaConvert service object, passing the parameters\. Handle the response in the promise callback\. 

```
// Load the AWS SDK for Node.js
var AWS = require('aws-sdk');
// Set the Region 
AWS.config.update({region: 'us-west-2'});
// Set the customer endpoint
AWS.config.mediaconvert = {endpoint : 'ACCOUNT_ENDPOINT'};

var params = {
  Name: 'TEMPLATE_NAME'
};

// Create a promise on a MediaConvert object
var deleteTemplatePromise = new AWS.MediaConvert({apiVersion: '2017-08-29'}).deleteJobTemplate(params).promise();

// Handle promise's fulfilled/rejected status
deleteTemplatePromise.then(
  function(data) {
    console.log("Success ", data);
  },
  function(err) {
    console.log("Error", err);
  }
);
```

To run the example, type the following at the command line\.

```
node emc_deletetemplate.js
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascript/example_code/mediaconvert/emc_deletetemplate.js)\.