--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# AWS STS examples using SDK for JavaScript V3<a name="javascript_sts_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for JavaScript V3 with AWS STS\.

*Actions* are code excerpts that show you how to call individual AWS STS functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple AWS STS functions\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#w362aac23b9c33c13)

## Actions<a name="w362aac23b9c33c13"></a>

### Assume a role<a name="sts_AssumeRole_javascript_topic"></a>

The following code example shows how to assume a role with AWS STS\.

**SDK for JavaScript V3**  
Create the client\.  

```
import { STSClient } from  "@aws-sdk/client-sts";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create an Amazon STS service client object.
const stsClient = new STSClient({ region: REGION });
export { stsClient };
```
Assume the IAM role\.  

```
// Import required AWS SDK clients and commands for Node.js
import { stsClient } from "./libs/stsClient.js";
import {
  AssumeRoleCommand,
  GetCallerIdentityCommand,
} from "@aws-sdk/client-sts";

// Set the parameters
export const params = {
  RoleArn: "ARN_OF_ROLE_TO_ASSUME", //ARN_OF_ROLE_TO_ASSUME
  RoleSessionName: "session1",
  DurationSeconds: 900,
};

export const run = async () => {
  try {
    //Assume Role
    const data = await stsClient.send(new AssumeRoleCommand(params));
    return data;
    const rolecreds = {
      accessKeyId: data.Credentials.AccessKeyId,
      secretAccessKey: data.Credentials.SecretAccessKey,
      sessionToken: data.Credentials.SessionToken,
    };
    //Get Amazon Resource Name (ARN) of current identity
    try {
      const stsParams = { credentials: rolecreds };
      const stsClient = new STSClient(stsParams);
      const results = await stsClient.send(
        new GetCallerIdentityCommand(rolecreds)
      );
      console.log("Success", results);
    } catch (err) {
      console.log(err, err.stack);
    }
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/sts#code-examples)\. 
+  For API details, see [AssumeRole](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sts/classes/assumerolecommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript V2**  
  

```
// Load the AWS SDK for Node.js
const AWS = require('aws-sdk');
// Set the region 
AWS.config.update({region: 'REGION'});

var roleToAssume = {RoleArn: 'arn:aws:iam::123456789012:role/RoleName',
                    RoleSessionName: 'session1',
                    DurationSeconds: 900,};
var roleCreds;

// Create the STS service object    
var sts = new AWS.STS({apiVersion: '2011-06-15'});

//Assume Role
sts.assumeRole(roleToAssume, function(err, data) {
    if (err) console.log(err, err.stack);
    else{
        roleCreds = {accessKeyId: data.Credentials.AccessKeyId,
                     secretAccessKey: data.Credentials.SecretAccessKey,
                     sessionToken: data.Credentials.SessionToken};
        stsGetCallerIdentity(roleCreds);
    }
});

//Get Arn of current identity
function stsGetCallerIdentity(creds) {
    var stsParams = {credentials: creds };
    // Create STS service object
    var sts = new AWS.STS(stsParams);
        
    sts.getCallerIdentity({}, function(err, data) {
        if (err) {
            console.log(err, err.stack);
        }
        else {
            console.log(data.Arn);
        }
    });    
}
```
+  Find instructions and more code on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/sts#code-examples)\. 
+  For API details, see [AssumeRole](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sts-2011-06-15/AssumeRole) in *AWS SDK for JavaScript API Reference*\. 