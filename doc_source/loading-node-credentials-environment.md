# Loading Credentials in Node\.js from Environment Variables<a name="loading-node-credentials-environment"></a>

The SDK automatically detects AWS credentials set as variables in your environment and uses them for SDK requests, eliminating the need to manage credentials in your application\. The environment variables that you set to provide your credentials are:
+ `AWS_ACCESS_KEY_ID`
+ `AWS_SECRET_ACCESS_KEY`
+ `AWS_SESSION_TOKEN` \(optional\)

**Note**  
When setting environment variables, be sure to take appropriate actions afterwards \(according to the needs of your operating system\) to make the variables available in the shell or command environment\.