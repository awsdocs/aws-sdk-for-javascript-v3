--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Prepare and create the Lambda function<a name="using-lambda-function-prep"></a>

In this task, you create the AWS Lambda function used by the application\.

![\[JavaScript running in a browser invoking a Lambda function\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/create-lambda-function.png)![\[JavaScript running in a browser invoking a Lambda function\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[JavaScript running in a browser invoking a Lambda function\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

The Lambda function is invoked by the browser script every time the player of the game clicks and releases the handle on the side of the machine\. There are 16 possible results that can appear in each slot position, chosen at random\. 

The Lambda function generates three random results, one for each slot wheel in the game\. For each result, the Lambda function calls the Amazon DynamoDB table to retrieve the file name of the result graphic\. When all three results are determined and their matching graphic URLs are retrieved, the result information is returned to the browser script for showing the result\.

The Node\.js code needed for the Lambda function is in the `src` directory\. You must edit this code to use it in your Lambda function\. Completing this portion of the application requires you to do these things:
+ Edit the Node\.js code used by the Lambda function\.
+ Compress the Node\.js code into a \.zip archive file that you then upload to the Amazon S3 bucket used by the application\.
+ Edit the Node\.js setup script that creates the Lambda function\.
+ Run the setup script, which creates the Lambda function from the \.zip archive file in the Amazon S3 bucket\.

**To make the necessary edits in the Node\.js code of the Lambda function**

1. Open `slotpull.ts` in the `src` directory in a text editor\.

1. Find this line of code in the browser script\.

   ```
   TableName: = "TABLE_NAME";
   ```

1. Replace *TABLE\_NAME* with your DynamoDB table name\.

1. Save and close the file\.

**To prepare the Node\.js code for creating the Lambda function**

1. Compress `slotpull.ts` into a \.zip archive file for creating the Lambda function\.

1. Upload `slotpull.js.zip` to the Amazon S3 bucket you created for this app\. You can use the following CLI command, where *BUCKET* is the name of your Amazon S3 bucket\.

   ```
   aws s3 cp slotpull.js.zip s3://BUCKET
   ```

## Lambda function code<a name="using-lambda-function-code"></a>

The code creates a JSON object to package the result for the browser script in the application\. Next, it generates three random integer values that are used to look up items in the DynamoDB table, and obtains file names of images in the Amazon S3 bucket\. The result JSON is populated and passed back to the Lambda function caller\.

## Creating the Lambda function<a name="using-lambda-function-creation"></a>

You can provide the Node\.js code for the Lambda function in a file compressed into a \.zip archive file that you upload to an Amazon S3 bucket\. The `slotassets` directory contains the Node\.js script `lambda-function-setup.ts` that you can modify and run to create the Lambda function\.

**To edit the Node\.js setup script for creating the Lambda function**

1. In a text editor open `lambda-function-setup.ts` in the `slotassets` directory\. 

1. Find this line in the script\. 

   ```
      S3Bucket: 'BUCKET_NAME',
   ```

   Replace *BUCKET\_NAME* with the name of the Amazon S3 bucket that contains the \.zip archive file of the Lambda function code\.

1. Find this line in the script\. 

   ```
      S3Key: 'ZIP_FILE_NAME',
   ```

   Replace *ZIP\_FILE\_NAME* with the name of the \.zip archive file of the Lambda function code in the Amazon S3 bucket\.

1. Find this line in the script\. 

   ```
      Role: 'ROLE_ARN',
   ```

   Replace *ROLE\_ARN* with the ARN of the execution role you just created\.

1. Save and close the file\.

**To run the setup script and create the Lambda function from the \.zip archive file in the Amazon S3 bucket**
+ At the command line, type the following\.

  ```
  node lambda-function-setup.ts
  ```

## Creation script code<a name="using-lambda-function-create-script"></a>

The following is the script that creates the Lambda function\. The code assumes you uploaded the \.zip archive file of the Lambda function to the Amazon S3 bucket you created for the application\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Load the Lambda client
const {
  LambdaClient,
  CreateFunctionCommand
} = require("@aws-sdk/client-lambda");

//Set the AWS Region
const REGION = "REGION"; //e.g. "us-east-1"

// Instantiate a Lambda client
const lambda = new LambdaClient({ region: REGION });

//Set the parameters
const params = {
  Code: {
    S3Bucket: "BUCKET_NAME", // BUCKET_NAME
    S3Key: "ZIP_FILE_NAME", // ZIP_FILE_NAME
  },
  FunctionName: "slotpull",
  Handler: "index.handler",
  Role: "IAM_ROLE_ARN", // IAM_ROLE_ARN; e.g., arn:aws:iam::650138640062:role/v3-lambda-tutorial-lambda-role
  Runtime: "nodejs12.x",
  Description: "Slot machine game results generator",
};

const run = async () => {
  try {
    const data = await lambda.send(new CreateFunctionCommand(params));
    console.log("Success", data); // successful response
  } catch (err) {
    console.log("Error", err); // an error occurred
  }
};
run();
```

Choose **next** to continue the tutorial\.

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/lambda/src/lambda-function-setup.ts)\.