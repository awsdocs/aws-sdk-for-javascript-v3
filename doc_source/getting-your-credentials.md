--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Getting your credentials<a name="getting-your-credentials"></a>

When you create an AWS account, your account is provided with root credential, or an access key, which consists of the following:
+ An access key ID
+ A secret access key

For more information about your access keys, see [Understanding and getting your security credentials](https://docs.aws.amazon.com/general/latest/gr/aws-sec-cred-types.html) in the *AWS General Reference*\.

Access keys consist of an access key ID and secret access key, which are used to sign programmatic requests that you make to AWS\. If you don't have access keys, you can create them from the AWS Management Console\. As a best practice, do not use the AWS account root user access keys for any task where it's not required\. Instead, [create a new administrator IAM user](https://docs.aws.amazon.com/IAM/latest/UserGuide/getting-started_create-admin-group.html) with access keys for yourself\.

The only time that you can view or download the secret access key is when you create the keys\. You cannot recover them later\. However, you can create new access keys at any time\. You must also have permissions to perform the required IAM actions\. For more information, see [Permissions required to access IAM resources](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_permissions-required.html) in the *IAM User Guide*\.

**To create access keys for an IAM user**

1. Sign in to the AWS Management Console and open the IAM console at [https://console\.aws\.amazon\.com/iam/](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane, choose **Users**\.

1. Choose the name of the user whose access keys you want to create, and then choose the **Security credentials** tab\.

1. In the **Access keys** section, choose **Create access key**\.

1. To view the new access key pair, choose **Show**\. You will not have access to the secret access key again after this dialog box closes\. Your credentials will look something like this:
   + Access key ID: AKIAIOSFODNN7EXAMPLE
   + Secret access key: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY

1. To download the key pair, choose **Download \.csv file**\. Store the keys in a secure location\. You will not have access to the secret access key again after this dialog box closes\.

   Keep the keys confidential in order to protect your AWS account and never email them\. Do not share them outside your organization, even if an inquiry appears to come from AWS or Amazon\.com\. No one who legitimately represents Amazon will ever ask you for your secret key\.

1. After you download the `.csv` file, choose **Close**\. When you create an access key, the key pair is active by default, and you can use the pair right away\.

**Related topics**
+ [What is IAM?](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) in the *IAM User Guide*
+ [AWS security credentials](https://docs.aws.amazon.com/general/latest/gr/aws-security-credentials.html) in *AWS General Reference* 