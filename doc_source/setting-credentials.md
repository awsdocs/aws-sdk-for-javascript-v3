# Setting Credentials<a name="setting-credentials"></a>

AWS uses credentials to identify who is calling services and whether access to the requested resources is allowed\. In AWS, these credentials are typically the access key ID and the secret access key that were created along with your account\.

Whether running in a web browser or in a Node\.js server, your JavaScript code must obtain valid credentials before it can access services through the API\. Credentials can be set globally on the configuration object, using `AWS.Config`, or per service, by passing credentials directly to a service object\.

There are several ways to set credentials that differ between Node\.js and JavaScript in web browsers\. The topics in this section describe how to set credentials in Node\.js or web browsers\. In each case, the options are presented in recommended order\.

## Best Practices for Credentials<a name="credentials-best-practices"></a>

Properly setting credentials ensures that your application or browser script can access the services and resources needed while minimizing exposure to security issues that may impact mission critical applications or compromise sensitive data\.

An important principle to apply when setting credentials is to always grant the least privilege required for your task\. It's more secure to provide minimal permissions on your resources and add further permissions as needed, rather than provide permissions that exceed the least privilege and, as a result, be required to fix security issues you might discover later\. For example, unless you have a need to read and write individual resources, such as objects in an Amazon S3 bucket or a DynamoDB table, set those permissions to read only\.

For more information on granting the least privilege, see the [Grant Least Privilege](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege) section of the Best Practices topic in the *IAM User Guide*\.

**Warning**  
While it is possible to do so, we recommend you not hard code credentials inside an application or browser script\. Hard coding credentials poses a risk of exposing your access key ID and secret access key\.

For more information about how to manage your access keys, see [ Best Practices for Managing AWS Access Keys](https://docs.aws.amazon.com/general/latest/gr/aws-access-keys-best-practices.html) in the AWS General Reference\.

**Topics**
+ [Best Practices for Credentials](#credentials-best-practices)
+ [Setting Credentials in Node\.js](setting-credentials-node.md)
+ [Setting Credentials in a Web Browser](setting-credentials-browser.md)