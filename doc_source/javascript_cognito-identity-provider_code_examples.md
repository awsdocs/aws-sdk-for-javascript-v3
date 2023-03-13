--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Amazon Cognito Identity Provider examples using SDK for JavaScript \(v3\)<a name="javascript_cognito-identity-provider_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for JavaScript \(v3\) with Amazon Cognito Identity Provider\.

*Actions* are code excerpts that show you how to call individual service functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple functions within the same service\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#actions)
+ [Scenarios](#scenarios)

## Actions<a name="actions"></a>

### Confirm a user<a name="cognito-identity-provider_ConfirmSignUp_javascript_topic"></a>

The following code example shows how to confirm an Amazon Cognito user\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cognito#code-examples)\. 
  

```
const confirmSignUp = async ({ clientId, username, code }) => {
  const client = createClientForDefaultRegion(CognitoIdentityProviderClient);

  const command = new ConfirmSignUpCommand({
    ClientId: clientId,
    Username: username,
    ConfirmationCode: code,
  });

  return client.send(command);
};
```
+  For API details, see [ConfirmSignUp](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/confirmsignupcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Confirm an MFA device for tracking<a name="cognito-identity-provider_ConfirmDevice_javascript_topic"></a>

The following code example shows how to confirm an MFA device for tracking by Amazon Cognito\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cognito#code-examples)\. 
  

```
const confirmDevice = async ({ deviceKey, accessToken, passwordVerifier, salt }) => {
  const client = createClientForDefaultRegion(CognitoIdentityProviderClient);

  const command = new ConfirmDeviceCommand({
    DeviceKey: deviceKey,
    AccessToken: accessToken,
    DeviceSecretVerifierConfig: {
      PasswordVerifier: passwordVerifier,
      Salt: salt
    },
  });

  return client.send(command);
};
```
+  For API details, see [ConfirmDevice](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/confirmdevicecommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Get a token to associate an MFA application with a user<a name="cognito-identity-provider_AssociateSoftwareToken_javascript_topic"></a>

The following code example shows how to get a token to associate an MFA application with an Amazon Cognito user\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cognito#code-examples)\. 
  

```
const associateSoftwareToken = async (session) => {
  const client = createClientForDefaultRegion(CognitoIdentityProviderClient);
  const command = new AssociateSoftwareTokenCommand({
    Session: session,
  });

  return client.send(command);
};
```
+  For API details, see [AssociateSoftwareToken](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/associatesoftwaretokencommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Get information about a user<a name="cognito-identity-provider_AdminGetUser_javascript_topic"></a>

The following code example shows how to get information about an Amazon Cognito user\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cognito/#code-examples)\. 
  

```
const adminGetUser = async ({ userPoolId, username }) => {
  const client = createClientForDefaultRegion(CognitoIdentityProviderClient);

  const command = new AdminGetUserCommand({
    UserPoolId: userPoolId,
    Username: username,
  });

  return client.send(command);
};
```
+  For API details, see [AdminGetUser](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/admingetusercommand.html) in *AWS SDK for JavaScript API Reference*\. 

### List users<a name="cognito-identity-provider_ListUsers_javascript_topic"></a>

The following code example shows how to list Amazon Cognito users\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cognito#code-examples)\. 
  

```
const listUsers = async ({ userPoolId }) => {
  const client = createClientForDefaultRegion(CognitoIdentityProviderClient);

  const command = new ListUsersCommand({
    UserPoolId: userPoolId,
  });

  return client.send(command);
};
```
+  For API details, see [ListUsers](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/listuserscommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Resend a confirmation code<a name="cognito-identity-provider_ResendConfirmationCode_javascript_topic"></a>

The following code example shows how to resend an Amazon Cognito confirmation code\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cognito#code-examples)\. 
  

```
const resendConfirmationCode = async ({ clientId, username }) => {
  const client = createClientForDefaultRegion(CognitoIdentityProviderClient);

  const command = new ResendConfirmationCodeCommand({
    ClientId: clientId,
    Username: username
  });

  return client.send(command);
};
```
+  For API details, see [ResendConfirmationCode](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/resendconfirmationcodecommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Respond to SRP authentication challenges<a name="cognito-identity-provider_RespondToAuthChallenge_javascript_topic"></a>

The following code example shows how to respond to Amazon Cognito SRP authentication challenges\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cognito#code-examples)\. 
  

```
const respondToAuthChallenge = async ({
  clientId,
  username,
  session,
  userPoolId,
  code,
}) => {
  const client = createClientForDefaultRegion(CognitoIdentityProviderClient);

  const command = new RespondToAuthChallengeCommand({
    ChallengeName: ChallengeNameType.SOFTWARE_TOKEN_MFA,
    ChallengeResponses: {
      SOFTWARE_TOKEN_MFA_CODE: code,
      USERNAME: username,
    },
    ClientId: clientId,
    UserPoolId: userPoolId,
    Session: session,
  });

  return client.send(command);
};
```
+  For API details, see [RespondToAuthChallenge](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/respondtoauthchallengecommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Respond to an authentication challenge<a name="cognito-identity-provider_AdminRespondToAuthChallenge_javascript_topic"></a>

The following code example shows how to respond to an Amazon Cognito authentication challenge\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cognito#code-examples)\. 
  

```
const adminRespondToAuthChallenge = async ({
  userPoolId,
  clientId,
  username,
  totp,
  session,
}) => {
  const client = createClientForDefaultRegion(CognitoIdentityProviderClient);
  const command = new AdminRespondToAuthChallengeCommand({
    ChallengeName: ChallengeNameType.SOFTWARE_TOKEN_MFA,
    ChallengeResponses: {
      SOFTWARE_TOKEN_MFA_CODE: totp,
      USERNAME: username,
    },
    ClientId: clientId,
    UserPoolId: userPoolId,
    Session: session,
  });

  return client.send(command);
};
```
+  For API details, see [AdminRespondToAuthChallenge](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/adminrespondtoauthchallengecommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Sign up a user<a name="cognito-identity-provider_SignUp_javascript_topic"></a>

The following code example shows how to sign up a user with Amazon Cognito\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cognito#code-examples)\. 
  

```
const signUp = async ({ clientId, username, password, email }) => {
  const client = createClientForDefaultRegion(CognitoIdentityProviderClient);

  const command = new SignUpCommand({
    ClientId: clientId,
    Username: username,
    Password: password,
    UserAttributes: [{ Name: "email", Value: email }],
  });

  return client.send(command);
};
```
+  For API details, see [SignUp](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/signupcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Start authentication with a tracked device<a name="cognito-identity-provider_InitiateAuth_javascript_topic"></a>

The following code example shows how to start authentication with a device tracked by Amazon Cognito\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cognito#code-examples)\. 
  

```
const initiateAuth = async ({ username, password, clientId }) => {
  const client = createClientForDefaultRegion(CognitoIdentityProviderClient);

  const command = new InitiateAuthCommand({
    AuthFlow: AuthFlowType.USER_PASSWORD_AUTH,
    AuthParameters: {
      USERNAME: username,
      PASSWORD: password,
    },
    ClientId: clientId,
  });

  return client.send(command);
};
```
+  For API details, see [InitiateAuth](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/initiateauthcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Start authentication with administrator credentials<a name="cognito-identity-provider_AdminInitiateAuth_javascript_topic"></a>

The following code example shows how to start authentication with Amazon Cognito and administrator credentials\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cognito/#code-examples)\. 
  

```
const adminInitiateAuth = async ({
  clientId,
  userPoolId,
  username,
  password,
}) => {
  const client = createClientForDefaultRegion(CognitoIdentityProviderClient);

  const command = new AdminInitiateAuthCommand({
    ClientId: clientId,
    UserPoolId: userPoolId,
    AuthFlow: AuthFlowType.ADMIN_USER_PASSWORD_AUTH,
    AuthParameters: { USERNAME: username, PASSWORD: password },
  });

  return client.send(command);
};
```
+  For API details, see [AdminInitiateAuth](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/admininitiateauthcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Verify an MFA application with a user<a name="cognito-identity-provider_VerifySoftwareToken_javascript_topic"></a>

The following code example shows how to verify an MFA application with an Amazon Cognito user\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cognito#code-examples)\. 
  

```
const verifySoftwareToken = async (totp) => {
  const client = createClientForDefaultRegion(CognitoIdentityProviderClient);
  // The 'Session' is provided in the response to 'AssociateSoftwareToken'.
  const session = process.env.SESSION;

  if (!session) {
    throw new Error(
      "Missing a valid Session. Did you run 'admin-initiate-auth'?"
    );
  }

  const command = new VerifySoftwareTokenCommand({
    Session: session,
    UserCode: totp,
  });

  return client.send(command);
};
```
+  For API details, see [VerifySoftwareToken](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/verifysoftwaretokencommand.html) in *AWS SDK for JavaScript API Reference*\. 

## Scenarios<a name="scenarios"></a>

### Sign up a user with a user pool that requires MFA<a name="cognito-identity-provider_Scenario_SignUpUserWithMfa_javascript_topic"></a>

The following code example shows how to:
+ Sign up a user with a user name, password, and email address\.
+ Confirm the user from a code sent in email\.
+ Set up multi\-factor authentication by associating an MFA application with the user\.
+ Sign in by using a password and an MFA code\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/cognito/scenarios/basic#code-examples)\. 
For the best experience, clone the GitHub repository and run this example\. The following code represents a sample of the full example application\.  

```
import { log } from "../../../../libs/utils/util-log.js";
import { signUp } from "../../../actions/sign-up.js";
import { FILE_USER_POOLS } from "./constants.js";
import { getSecondValuesFromEntries } from "../../../../libs/utils/util-csv.js";

const validateClient = (clientId) => {
  if (!clientId) {
    throw new Error(
      `App client id is missing. Did you run 'create-user-pool'?`
    );
  }
};

const validateUser = (username, password, email) => {
  if (!(username && password && email)) {
    throw new Error(
      `Username, password, and email must be provided as arguments to the 'sign-up' command.`
    );
  }
};

const signUpHandler = async (commands) => {
  const [_, username, password, email] = commands;

  try {
    validateUser(username, password, email);
    const clientId = getSecondValuesFromEntries(FILE_USER_POOLS)[0];
    validateClient(clientId);
    log(`Signing up.`);
    await signUp({ clientId, username, password, email });
    log(`Signed up. An confirmation email has been sent to: ${email}.`);
    log(`Run 'confirm-sign-up ${username} <code>' to confirm your account.`);
  } catch (err) {
    log(err);
  }
};

export { signUpHandler };

const signUp = async ({ clientId, username, password, email }) => {
  const client = createClientForDefaultRegion(CognitoIdentityProviderClient);

  const command = new SignUpCommand({
    ClientId: clientId,
    Username: username,
    Password: password,
    UserAttributes: [{ Name: "email", Value: email }],
  });

  return client.send(command);
};

import { log } from "../../../../libs/utils/util-log.js";
import { confirmSignUp } from "../../../actions/confirm-sign-up.js";
import { FILE_USER_POOLS } from "./constants.js";
import { getSecondValuesFromEntries } from "../../../../libs/utils/util-csv.js";

const validateClient = (clientId) => {
  if (!clientId) {
    throw new Error(
      `App client id is missing. Did you run 'create-user-pool'?`
    );
  }
};

const validateUser = (username) => {
  if (!username) {
    throw new Error(
      `Username name is missing. It must be provided as an argument to the 'confirm-sign-up' command.`
    );
  }
};

const validateCode = (code) => {
  if (!code) {
    throw new Error(
      `Verification code is missing. It must be provided as an argument to the 'confirm-sign-up' command.`
    );
  }
};

const confirmSignUpHandler = async (commands) => {
  const [_, username, code] = commands;

  try {
    validateUser(username);
    validateCode(code);
    const clientId = getSecondValuesFromEntries(FILE_USER_POOLS)[0];
    validateClient(clientId);
    log(`Confirming user.`);
    await confirmSignUp({ clientId, username, code });
    log(
      `User confirmed. Run 'admin-initiate-auth ${username} <password>' to sign in.`
    );
  } catch (err) {
    log(err);
  }
};

export { confirmSignUpHandler };

const confirmSignUp = async ({ clientId, username, code }) => {
  const client = createClientForDefaultRegion(CognitoIdentityProviderClient);

  const command = new ConfirmSignUpCommand({
    ClientId: clientId,
    Username: username,
    ConfirmationCode: code,
  });

  return client.send(command);
};

import qrcode from "qrcode-terminal";
import { log } from "../../../../libs/utils/util-log.js";
import { adminInitiateAuth } from "../../../actions/admin-initiate-auth.js";
import { associateSoftwareToken } from "../../../actions/associate-software-token.js";
import { FILE_USER_POOLS } from "./constants.js";
import { getFirstEntry } from "../../../../libs/utils/util-csv.js";

const handleMfaSetup = async (session, username) => {
  const { SecretCode, Session } = await associateSoftwareToken(session);

  // Store the Session for use with 'VerifySoftwareToken'.
  process.env.SESSION = Session;

  console.log(
    "Scan this code in your preferred authenticator app, then run 'verify-software-token' to finish the setup."
  );
  qrcode.generate(
    `otpauth://totp/${username}?secret=${SecretCode}`,
    { small: true },
    console.log
  );
};

const handleSoftwareTokenMfa = (session) => {
  // Store the Session for use with 'AdminRespondToAuthChallenge'.
  process.env.SESSION = session;
};

const validateClient = (id) => {
  if (!id) {
    throw new Error(
      `User pool client id is missing. Did you run 'create-user-pool'?`
    );
  }
};

const validateId = (id) => {
  if (!id) {
    throw new Error(`User pool id is missing. Did you run 'create-user-pool'?`);
  }
};

const validateUser = (username, password) => {
  if (!(username && password)) {
    throw new Error(
      `Username and password must be provided as arguments to the 'admin-initiate-auth' command.`
    );
  }
};

const adminInitiateAuthHandler = async (commands) => {
  const [_, username, password] = commands;

  try {
    validateUser(username, password);

    const [userPoolId, clientId] = getFirstEntry(FILE_USER_POOLS);
    validateId(userPoolId);
    validateClient(clientId);

    log("Signing in.");
    const { ChallengeName, Session } = await adminInitiateAuth({
      clientId,
      userPoolId,
      username,
      password,
    });

    if (ChallengeName === "MFA_SETUP") {
      log("MFA setup is required.");
      return handleMfaSetup(Session, username);
    }

    if (ChallengeName === "SOFTWARE_TOKEN_MFA") {
      handleSoftwareTokenMfa(Session);
      log(`Run 'admin-respond-to-auth-challenge ${username} <totp>'`);
    }
  } catch (err) {
    log(err);
  }
};

export { adminInitiateAuthHandler };

const adminInitiateAuth = async ({
  clientId,
  userPoolId,
  username,
  password,
}) => {
  const client = createClientForDefaultRegion(CognitoIdentityProviderClient);

  const command = new AdminInitiateAuthCommand({
    ClientId: clientId,
    UserPoolId: userPoolId,
    AuthFlow: AuthFlowType.ADMIN_USER_PASSWORD_AUTH,
    AuthParameters: { USERNAME: username, PASSWORD: password },
  });

  return client.send(command);
};

import { log } from "../../../../libs/utils/util-log.js";
import { adminRespondToAuthChallenge } from "../../../actions/admin-respond-to-auth-challenge.js";
import { getFirstEntry } from "../../../../libs/utils/util-csv.js";
import { FILE_USER_POOLS } from "./constants.js";

const verifyUsername = (username) => {
  if (!username) {
    throw new Error(
      `Username is missing. It must be provided as an argument to the 'admin-respond-to-auth-challenge' command.`
    );
  }
};

const verifyTotp = (totp) => {
  if (!totp) {
    throw new Error(
      `Time-based one-time password (TOTP) is missing. It must be provided as an argument to the 'admin-respond-to-auth-challenge' command.`
    );
  }
};

const storeAccessToken = (token) => {
  process.env.AccessToken = token;
};

const adminRespondToAuthChallengeHandler = async (commands) => {
  const [_, username, totp] = commands;

  try {
    verifyUsername(username);
    verifyTotp(totp);

    const [userPoolId, clientId] = getFirstEntry(FILE_USER_POOLS);
    const session = process.env.SESSION;

    const { AuthenticationResult } = await adminRespondToAuthChallenge({
      clientId,
      userPoolId,
      username,
      totp,
      session,
    });

    storeAccessToken(AuthenticationResult.AccessToken);

    log("Successfully authenticated.");
  } catch (err) {
    log(err);
  }
};

export { adminRespondToAuthChallengeHandler };

const respondToAuthChallenge = async ({
  clientId,
  username,
  session,
  userPoolId,
  code,
}) => {
  const client = createClientForDefaultRegion(CognitoIdentityProviderClient);

  const command = new RespondToAuthChallengeCommand({
    ChallengeName: ChallengeNameType.SOFTWARE_TOKEN_MFA,
    ChallengeResponses: {
      SOFTWARE_TOKEN_MFA_CODE: code,
      USERNAME: username,
    },
    ClientId: clientId,
    UserPoolId: userPoolId,
    Session: session,
  });

  return client.send(command);
};

import { log } from "../../../../libs/utils/util-log.js";
import { verifySoftwareToken } from "../../../actions/verify-software-token.js";

const validateTotp = (totp) => {
  if (!totp) {
    throw new Error(
      `Time-based one-time password (TOTP) must be provided to the 'validate-software-token' command.`
    );
  }
};
const verifySoftwareTokenHandler = async (commands) => {
  const [_, totp] = commands;

  try {
    validateTotp(totp);

    log("Verifying TOTP.");
    await verifySoftwareToken(totp);
    log("TOTP Verified. Run 'admin-initiate-auth' again to sign-in.");
  } catch (err) {
    console.log(err);
  }
};

export { verifySoftwareTokenHandler };

const verifySoftwareToken = async (totp) => {
  const client = createClientForDefaultRegion(CognitoIdentityProviderClient);
  // The 'Session' is provided in the response to 'AssociateSoftwareToken'.
  const session = process.env.SESSION;

  if (!session) {
    throw new Error(
      "Missing a valid Session. Did you run 'admin-initiate-auth'?"
    );
  }

  const command = new VerifySoftwareTokenCommand({
    Session: session,
    UserCode: totp,
  });

  return client.send(command);
};
```
+ For API details, see the following topics in *AWS SDK for JavaScript API Reference*\.
  + [AdminGetUser](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/admingetusercommand.html)
  + [AdminInitiateAuth](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/admininitiateauthcommand.html)
  + [AdminRespondToAuthChallenge](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/adminrespondtoauthchallengecommand.html)
  + [AssociateSoftwareToken](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/associatesoftwaretokencommand.html)
  + [ConfirmDevice](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/confirmdevicecommand.html)
  + [ConfirmSignUp](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/confirmsignupcommand.html)
  + [InitiateAuth](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/initiateauthcommand.html)
  + [ListUsers](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/listuserscommand.html)
  + [ResendConfirmationCode](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/resendconfirmationcodecommand.html)
  + [RespondToAuthChallenge](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/respondtoauthchallengecommand.html)
  + [SignUp](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/signupcommand.html)
  + [VerifySoftwareToken](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-cognito-identity-provider/classes/verifysoftwaretokencommand.html)