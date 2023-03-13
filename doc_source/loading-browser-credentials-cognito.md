--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Using Amazon Cognito Identity to authenticate users<a name="loading-browser-credentials-cognito"></a>

The recommended way to obtain AWS credentials for your browser scripts is to use the Amazon Cognito Identity credentials client `CognitoIdentityClient`\. Amazon Cognito enables authentication of users through third\-party identity providers\.

To use Amazon Cognito Identity, you must first create an identity pool in the Amazon Cognito console\. An identity pool represents the group of identities that your application provides to your users\. The identities given to users uniquely identify each user account\. Amazon Cognito identities are not credentials\. They are exchanged for credentials using web identity federation support in AWS Security Token Service \(AWS STS\)\.

Amazon Cognito helps you manage the abstraction of identities across multiple identity providers\. The identity that is loaded is then exchanged for credentials in AWS STS\.

## Configuring the Amazon Cognito Identity credentials object<a name="browser-cognito-configuration"></a>

If you have not yet created one, create an identity pool to use with your browser scripts in the [Amazon Cognito console](https://console.aws.amazon.com/cognito) before you configure your Amazon Cognito client\. Create and associate both authenticated and unauthenticated IAM roles for your identity pool\. For more information, see [https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-create-identity-pool.html](https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-create-identity-pool.html)\.

Unauthenticated users don't have their identity verified, making this role appropriate for guest users of your app or in cases when it doesn't matter if users have their identities verified\. Authenticated users log in to your application through a third\-party identity provider that verifies their identities\. Make sure you scope the permissions of resources appropriately so you don't grant access to them from unauthenticated users\.

After you configure an identity pool, use the `fromCognitoIdentityPool` method from the `@aws-sdk/credential-providers` to retrieve the cendentials from the identity pool\. This is shown in the following example of creating an Amazon S3 client in the `us-west-2` AWS Region for users in the *IDENTITY\_POOL\_ID* identity pool\.

```
// Import required AWS SDK clients and command for Node.js
import {S3Client} from "@aws-sdk/client-s3";
import {fromCognitoIdentityPool} from "@aws-sdk/credential-providers";

const REGION = AWS_REGION               

const s3Client = new S3Client({
  region: REGION,
  credentials: fromCognitoIdentityPool({
    clientConfig: { region: REGION }, // Configure the underlying CognitoIdentityClient.
    identityPoolId: 'IDENTITY_POOL_ID',
    logins: {
            // Optional tokens, used for authenticated login.
        },
  })
});
```

The optional `logins` property is a map of identity provider names to the identity tokens for those providers\. How you get the token from your identity provider depends on the provider you use\. For example, if you are using an Amazon Cognito user pool as your authentication provider, you could use a method similar to the one below\.

```
// Get the Amazon Cognito ID token for the user. 'getToken()' below.
let idToken = getToken();
let COGNITO_ID = "COGNITO_ID"; // 'COGNITO_ID' has the format 'cognito-idp.REGION.amazonaws.com/COGNITO_USER_POOL_ID'
let loginData = {
  [COGNITO_ID]: idToken,
};
const s3Client = new S3Client({
    region: REGION,
    credentials: fromCognitoIdentityPool({
    clientConfig: { region: REGION }, // Configure the underlying CognitoIdentityClient.
    identityPoolId: 'IDENTITY_POOL_ID',
    logins: loginData
  })
});

// Strips the token ID from the URL after authentication.
window.getToken = function () {
  var idtoken = window.location.href;
  var idtoken1 = idtoken.split("=")[1];
  var idtoken2 = idtoken1.split("&")[0];
  var idtoken3 = idtoken2.split("&")[0];
  return idtoken3;
};
```

## Switching Unauthenticated Users to Authenticated Users<a name="browser-switching-unauthenticated-users"></a>

Amazon Cognito supports both authenticated and unauthenticated users\. Unauthenticated users receive access to your resources even if they aren't logged in with any of your identity providers\. This degree of access is useful to display content to users prior to logging in\. Each unauthenticated user has a unique identity in Amazon Cognito even though they have not been individually logged in and authenticated\.

### Initially Unauthenticated User<a name="browser-initially-unauthenticated-user"></a>

Users typically start with the unauthenticated role, for which you set the credentials property of your configuration object without a `logins` property\. In this case, your default credentials might look like the following:

```
// Import the required AWS SDK for JavaScript v3 modules.                   
import {fromCognitoIdentityPool} from "@aws-sdk/credential-providers";
// Set the default credentials.
const creds = new fromCognitoIdentityPool({
  IdentityPoolId: "IDENTITY_POOL_ID",
  clientConfig({ region: REGION }) // Configure the underlying CognitoIdentityClient.
});
```

### Switch to Authenticated User<a name="switch-to-authenticated"></a>

When an unauthenticated user logs in to an identity provider and you have a token, you can switch the user from unauthenticated to authenticated by calling a custom function that updates the credentials object and adds the `logins` token\.

```
// Called when an identity provider has a token for a logged in user
function userLoggedIn(providerName, token) {
  creds.params.Logins = creds.params.logins || {};
  creds.params.Logins[providerName] = token;
                    
  // Expire credentials to refresh them on the next request
  creds.expired = true;
}
```