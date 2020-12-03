--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Getting started in a browser script<a name="getting-started-browser"></a>

This section walks you through an example that demonstrate how to run version 3 \(V3\) of the SDK for JavaScript in the browser\. 

**Note**  
Running V3 in the browser is slightly different from version 2 \(V2\)\. For more information, see [Using browsers in V3](welcome.md#v3_browsers)\.

For other examples of using \(V3\) of the SDK for JavaScript with the Node\.js in the browser, see:
+  [Viewing photos in an Amazon S3 bucket from a browser](s3-example-photos-view.md)
+  [Uploading photos to Amazon S3 from a browser](s3-example-photo-album.md)
+  [Tutorial: Build an app to submit data to DynamoDB](cross-service-example-submitting-data.md)

![\[JavaScript code example that applies to browser execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/browsericon.png)

**This browser script example shows you:**
+ How to access AWS services from a browser script using Amazon Cognito Identity\.
+ How to turn text into synthesized speech using Amazon Polly\.
+ How to use a presigner object to create a presigned URL\.

## The Scenario<a name="getting-started-browser-scenario"></a>

Amazon Polly is a cloud service that converts text into lifelike speech\. You can use Amazon Polly to develop applications that increase engagement and accessibility\. Amazon Polly supports multiple languages and includes a variety of lifelike voices\. For more information about Amazon Polly, see the [Amazon Polly Developer Guide](https://docs.aws.amazon.com/polly/latest/dg/)\.

This example shows you how to set up and run a browser script that takes text, sends that text to Amazon Polly, and returns the URL of the synthesized audio of the text for you to play\. The browser script uses an Amazon Cognito Identity pool to provide credentials needed to access AWS services\. The example demonstrates the basic patterns for loading and using the SDK for JavaScript in browser scripts\.

**Note**  
You must run this example in a browser that supports HTML 5 audio to playback the synthesized speech\.

![\[Illustration of how a browser script interacts with Amazon Cognito Identity and Amazon Polly services\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/browserscenario.png)

The browser script uses the SDK for JavaScript to synthesize text by using the following APIs:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity/classes/cognitoidentityclient.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity/classes/cognitoidentityclient.html) constructor
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-polly/classes/polly.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-polly/classes/polly.html) constructor
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/modules/_aws_sdk_polly_request_presigner.html#getsynthesizespeechurl-1](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/modules/_aws_sdk_polly_request_presigner.html#getsynthesizespeechurl-1)

**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypeScript, so for consistency this example is presented in TypeScript\. TypeScript extends JavaScript, so this example can also be run in JavaScript\. For more information, see [this article](https://aws.amazon.com/blogs/developer/first-class-typescript-support-in-modular-aws-sdk-for-javascript/) in the AWS Developer Blog\.

## Step 1: Create an Amazon Cognito Identity Pool<a name="getting-started-browser-create-identity-pool"></a>

In this exercise, you create and use an Amazon Cognito Identity pool to provide unauthenticated access to your browser script for the Amazon Polly service\. Creating an identity pool also creates two AWS Identity and Access Management \(IAM\) roles, one to support users authenticated by an identity provider and the other to support unauthenticated guest users\.

In this exercise, we will only work with the unauthenticated user role to keep the task focused\. You can integrate support for an identity provider and authenticated users later\.

**To create an Amazon Cognito Identity pool**

1. Sign in to the AWS Management Console and open the Amazon Cognito console at [https://console\.aws\.amazon\.com/cognito/\.](https://console.aws.amazon.com/cognito/)

1. Choose **Manage Identity Pools** on the console opening page\.

1. On the next page, choose **Create new identity pool**\.
**Note**  
If there are no other identity pools, the Amazon Cognito console will skip this page and open the next page instead\.

1. In the **Getting started wizard**, type a name for your identity pool in **Identity pool name**\.

1. Choose **Enable access to unauthenticated identities**\.

1. Choose **Create Pool**\.

1. On the next page, choose **View Details** to see the names of the two IAM roles created for your identity pool\. Make a note of the name of the role for unauthenticated identities\. You need this name to add the required policy for Amazon Polly\.

1. Choose **Allow**\.

1. On the **Sample code** page, select the Platform of *JavaScript*\. Then, copy or write down the identity pool ID and the Region\. You need these values to replace REGION and IDENTITY\_POOL\_ID in your browser script\.

After you create your Amazon Cognito identity pool, you're ready to add permissions for Amazon Polly that are needed by your browser script\.

## Step 2: Add a Policy to the Created IAM Role<a name="getting-started-browser-iam-role"></a>

To enable browser script access to Amazon Polly for speech synthesis, use the unauthenticated IAM role created for your Amazon Cognito identity pool\. This requires you to add an IAM policy to the role\. For more information about IAM roles, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

**To add an Amazon Polly policy to the IAM role associated with unauthenticated users**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation panel on the left of the page, choose **Roles**\.

1. In the list of IAM roles, click the link for the unauthenticated identities role previously created by Amazon Cognito\.

1. In the **Summary** page for this role, choose **Attach policies**\.

1. In the **Attach Permissions** page for this role, find and then select the check box for **AmazonPollyFullAccess**\.
**Note**  
You can use this process to enable access to any Amazon or AWS service\.

1. Choose **Attach policy**\.

After you create your Amazon Cognito identity pool and add permissions for Amazon Polly to your IAM role for unauthenticated users, you are ready to build the webpage and browser script\.

## Step 3: Create a project environment<a name="browserstart-prerequisites"></a>

Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/polly/README.md)\. 

## Step 4: Create the HTML Page<a name="getting-started-browser-create-html"></a>

The sample app consists of a single HTML page that contains the user interface, and a JavaScript file that contains the required JavsScript\. To begin, create an HTML document and copy the following contents into it\. The page includes an input field and button, an `<audio>` element to play the synthesized speech, and a `<p>` element to display messages\. \(Note that the full example is shown at the bottom of this page\.\)

The `<script>` element adds the `main.js` file, which contains all the required JavaScript for the example\.

You use webpack to create the `main.js` file, as described in [Step 5: Write the JavaScript](#getting-started-browser-write-sample)\.

For more information about the `<audio>` element, see [audio](https://www.w3schools.com/tags/tag_audio.asp)\.

The full HTML page is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/polly/src/polly.html)\. 

Save the HTML file, naming it `polly.html`\. After you have created the user interface for the application, you're ready to add the browser script code that runs the application\.

To use V3 of the AWS SDK for JavaScript in the browser, you require Webpack to bundle the JavaScript modules and functions, which you installed in the [Step 3: Create a project environment](#browserstart-prerequisites)\.

**Note**  
For information on installing Webpack, see [https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/webpack.html](https://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/webpack.html)\.

## Step 5: Write the JavaScript<a name="getting-started-browser-write-sample"></a>

Copy the following program and paste it into a file named `polly.js`\.

```
const { CognitoIdentityClient } = require("@aws-sdk/client-cognito-identity");
const {
  fromCognitoIdentityPool,
} = require("@aws-sdk/credential-provider-cognito-identity");
const { Polly } = require("@aws-sdk/client-polly");
const { getSynthesizeSpeechUrl } = require("@aws-sdk/polly-request-presigner");

// Create the Polly service client, assigning your credentials
const client = new Polly({
  region: "REGION",
  credentials: fromCognitoIdentityPool({
    client: new CognitoIdentityClient({ region: "REGION" }),
    identityPoolId: "IDENTITY_POOL_ID" // IDENTITY_POOL_ID
  }),
});
// Set the parameters
const speechParams = {
  OutputFormat: "OUTPUT_FORMAT", // For example, 'mp3'
  SampleRate: "SAMPLE_RATE", // For example, '16000
  Text: "", // The 'speakText' function supplies this value
  TextType: "TEXT_TYPE", // For example, "text"
  VoiceId: "POLLY_VOICE" // For example, "Matthew"
};
```

First import the required AWS SDK clients and commands\.

Then create the `Polly` service client object, specifying the credentials for the SDK\.

To synthesize speech with Amazon Polly, you must provide a variety of parameters including the sound format of the output, the sampling rate, the ID of the voice to use, and the text to play back\. When you initially create the parameters, set the `Text:` parameter to an empty string; the `Text:` parameter will be set to the value you retrieve from the `<input>` element in the webpage\.

Next, create a function named `speakText()` that will be invoked as an event handler by the button\.

```
const speakText = async () => {
    // Update the Text parameter with the text entered by the user
    speechParams.Text = document.getElementById("textEntry").value;
    try{
      let url = await getSynthesizeSpeechUrl({
        client, params: speechParams
      });
    console.log(url);
    // Load the URL of the voice recording into the browser
    document.getElementById('audioSource').src = url;
    document.getElementById('audioPlayback').load();
    document.getElementById('result').innerHTML = "Speech ready to play.";
  } catch (err) {
    console.log("Error", err);
    document.getElementById('result').innerHTML = err;
  }
};
// Expose the function to the browser
window.speakText = speakText;
```

Amazon Polly returns synthesized speech as an audio stream\. The easiest way to play that audio in a browser is to have Amazon Polly make the audio available at a presigned URL you can then set as the `src` attribute of the `<audio>` element in the webpage\.

Create a new `Polly` service object\. Then create the `Presigner` object you'll use to create the presigned URL from which the synthesized speech audio can be retrieved\. You must pass the speech parameters that you defined as well as the `Polly` service object that you created to the `Polly.Presigner` constructor\.

After you create the presigner object, call the `getSynthesizeSpeechUrl` method of that object, passing the speech parameters\. If successful, this method returns the URL of the synthesized speech, which you then assign to the `<audio>` element for playback\.

Finally, run the following at the command prompt to bundle the JavaScript for this example in a file named `main.js`:

```
./node_modules/.bin/webpack --entry ./polly.ts --mode development --target web --devtool false -o main.js
```

**Note**  
For information about installing webpack, see [Bundling applications with webpack](webpack.md)\.

The full JavaScript page is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/polly/src/polly.ts)\.

## Step 6: Run the Example<a name="getting-started-browser-run-sample"></a>

To run the example app, load `polly.html` into a web browser\. The app should look similar to the following\.

![\[Web application browser interface\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/browsergetstarted.png)

Enter a phrase you want turned to speech in the input box, then choose **Synthesize**\. When the audio is ready to play, a message appears\. Use the audio player controls to hear the synthesized speech\.

## Possible Enhancements<a name="getting-started-browser-variations"></a>

Here are variations on this application you can use to further explore using the SDK for JavaScript in a browser script\.
+ Experiment with other sound output formats\.
+ Add the option to select any of the various voices provided by Amazon Polly\.
+ Integrate an identity provider like Facebook or Amazon to use with the authenticated IAM role\.