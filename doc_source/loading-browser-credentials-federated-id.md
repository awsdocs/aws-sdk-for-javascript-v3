# Using Web Federated Identity to Authenticate Users<a name="loading-browser-credentials-federated-id"></a>

You can directly configure individual identity providers to access AWS resources using web identity federation\. AWS currently supports authenticating users using web identity federation through several identity providers:
+ [Login with Amazon](https://login.amazon.com)
+ [Facebook Login](https://www.facebook.com/about/login)
+ [Google Sign\-in](https://developers.google.com/identity/)

You must first register your application with the providers that your application supports\. Next, create an IAM role and set up permissions for it\. The IAM role you create is then used to grant the permissions you configured for it through the respective identity provider\. For example, you can set up a role that allows users who logged in through Facebook to have read access to a specific Amazon S3 bucket you control\.

After you have both an IAM role with configured privileges and an application registered with your chosen identity providers, you can set up the SDK to get credentials for the IAM role\.

## Step 1: Registering with Identity Providers<a name="config-web-identity-register"></a>

To begin, register an application with the identity providers you choose to support\. You will be asked to provide information that identifies your application and possibly its author\. This ensures that the identity providers know who is receiving their user information\. In each case, the identity provider will issue an application ID that you use to configure user roles\.

## Step 2: Creating an IAM Role for an Identity Provider<a name="config-web-identity-role"></a>

After you obtain the application ID from an identity provider, use the [IAM console](https://console.aws.amazon.com/iam) to create a new IAM role\. See https://docs\.aws\.amazon\.com/cognito/latest/developerguide/iam\-roles\.html for details\.  

## Step 3: Obtaining a Provider Access Token After Login<a name="config-web-identity-obtain-token"></a>

Set up the login action for your application by using the identity provider's SDK\. You can download and install a JavaScript SDK from the identity provider that enables user login, using either OAuth or OpenID\. For information on how to download and set up the SDK code in your application, see the SDK documentation for your identity provider:
+ [Login with Amazon](https://login.amazon.com/website)
+ [Facebook Login](https://developers.facebook.com/docs/javascript)
+ [Google Sign\-in](https://developers.google.com/identity/)

## Step 4: Obtaining Temporary Credentials<a name="config-web-identity-get-credentials"></a>

After your application, roles, and resource permissions are configured, add the code to your application to obtain temporary credentials\. These credentials are provided through the AWS Security Token Service using web identity federation\. Users log in to the identity provider, which returns an access token\. Set up the `AWS.WebIdentityCredentials` object using the ARN for the IAM role you created for this identity provider\.

```
AWS.config.credentials = new AWS.WebIdentityCredentials({
    RoleArn: 'arn:aws:iam::AWS_ACCOUNT_ID:role/WEB_IDENTITY_ROLE_NAME',
    ProviderId: 'PROVIDER_ID',
    WebIdentityToken: ACCESS_TOKEN
});
```

The value for *PROVIDER\_ID* depends on the specified identity provider: **graph\.facebook\.com** for Facebook, **www\.amazon\.com** for Amazon, and **null** for Google\. The value for *ACCESS\_TOKEN* is the access token retrieved from a successful login with the identity provider\. For more information about how to configure and retrieve access tokens for each identity provider, see the documentation from the identity provider\.

Service objects that are created subsequently will have the proper credentials\.

You can also create credentials before retrieving the access token\. This allows you to create service objects that depend on credentials before loading the access token\. To do this, create the credentials object without the `WebIdentityToken` parameter\.

```
AWS.config.credentials = new AWS.WebIdentityCredentials({
    RoleArn: 'arn:aws:iam::AWS_ACCOUNT_ID:role/WEB_IDENTITY_ROLE_NAME',
    ProviderId: 'PROVIDER_ID'
});
```

Then set the `WebIdentityToken` in the callback from the identity provider SDK that contains the access token\.

```
S3Client.credentials.WebIdentityToken = ACCESS_TOKEN
```