# Prepare the Browser Script<a name="using-lambda-browser-script"></a>

This topic is part of a larger tutorial about using the AWS SDK for JavaScript with AWS Lambda functions\. To start at the beginning of the tutorial, see [Tutorial: Creating and Using Lambda Functions](using-lambda-functions.md)\.

In this task, you will focus on creating an Amazon Cognito identity pool used to authenticate your browser script code, and then editing the browser script accordingly\.

![\[Preparing the browser JavaScript\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/browser-script.png)![\[Preparing the browser JavaScript\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Preparing the browser JavaScript\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

## Prepare an Amazon Cognito Identity Pool<a name="identity-pool"></a>

The JavaScript code in the browser script needs authentication to access AWS services\. Within webpages, you typically use Amazon Cognito Identity to do this authentication\. First, create an Amazon Cognito identity pool\.

**To create and prepare an Amazon Cognito identity pool for the browser script**

1. Open the [Amazon Cognito console](https://console.aws.amazon.com/cognito/), choose **Manage Federated Identities**, and then choose **Create new identity pool**\.

1. Enter a name for your identity pool, choose **enable access to unauthenticated identities**, and then choose **Create Pool**\.

1. Choose **View Details** to display details on both the authenticated and unauthenticated IAM roles created for this identity pool\.

1. In the summary for the unauthenticated role, choose **View Policy Document** to display the current role policy\.

1. Choose **Edit** to change the role policy, and then choose **Ok**\.

1. In the text box, edit the policy to insert this `"lambda:InvokeFunction"` action, so the full policy becomes the following\.

   ```
   {
      "Version": "2012-10-17",
      "Statement": [
      {
         "Effect": "Allow",
         "Action": [
            "lambda:InvokeFunction",
            "mobileanalytics:PutEvents",
            "cognito-sync:*"
         ],
         "Resource": [
            "*"
         ]
       }
     ]
   }
   ```

1. Choose **Allow**\.

1. Choose **Sample code** in the side menu\. Make a note of the identity pool ID, shown in red text in the console\.  
![\[Preparing an Amazon Cognito identity pool for the browser script\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/identity-pool-id.png)![\[Preparing an Amazon Cognito identity pool for the browser script\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Preparing an Amazon Cognito identity pool for the browser script\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

## Edit the Browser Script<a name="edit-script"></a>

Next, update the browser script to include the Amazon Cognito identity pool ID created for this application\.

**To prepare the browser script in the webpage**

1. Open `index.html` in the `MyLambdaApp` folder in a text editor\.

1. Find this line of code in the browser script\.

   `AWS.config.credentials = new AWS.CognitoIdentityCredentials({IdentityPoolId: 'IDENTITY_POOL_ID'});`

1. Replace `IDENTITY_POOL_ID` with the identity pool ID you obtained previously\.

1. Save `index.html`\.

Click **next** to continue the tutorial\.