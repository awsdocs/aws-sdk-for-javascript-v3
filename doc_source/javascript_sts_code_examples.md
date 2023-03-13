--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# AWS STS examples using SDK for JavaScript \(v3\)<a name="javascript_sts_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for JavaScript \(v3\) with AWS STS\.

*Actions* are code excerpts that show you how to call individual service functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple functions within the same service\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#actions)

## Actions<a name="actions"></a>

### Assume a role<a name="sts_AssumeRole_javascript_topic"></a>

The following code example shows how to assume a role with AWS STS\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/sts#code-examples)\. 
Create the client\.  

```
import { STSClient } from "@aws-sdk/client-sts";
// Set the AWS Region.
const REGION = "us-east-1";
// Create an AWS STS service client object.
export const client = new STSClient({ region: REGION });
```
Assume the IAM role\.  

```
import { AssumeRoleCommand } from "@aws-sdk/client-sts";

import { client } from "../libs/client.js";

export const main = async () => {
  try {
    // Returns a set of temporary security credentials that you can use to
    // access Amazon Web Services resources that you might not normally 
    // have access to.
    const command = new AssumeRoleCommand({
      // The Amazon Resource Name (ARN) of the role to assume.
      RoleArn: "ROLE_ARN",
      // An identifier for the assumed role session.
      RoleSessionName: "session1",
      // The duration, in seconds, of the role session. The value specified
      // can range from 900 seconds (15 minutes) up to the maximum session 
      // duration set for the role.
      DurationSeconds: 900,
    });
    const response = await client.send(command);
    console.log(response);
  } catch (err) {
    console.error(err);
  }
};
```
+  For API details, see [AssumeRole](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-sts/classes/assumerolecommand.html) in *AWS SDK for JavaScript API Reference*\. 

**SDK for JavaScript \(v2\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascript/example_code/sts#code-examples)\. 
  

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
+  For API details, see [AssumeRole](https://docs.aws.amazon.com/goto/AWSJavaScriptSDK/sts-2011-06-15/AssumeRole) in *AWS SDK for JavaScript API Reference*\. 