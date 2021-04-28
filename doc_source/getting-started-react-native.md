--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Getting started in React Native<a name="getting-started-react-native"></a>

This tutorial shows you how you can create a React Native app using [React Native CLI](https://reactnative.dev/docs/environment-setup)\. 

![\[JavaScript code example that applies to React Native.\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/browsericon.png)

**This tutorial shows you:**
+ How to install and include the AWS SDK for JavaScript version 3 \(V3\) modules that your project uses\.
+ How to write code that connects to Amazon Simple Storage Service \(Amazon S3\) to create and delete an Amazon S3 bucket\.

## The Scenario<a name="getting-started-react-scenario"></a>

Amazon S3 is a cloud service that enables you to store and retrieve any amount of data at any time, from anywhere on the web\. React Native is a development framework that enables you to create mobile applications\. This tutorial shows you how you can create a React Native app that connects to Amazon S3 to create and delete an Amazon S3 bucket\.

The app uses the following SDK for JavaScript APIs:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CognitoIdentityCredentials.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CognitoIdentityCredentials.html) constructor
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html) constructor

## Setup for this tutorial<a name="getting-started-react-setup"></a>

This section provides the minimal setup needed to complete this tutorial\. You shouldn't consider this to be a full setup\. For that, see [Setting up the SDK for JavaScript](setting-up.md)\.

**Note**  
If you've already completed any of the following steps through other tutorials or existing configuration, skip those steps\.

### Create an AWS account<a name="getting-started-react-setup-account"></a>

To create an AWS account, see [How do I create and activate a new Amazon Web Services account?](https://aws.amazon.com/premiumsupport/knowledge-center/create-and-activate-aws-account)

### Create AWS credentials and a profile<a name="getting-started-react-setup-creds"></a>

To perform these tutorials, you need to create an AWS Identity and Access Management \(IAM\) user and obtain credentials for that user\. After you have those credentials, you make them available to the SDK in your development environment\. Here's how\.

**To create and use credentials**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. Choose **Users**, and then choose **Add user**\.

1. Provide a user name\. For this tutorial, we'll use *React\-Native\-Tutorial\-User*\.

1. Under **Select AWS access type**, select **Programmatic access**, and then choose **Next: Permissions**\.

1. Choose **Attach existing policies directly**\.

1. In **Search**, enter **s3**, and then select **AmazonS3FullAccess**\.

1. Choose **Next: Tags**, **Next: Review**, and **Create user**\.

1. Record the credentials for *React\-Native\-Tutorial\-User*\. You can do so by downloading the `.csv` file or by copying and pasting the *Access key ID* and *Secret access key*\.
**Warning**  
Use appropriate security measures to keep these credentials safe and rotated\.

1. Create or open the shared AWS credentials file\. This file is `~/.aws/credentials` on Linux and macOS systems, and `%USERPROFILE%\.aws\credentials` on Windows\.

1. Add the following text to the shared AWS credentials file, but replace the example ID and example key with the ones you obtained earlier\. Remember to save the file\.

   ```
   [javascript-tutorials]
   aws_access_key_id = AKIAIOSFODNN7EXAMPLE
   aws_secret_access_key = wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
   ```

   

The preceding procedure is the simplest of several possibilities for authentication and authorization\. For complete information, see [Setting credentials](setting-credentials.md)\.

### Install other tools<a name="getting-started-react-setup-tools"></a>

To complete this tutorial, you need to set up your [React Native development environment](https://reactnative.dev/docs/environment-setup)\. 

You also need to install the following tools:
+ [https://nodejs.org/en/download/](https://nodejs.org/en/download/)
+ [Xcode](https://developer.apple.com/xcode/) if you're testing on IOS\.
+ [Android Studio](https://developer.android.com/studio/) if you're testing on Android\.

## Step 1: Create an Amazon Cognito Identity Pool<a name="getting-started-react-create-identity-pool"></a>

In this exercise, you create and use an Amazon Cognito Identity pool to provide unauthenticated access to your app for the Amazon S3 service\. Creating an identity pool also creates two AWS Identity and Access Management \(IAM\) roles, one to support users authenticated by an identity provider and the other to support unauthenticated guest users\.

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

1. On the next page, choose **View Details** to see the names of the two IAM roles created for your identity pool\. Make a note of the name of the role for unauthenticated identities\. You need this name to add the required policy for Amazon S3\.

1. Choose **Allow**\.

1. On the **Sample code** page, select the Platform of *JavaScript*\. Then, copy or write down the identity pool ID and the Region\. You need these values to replace *REGION* and *IDENTITY\_POOL\_ID* in your browser script\.

After you create your Amazon Cognito identity pool, you're ready to add permissions for Amazon S3 that are needed by your React Native app\.

## Step 2: Add a Policy to the Created IAM Role<a name="getting-started-react-iam-role"></a>

To enable browser script access to Amazon S3 to create and delete an Amazon S3 bucket, use the unauthenticated IAM role created for your Amazon Cognito identity pool\. This requires you to add an IAM policy to the role\. For more information about IAM roles, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

**To add an Amazon S3 policy to the IAM role associated with unauthenticated users**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation panel on the left of the page, choose **Roles**\.

1. In the list of IAM roles, click the link for the unauthenticated identities role previously created by Amazon Cognito\.

1. In the **Summary** page for this role, choose **Attach policies**\.

1. In the **Attach Permissions** page for this role, find and then select the check box for **AmazonS3FullAccess**\.
**Note**  
You can use this process to enable access to any Amazon or AWS service\.

1. Choose **Attach policy**\.

After you create your Amazon Cognito identity pool and add permissions for Amazon S3 to your IAM role for unauthenticated users, you are ready to build the app\.

## Step 3: Create app using create\-react\-native\-app<a name="react-prerequisites"></a>

Create a React Native App by running the following command\.

```
npx react-native init ReactNativeApp --npm
```

## Step 4: Install the Amazon S3 package and other dependencies<a name="getting-started-react-install-dependancies"></a>

Inside the directory of the project, run the following commands to install the Amazon S3 package\.

```
npm install @aws-sdk/client-s3
```

This command installs the Amazon S3 package in your project, and updates `package.json` to list Amazon S3 as a project dependency\. You can find information about this package by searching for "@aws\-sdk" on the [https://www.npmjs.com/](https://www.npmjs.com/)npm website\.

These packages and their associated code are installed in the `node_modules` subdirectory of your project\.

For more information about installing Node\.js packages, see [Downloading and installing packages locally](https://docs.npmjs.com/downloading-and-installing-packages-locally) and [Creating Node\.js modules](Downloading and installing packages locally) on the [npm \(Node\.js package manager\) website](https://www.npmjs.com/)\. For information about downloading and installing the AWS SDK for JavaScript, see [Installing the SDK for JavaScript](installing-jssdk.md)\.

Install other dependencies required for authentication\.

```
npm install @aws-sdk/client-cognito-identity @aws-sdk/credential-provider-cognito-identity
```

## Step 5: Write the React Native code<a name="getting-started-react-write-native-code"></a>

Add the following code to the `App.js`\.

```
import React, { useState } from "javascriptv3/example_code/reactnative/App";
import { Button, StyleSheet, Text, TextInput, View } from "react-native";

import {
  S3Client,
  CreateBucketCommand,
  DeleteBucketCommand,
} from "@aws-sdk/client-s3";
import { CognitoIdentityClient } from "@aws-sdk/client-cognito-identity";
import { fromCognitoIdentityPool } from "@aws-sdk/credential-provider-cognito-identity";

const App = () => {
  const [bucketName, setBucketName] = useState("");
  const [successMsg, setSuccessMsg] = useState("");
  const [errorMsg, setErrorMsg] = useState("");

  // Replace REGION with the appropriate AWS Region, such as 'us-east-1'.
  const region = "REGION";
  const client = new S3Client({
    region,
    credentials: fromCognitoIdentityPool({
      client: new CognitoIdentityClient({ region }),
      // Replace IDENTITY_POOL_ID with an appropriate Amazon Cognito Identity Pool ID for, such as 'us-east-1:xxxxxx-xxx-4103-9936-b52exxxxfd6'.
      identityPoolId: "IDENTITY_POOL_ID",
    }),
  });

  const createBucket = async () => {
    setSuccessMsg("");
    setErrorMsg("");

    try {
      await client.send(new CreateBucketCommand({ Bucket: bucketName }));
      setSuccessMsg(`Bucket "${bucketName}" created.`);
    } catch (e) {
      setErrorMsg(e);
    }
  };

  const deleteBucket = async () => {
    setSuccessMsg("");
    setErrorMsg("");

    try {
      await client.send(new DeleteBucketCommand({ Bucket: bucketName }));
      setSuccessMsg(`Bucket "${bucketName}" deleted.`);
    } catch (e) {
      setErrorMsg(e);
    }
  };

  return (
    <View style={styles.container}>
      <Text style={{ color: "green" }}>
	{successMsg ? `Success: ${successMsg}` : ``}
      </Text>
      <Text style={{ color: "red" }}>
	{errorMsg ? `Error: ${errorMsg}` : ``}
      </Text>
      <View>
	<TextInput
	  style={styles.textInput}
	  onChangeText={(text) => setBucketName(text)}
	  autoCapitalize={"none"}
	  value={bucketName}
	  placeholder={"Enter Bucket Name"}
	/>
	<Button
	  backroundColor="#68a0cf"
	  title="Create Bucket"
	  onPress={createBucket}
	/>
	<Button
	  backroundColor="#68a0cf"
	  title="Delete Bucket"
	  onPress={deleteBucket}
	/>
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    alignItems: "center",
    justifyContent: "center",
  },
});

export default App;
```

The code first imports required React, React Native, and AWS SDK dependencies\.

Inside the function App:
+ The S3Client object is created, specifying the credentials using Cognito Identity Pool created earlier\.
+ The methods `createBucket` and `deleteBucket` create and delete the specified bucket, respectively\.
+ The React Native View displays a text input field for the user to specify an Amazon S3 bucket name, and buttons to create and delete the specified Amazon S3 bucket\.

The full JavaScript page is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/reactnative/App.js)\.

## Step 6: Run the Example<a name="getting-started-react-native-run-sample"></a>

To run the example, either run web, ios or android command using npm\.

Here is an example output of running `ios` command on macOS\.

```
$ npm run ios

> ReactNativeApp@0.0.1 ios /Users/trivikr/workspace/ReactNativeApp
> react-native run-ios

info Found Xcode workspace "ReactNativeApp.xcworkspace"
info Launching iPhone 11 (iOS 14.2)
info Building (using "xcodebuild -workspace ReactNativeApp.xcworkspace -configuration Debug -scheme ReactNativeApp -destination id=706C1A97-FA38-407D-AD77-CB4FCA9134E9")
success Successfully built the app
info Installing "/Users/trivikr/Library/Developer/Xcode/DerivedData/ReactNativeApp-cfhmsyhptwflqqejyspdqgjestra/Build/Products/Debug-iphonesimulator/ReactNativeApp.app"
info Launching "org.reactjs.native.example.ReactNativeApp"

success Successfully launched the app on the simulator
```

Here is an example output of running `android` command on macOS\.

```
$ npm run android

> ReactNativeApp@0.0.1 android
> react-native run-android

info Running jetifier to migrate libraries to AndroidX. You can disable it using "--no-jetifier" flag.
Jetifier found 970 file(s) to forward-jetify. Using 12 workers...
info Starting JS server...
info Launching emulator...
info Successfully launched emulator.
info Installing the app...

> Task :app:stripDebugDebugSymbols UP-TO-DATE
Compatible side by side NDK version was not found.

> Task :app:installDebug
02:18:38 V/ddms: execute: running am get-config
02:18:38 V/ddms: execute 'am get-config' on 'emulator-5554' : EOF hit. Read: -1
02:18:38 V/ddms: execute: returning
Installing APK 'app-debug.apk' on 'Pixel_3a_API_30_x86(AVD) - 11' for app:debug
02:18:38 D/app-debug.apk: Uploading app-debug.apk onto device 'emulator-5554'
02:18:38 D/Device: Uploading file onto device 'emulator-5554'
02:18:38 D/ddms: Reading file permision of /Users/trivikr/workspace/ReactNativeApp/android/app/build/outputs/apk/debug/app-debug.apk as: rw-r--r--
02:18:40 V/ddms: execute: running pm install -r -t "/data/local/tmp/app-debug.apk"
02:18:41 V/ddms: execute 'pm install -r -t "/data/local/tmp/app-debug.apk"' on 'emulator-5554' : EOF hit. Read: -1
02:18:41 V/ddms: execute: returning
02:18:41 V/ddms: execute: running rm "/data/local/tmp/app-debug.apk"
02:18:41 V/ddms: execute 'rm "/data/local/tmp/app-debug.apk"' on 'emulator-5554' : EOF hit. Read: -1
02:18:41 V/ddms: execute: returning
Installed on 1 device.

Deprecated Gradle features were used in this build, making it incompatible with Gradle 7.0.
Use '--warning-mode all' to show the individual deprecation warnings.
See https://docs.gradle.org/6.2/userguide/command_line_interface.html#sec:command_line_warnings

BUILD SUCCESSFUL in 6s
27 actionable tasks: 2 executed, 25 up-to-date
info Connecting to the development server...
8081
info Starting the app on "emulator-5554"...
Starting: Intent { cmp=com.reactnativeapp/.MainActivity }
```

Enter the bucket name you want to create or delete and click on either **Create Bucket** or **Delete Bucket**\. The respective command will be sent to Amazon S3, and success or error message will be displayed\.

![\[Image NOT FOUND\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/react-app-running.png)

## Possible Enhancements<a name="getting-started-react-native-variations"></a>

Here are variations on this application you can use to further explore using the SDK for JavaScript in a React Native app\.
+ Add a button to list Amazon S3 buckets, and provide a delete button next to each bucket listed\.
+ Add a button to put text object into a bucket\.
+ Integrate an external identity provider like Facebook or Amazon to use with the authenticated IAM role\.
