--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Using Amazon Cognito Identity to authenticate users<a name="loading-browser-credentials-cognito"></a>

The recommended way to obtain AWS credentials for your browser scripts is to use the Amazon Cognito Identity credentials client, `client-cognito-identity`\. Amazon Cognito enables authentication of users through third\-party identity providers\.

To use Amazon Cognito Identity, you must first create an identity pool in the Amazon Cognito console\. An identity pool represents the group of identities that your application provides to your users\. The identities given to users uniquely identify each user account\. Amazon Cognito identities are not credentials\. They are exchanged for credentials using web identity federation support in AWS Security Token Service \(AWS STS\)\.

Amazon Cognito helps you manage the abstraction of identities across multiple identity providers\. The identity that is loaded is then exchanged for credentials in AWS STS\.

## Configuring the Amazon Cognito Identity credentials object<a name="browser-cognito-configuration"></a>

If you have not yet created one, create an identity pool to use with your browser scripts in the [Amazon Cognito console](https://console.aws.amazon.com/cognito) before you configure your Amazon Cognito client\. Create and associate both authenticated and unauthenticated IAM roles for your identity pool\. For more information, see [https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-create-identity-pool.html](https://docs.aws.amazon.com/cognito/latest/developerguide/tutorial-create-identity-pool.html)\.

Unauthenticated users don't have their identity verified, making this role appropriate for guest users of your app or in cases when it doesn't matter if users have their identities verified\. Authenticated users log in to your application through a third\-party identity provider that verifies their identities\. Make sure you scope the permissions of resources appropriately so you don't grant access to them from unauthenticated users\.

After you configure an identity pool, use the Amazon Cognito `CognitoIdentityCredentials` method to create the credentials to authenticate users\. This is shown in the following example of creating an Amazon S3 client in the `us-west-2` Region for users in the *IDENTITY\_POOL\_ID* identity pool\.

```
const cognitoIdentityClient = new CognitoIdentityClient({
  region: "us-west-2"
});

const s3Client = new S3Client({
  region: "us-west-2",
  credentials: fromCognitoIdentityPool({
    client: cognitoIdentityClient,
    identityPoolId: 'IDENTITY_POOL_ID'
  })
});
```

The optional `Logins` property is a map of identity provider names to the identity tokens for those providers\. How you get the token from your identity provider depends on the provider you use\. For example, if Facebook is one of your identity providers, you might use the `FB.login` function from the [Facebook SDK](https://developers.facebook.com/docs/facebook-login/web) to get an identity provider token\.

```
FB.login(function (response) {
  if (response.authResponse) { 
    // logged in
    const cognitoIdentityClient = new CognitoIdentityClient({
      region: "us-west-2"
    });
    const s3Client = new S3Client({
      region: "us-west-2",
      credentials: fromCognitoIdentityPool({
        client: cognitoIdentityClient,
        IdentityPoolId: 'IDENTITY_POOL_ID',
        Logins: {
           "graph.facebook.com': 'response.authResponse.accessToken"
        },
      }),
    });
    console.log('You are now logged in.');
  } else {
    console.log('There was a problem logging you in.');
  }
});
```