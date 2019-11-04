# Setting Credentials in a Web Browser<a name="setting-credentials-browser"></a>

There are several ways to supply your credentials to the SDK from browser scripts\. Some of these are more secure and others afford greater convenience while developing a script\. Here are the ways you can supply your credentials in order of recommendation:

1. Using Amazon Cognito Identity to authenticate users and supply credentials

1. Using web federated identity

1. Hard coded in the script

**Warning**  
We do not recommend hard coding your AWS credentials in your scripts\. Hard coding credentials poses a risk of exposing your access key ID and secret access key\.

**Topics**
+ [Using Amazon Cognito Identity to Authenticate Users](loading-browser-credentials-cognito.md)
+ [Using Web Federated Identity to Authenticate Users](loading-browser-credentials-federated-id.md)
+ [Web Federated Identity Examples](config-web-identity-examples.md)