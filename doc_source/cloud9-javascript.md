# Using AWS Cloud9 with the AWS SDK for JavaScript<a name="cloud9-javascript"></a>

You can use AWS Cloud9 with the AWS SDK for JavaScript to write and run your JavaScript in the browser code —as well as write, run, and debug your Node\.js code—using just a browser\. AWS Cloud9 includes tools such as a code editor and terminal, plus a debugger for Node\.js code\. Because the AWS Cloud9 IDE is cloud based, you can work on your projects from your office, home, or anywhere using an internet\-connected machine\. For general information about AWS Cloud9, see the [AWS Cloud9 User Guide](https://docs.aws.amazon.com/cloud9/latest/user-guide/)\.

Follow these steps to set up AWS Cloud9 with the SDK for JavaScript:

**Contents**
+ [Step 1: Set up Your AWS Account to Use AWS Cloud9](#cloud9-javascript-account)
+ [Step 2: Set up Your AWS Cloud9 Development Environment](#cloud9-javascript-environment)
+ [Step 3: Set up the SDK for JavaScript](#cloud9-javascript-sdk)
  + [To set up the SDK for JavaScript for Node\.js](#cloud9-javascript-sdk-nodejs)
  + [To set up the SDK for JavaScript in the browser](#cloud9-javascript-sdk-browser)
+ [Step 4: Download Example Code](#cloud9-javascript-examples)
+ [Step 5: Run and Debug Example Code](#cloud9-javascript-run)

## Step 1: Set up Your AWS Account to Use AWS Cloud9<a name="cloud9-javascript-account"></a>

Start to use AWS Cloud9 by signing in to the AWS Cloud9 console as an AWS Identity and Access Management \(IAM\) entity \(for example, an IAM user\) who has access permissions for AWS Cloud9 in your AWS account\.

To set up an IAM entity in your AWS account to access AWS Cloud9, and to sign in to the AWS Cloud9 console, see [Team Setup for AWS Cloud9](https://docs.aws.amazon.com/cloud9/latest/user-guide/setup.html) in the *AWS Cloud9 User Guide*\.

## Step 2: Set up Your AWS Cloud9 Development Environment<a name="cloud9-javascript-environment"></a>

After you sign in to the AWS Cloud9 console, use the console to create an AWS Cloud9 development environment\. After you create the environment, AWS Cloud9 opens the IDE for that environment\.

See [Creating an Environment in AWS Cloud9](https://docs.aws.amazon.com/cloud9/latest/user-guide/create-environment.html) in the *AWS Cloud9 User Guide* for details\.

**Note**  
As you create your environment in the console for the first time, we recommend that you choose the option to **Create a new instance for environment \(EC2\)**\. This option tells AWS Cloud9 to create an environment, launch an Amazon EC2 instance, and then connect the new instance to the new environment\. This is the fastest way to begin using AWS Cloud9\.

## Step 3: Set up the SDK for JavaScript<a name="cloud9-javascript-sdk"></a>

After AWS Cloud9 opens the IDE for your development environment, follow one or both of the following procedures to use the IDE to set up the SDK for JavaScript in your environment\.

### To set up the SDK for JavaScript for Node\.js<a name="cloud9-javascript-sdk-nodejs"></a>

1. If the terminal isn't already open in the IDE, open it\. To do this, on the menu bar in the IDE, choose **Window, New Terminal**\.

1. Run the following command to use npm to install the SDK for JavaScript\.

   ```
   npm install aws-sdk
   ```

   If the IDE can't find npm, run the following commands, one at a time in the following order, to install npm\. \(These commands assume you chose the option to **Create a new instance for environment \(EC2\)**, earlier in this topic\.\)
**Warning**  
AWS does not control the following code\. Before you run it, be sure to verify its authenticity and integrity\. More information about this code can be found in the [nvm](https://github.com/nvm-sh/nvm/blob/master/README.md) GitHub repository\.

   ```
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash # Download and install Node Version Manager (nvm).
   . ~/.bashrc                                                                     # Activate nvm.
   nvm install node                                                                # Use nvm to install npm (and Node.js at the same time).
   ```

### To set up the SDK for JavaScript in the browser<a name="cloud9-javascript-sdk-browser"></a>

You don't have to install the SDK for JavaScript to use it in browser scripts\. You can load the hosted SDK for JavaScript package directly from AWS with a script in your HTML pages\.

## Step 4: Download Example Code<a name="cloud9-javascript-examples"></a>

Use the terminal you opened in the previous step to download example code for the SDK for JavaScript into the AWS Cloud9 development environment\. \(If the terminal isn't already open in the IDE, open it by choosing **Window, New Terminal** on the menu bar in the IDE\.\)

To download the example code, run the following command\. This command downloads a copy of all of the code examples used in the official AWS SDK documentation into your environment's root directory\.

```
git clone https://github.com/awsdocs/aws-doc-sdk-examples.git
```

To find code examples for the SDK for JavaScript, use the **Environment** window to open the `ENVIRONMENT_NAME\aws-doc-sdk-examples\javascript\example_code`, where *ENVIRONMENT\_NAME* is the name of your AWS Cloud9 development environment\.

To learn how to work with these and other code examples, see [SDK for JavaScript Code Examples](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/sdk-code-samples.html)\.

## Step 5: Run and Debug Example Code<a name="cloud9-javascript-run"></a>

To run code in your AWS Cloud9 development environment, see [Run Your Code](https://docs.aws.amazon.com/cloud9/latest/user-guide/build-run-debug.html#build-run-debug-run) in the *AWS Cloud9 User Guide*\.

To debug Node\.js code, see [Debug Your Code](https://docs.aws.amazon.com/cloud9/latest/user-guide/build-run-debug.html#build-run-debug-debug) in the *AWS Cloud9 User Guide*\.