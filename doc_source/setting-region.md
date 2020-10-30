--------

This is a preview version of the Developer Guide for the AWS SDK for JavaScript Version 3 \(V3\)\.

A preview version of the AWS SDK for JavaScript V3 is available on [Github](https://github.com/aws/aws-sdk-js-v3)\.

Help us improve the AWS SDK for JavaScript documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

--------

# Setting the AWS Region<a name="setting-region"></a>

An AWS Region is a named set of AWS resources in the same geographical area\. An example of a Region is `us-east-1`, which is the US West \(Oregon\) Region\. You specify a Region when creating a service client in the SDK for JavaScript so that the SDK accesses the service in that Region\. Some services are available only in specific Regions\.

The SDK for JavaScript doesn't select a Region by default\. However, you can set the Region using an environment variable, a shared configuration `config` file, or the global configuration object\.

## In a client class constructor<a name="setting-region-constructor"></a>

When you instantiate a service object, you can specify the AWS Region for that resource as part of the client class constructor, as shown here\.

```
const s3Client = new S3.S3Client({region: 'us-west-2'});
```

## Using an environment variable<a name="setting-region-environment-variable"></a>

You can set the Region using the `AWS_REGION` environment variable\. If you define this variable, the SDK for JavaScript reads it and uses it\.

## Using a shared config file<a name="setting-region-config-file"></a>

Much like the shared credentials file lets you store credentials for use by the SDK, you can keep your AWS Region and other configuration settings in a shared file named `config` for the SDK to use\. If the `AWS_SDK_LOAD_CONFIG` environment variable is set to a truthy value, the SDK for JavaScript automatically searches for a `config` file when it loads\. Where you save the `config` file depends on your operating system:
+ Linux, macOS, or Unix users \- `~/.aws/config`
+ Windows users \- `C:\Users\USER_NAME\.aws\config`

If you don't already have a shared `config` file, you can create one in the designated directory\. In the following example, the `config` file sets both the Region and the output format\.

```
[default]
   region=us-west-2
   output=json
```

For more information about using shared `config` and credentials files, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md) or [Configuration and credential files](https://docs.aws.amazon.com/cli/latest/userguide/cli-config-files.html) in the *AWS Command Line Interface User Guide*\.

## Order of precedence for setting the Region<a name="setting-region-order-of-precedence"></a>

The following is the order of precedence for Region setting:

1. If a Region is passed to a client class constructor, that Region is used\.

1. Otherwise, if the `AWS_REGION` environment variable is a [truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) value, that Region is used\.

1. Otherwise, if the `AMAZON_REGION` environment variable is a truthy value, that Region is used\.