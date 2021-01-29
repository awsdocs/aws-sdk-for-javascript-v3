--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Configuration per service<a name="global-config-object"></a>

You can configure the SDK by passing configuration information to a service object\.

Service\-level configuration provides significant control over individual services, enabling you to update the configuration of individual service objects when your needs vary from the default configuration\.

**Note**  
In version 2\.x of the AWS SDK for JavaScript service configuration could be passed to individual client constructors\. However, these configurations would first be merged automatically into a copy of the global SDK configuration `AWS.config`\.  
Also, calling `AWS.config.update({/* params *})` only updated configuration for service clients instantiated after the update call was made, not any existing clients\.  
This behavior was a frequent source of confusion, and made it difficult to add configuration to the global object that only affects a subset of service clients in a forward\-compatible way\. In version 3 , there is no longer a global configuration managed by the SDK\. Configuration must be passed to each service client that is instantiated\. It is still possible to share the same configuration across multiple clients but that configuration will not be automatically merged with a global state\.

## Setting configuration per service<a name="service-specific-configuration"></a>

Each service that you use in the SDK for JavaScript is accessed through a service object that is part of the API for that service\. For example, to access the Amazon S3 service you create the Amazon S3 service object\. You can specify configuration settings that are specific to a service as part of the constructor for that service object\. 

For example, if you need to access Amazon EC2 objects in multiple Regions, create an EC2 service object for each Region and then set the Region configuration of each service object accordingly\.

```
var ec2_regionA = new EC2({region: 'ap-southeast-2', maxRetries: 15, apiVersion: '2014-10-01'});
var ec2_regionB = new EC2({region: 'us-west-2', maxRetries: 15, apiVersion: '2014-10-01'});
```