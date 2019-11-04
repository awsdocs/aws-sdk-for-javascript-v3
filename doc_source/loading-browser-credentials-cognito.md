# Using Amazon Cognito Identity to Authenticate Users<a name="loading-browser-credentials-cognito"></a>

The recommended way to obtain AWS credentials for your browser scripts is to use the Amazon Cognito Identity credentials client, `client-cognito-identity`\. Amazon Cognito enables authentication of users through third\-party identity providers\.

To use Amazon Cognito Identity, you must first create an identity pool in the Amazon Cognito console\. An identity pool represents the group of identities that your application provides to your users\. The identities given to users uniquely identify each user account\. Amazon Cognito identities are not credentials\. They are exchanged for credentials using web identity federation support in AWS Security Token Service \(AWS STS\)\.

Amazon Cognito helps you manage the abstraction of identities across multiple identity providers\. The identity that is loaded is then exchanged for credentials in AWS STS\.

## Configuring the Amazon Cognito Identity Credentials Object<a name="browser-cognito-configuration"></a>

If you have not yet created one, create an identity pool to use with your browser scripts in the [Amazon Cognito console](https://console.aws.amazon.com/cognito) before you configure your Amazon Cognito client\. Create and associate both authenticated and unauthenticated IAM roles for your identity pool\.

Unauthenticated users do not have their identity verified, making this role appropriate for guest users of your app or in cases when it doesn't matter if users have their identities verified\. Authenticated users log in to your application through a third\-party identity provider that verifies their identities\. Make sure you scope the permissions of resources appropriately so you don't grant access to them from unauthenticated users\.

After you configure an identity pool, use the Amazon Cognito CognitoIdentityCredentials method to create the credentials to authenticate users, as shown in the following example of creating an Amazon S3 client in the `us-west-2` Region for users in the *IDENTITY\_POOL\_ID* identity pool:

```
const s3Client = new AWS.S3({
  region: 'us-west-2',
  credentials: new AWS.CognitoIdentityCredentials(
    {
      IdentityPoolId: 'IDENTITY_POOL_ID'
    },
    {
      region: 'us-west-2'
    }
  )
})
```

The optional `Logins` property is a map of identity provider names to the identity tokens for those providers\. How you get the token from your identity provider depends on the provider you use\. For example, if Facebook is one of your identity providers, you might use the `FB.login` function from the [Facebook SDK](https://developers.facebook.com/docs/facebook-login/web) to get an identity provider token:

```
FB.login(function (response) {
  if (response.authResponse) { // logged in
    credentials = new AWS.CognitoIdentityCredentials({
      IdentityPoolId: 'us-east-1:1699ebc0-7900-4099-b910-2df94f52a030',
      Logins: {
        'graph.facebook.com': 'response.authResponse.accessToken'
      })

    s3Client = new AWS.S3({ credentials: credentials });

    console.log('You are now logged in.');
  } else {
    console.log('There was a problem logging you in.');
  }
});
```

## Switching Unauthenticated Users to Authenticated Users<a name="browser-switching-unauthenticated-users"></a>

Amazon Cognito supports both authenticated and unauthenticated users\. Unauthenticated users receive access to your resources even if they aren't logged in with any of your identity providers\. This degree of access is useful to display content to users prior to logging in\. Each unauthenticated user has a unique identity in Amazon Cognito even though they have not been individually logged in and authenticated\.

### Initially Unauthenticated User<a name="browser-initially-unauthenticated-user"></a>

Users typically start with the unauthenticated role, for which you set the credentials property of your configuration object without a `Logins` property\. In this case, your default configuration might look like the following:

```
// set the default config object
var creds = new AWS.CognitoIdentityCredentials({
 IdentityPoolId: 'us-east-1:1699ebc0-7900-4099-b910-2df94f52a030'
});
AWS.config.credentials = creds;
```

### Switch to Authenticated User<a name="switch-to-authenticated"></a>

When an unauthenticated user logs in to an identity provider and you have a token, you can switch the user from unauthenticated to authenticated by calling a custom function that updates the credentials object and adds the `Logins` token:

```
// Called when an identity provider has a token for a logged in user
function userLoggedIn(providerName, token) {
  creds.params.Logins = creds.params.Logins || {};
  creds.params.Logins[providerName] = token;
                    
  // Expire credentials to refresh them on the next request
  creds.expired = true;
}
```

You can also Create `CognitoIdentityCredentials` object\. If you do, you must reset the credentials properties of existing service objects you created\. Service objects read from the global configuration only on object initialization\. 

For more information about the `CognitoIdentityCredentials` object, see [ AWS\.CognitoIdentityCredentials](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/CognitoIdentityCredentials.html) in the AWS SDK for JavaScript API Reference\. 