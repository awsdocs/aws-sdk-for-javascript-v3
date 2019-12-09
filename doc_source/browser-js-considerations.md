# Browser Script Considerations<a name="browser-js-considerations"></a>

This topic describes special considerations for using the AWS SDK for JavaScript in browser scripts\.

## Cross\-Origin Resource Sharing \(CORS\)<a name="cors"></a>

Cross\-origin resource sharing, or CORS, is a security feature of modern web browsers\. It enables web browsers to negotiate which domains can make requests of external websites or services\. 

CORS is an important consideration when developing browser applications with the AWS SDK for JavaScript because most requests to resources are sent to an external domain, such as the endpoint for a web service\. If your JavaScript environment enforces CORS security, you must configure CORS with the service\.

CORS determines whether to allow sharing of resources in a cross\-origin request based on the following:
+ The specific domain that makes the request 
+ The type of HTTP request being made \(GET, PUT, POST, DELETE and so on\)

### How CORS Works<a name="how-cors-works"></a>

In the simplest case, your browser script makes a GET request for a resource from a server in another domain\. Depending on the CORS configuration of that server, if the request is from a domain that's authorized to submit GET requests, the cross\-origin server responds by returning the requested resource\.

If either the requesting domain or the type of HTTP request is not authorized, the request is denied\. However, CORS makes it possible to preflight the request before actually submitting it\. In this case, a preflight request is made in which the `OPTIONS` access request operation is sent\. If the cross\-origin server's CORS configuration grants access to the requesting domain, the server sends back a preflight response that lists all the HTTP request types that the requesting domain can make on the requested resource\.

![\[Process flow for CORS requests\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/cors-overview.png)

### Is CORS Configuration Required<a name="the-need-for-cors-configuration"></a>

Amazon S3 buckets require CORS configuration before you can perform operations on them\. In some JavaScript environments CORS may not be enforced and therefore configuring CORS is unnecessary\. For example, if you host your application from an Amazon S3 bucket and access resources from `*.s3.amazonaws.com` or some other specific endpoint, your requests won't access an external domain\. Therefore, this configuration doesn't require CORS\. In this case, CORS is still used for services other than Amazon S3\.

### Configuring CORS for an Amazon S3 Bucket<a name="configuring-cors-s3-bucket"></a>

You can configure an Amazon S3 bucket to use CORS in the Amazon S3 console\.

1. In the Amazon S3 console, find the bucket you want to configure and select its check box\.

1. In the pane that opens, choose **Permissions**\.

1. On the **Permission** tab, choose **CORS Configuration**\.

1. Type your CORS configuration in the **CORS Configuration Editor**, and then choose **Save**\.

A CORS configuration is an XML file that contains a series of rules within a `<CORSRule>`\. A configuration can have up to 100 rules\. A rule is defined by one of the following tags:
+ `<AllowedOrigin>` – Specifies domain origins that you allow to make cross\-domain requests\.
+ `<AllowedMethod>` – Specifies a type of request you allow \(GET, PUT, POST, DELETE, HEAD\) in cross\-domain requests\.
+ `<AllowedHeader>` – Specifies the headers allowed in a preflight request\.

For example configurations, see [How Do I Configure CORS on My Bucket?](https://docs.aws.amazon.com/AmazonS3/latest/dev/cors.html#how-do-i-enable-cors) in the *Amazon Simple Storage Service Developer Guide*\.

### CORS Configuration Example<a name="cors-configuration-example"></a>

The following CORS configuration example allows a user to view, add, remove, or update objects inside of a bucket from the domain `example.org`\. However, we recommend that you scope the `<AllowedOrigin>` to the domain of your website\. You can specify `"*"` to allow any origin\.

```
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <CORSRule>
    <AllowedOrigin>https://example.org</AllowedOrigin>
    <AllowedMethod>HEAD</AllowedMethod>
    <AllowedMethod>GET</AllowedMethod>
    <AllowedMethod>PUT</AllowedMethod>
    <AllowedMethod>POST</AllowedMethod>
    <AllowedMethod>DELETE</AllowedMethod>
    <AllowedHeader>*</AllowedHeader>
    <ExposeHeader>ETag</ExposeHeader>
    <ExposeHeader>x-amz-meta-custom-header</ExposeHeader>
  </CORSRule>
</CORSConfiguration>
```

This configuration does not authorize the user to perform actions on the bucket\. It enables the browser's security model to allow a request to Amazon S3\. Permissions must be configured through bucket permissions or IAM role permissions\.

You can use `ExposeHeader` to let the SDK read response headers returned from Amazon S3\. For example, if you want to read the `ETag` header from a `PUT` or multipart upload, you need to include the `ExposeHeader` tag in your configuration, as shown in the previous example\. The SDK can only access headers that are exposed through CORS configuration\. If you set metadata on the object, values are returned as headers with the prefix `x-amz-meta-`, such as `x-amz-meta-my-custom-header`, and must also be exposed in the same way\.