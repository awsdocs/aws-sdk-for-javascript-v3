--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Loading credentials in Node\.js from the shared credentials file<a name="loading-node-credentials-shared"></a>

You can keep your AWS credentials data in a shared file used by SDKs and the command line interface\. When the SDK for JavaScript loads, it automatically searches the shared credentials file, which is named "credentials"\. Where you keep the shared credentials file depends on your operating system:
+ The shared credentials file on Linux, Unix, and macOS: `~/.aws/credentials`
+ The shared credentials file on Windows: `C:\Users\USER_NAME\.aws\credentials`

If you do not already have a shared credentials file, see [Getting your credentials](getting-your-credentials.md)\. Once you follow those instructions, you should see text similar to the following in the credentials file, where *<YOUR\_ACCESS\_KEY\_ID>* is your access key ID and *<YOUR\_SECRET\_ACCESS\_KEY>* is your secret access key\. Create a shared credentials file like below in the directory\.

```
[default]
aws_access_key_id = <YOUR_ACCESS_KEY_ID>
aws_secret_access_key = <YOUR_SECRET_ACCESS_KEY>
```

The `[default]` section heading specifies a default profile and associated values for credentials\. You can create additional profiles in the same shared configuration file, each with its own credential information\. The following example shows a configuration file with the default profile and two additional profiles:

```
[default] ; default profile
aws_access_key_id = <DEFAULT_ACCESS_KEY_ID>
aws_secret_access_key = <DEFAULT_SECRET_ACCESS_KEY>
    
[personal-account] ; personal account profile
aws_access_key_id = <PERSONAL_ACCESS_KEY_ID>
aws_secret_access_key = <PERSONAL_SECRET_ACCESS_KEY>
    
[work-account] ; work account profile
aws_access_key_id = <WORK_ACCESS_KEY_ID>
aws_secret_access_key = <WORK_SECRET_ACCESS_KEY>
```

By default, the SDK checks the `AWS_PROFILE` environment variable to determine which profile to use\. If the `AWS_PROFILE` variable is not set in your environment, the SDK uses the credentials for the `[default]` profile\. To use one of the alternate profiles, set or change the value of the `AWS_PROFILE` environment variable\. For example, given the configuration file shown, to use the credentials from the work account, set the `AWS_PROFILE` environment variable to `work-account` \(as appropriate for your operating system\)\.

**Note**  
When setting environment variables, be sure to take appropriate actions afterward \(according to the needs of your operating system\) to make the variables available in the shell or command environment\.

After setting the environment variable \(if needed\), you can run a JavaScript file that uses the SDK, such as for example, a file named `script.js`\.

```
$ node script.js
```

You can also explicitly select the profile used by a client, as shown in the following example\.

```
const {fromIni} = require("@aws-sdk/credential-providers");
const s3Client = new S3.S3Client({
  credentials: fromIni({profile: 'work-account'})
});
```
