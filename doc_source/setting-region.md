# Setting the AWS Region<a name="setting-region"></a>

An AWS Region is a named set of AWS resources in the same geographical area\. An example of a Region is `us-east-1`, which is the US East \(N\. Virginia\) Region\. You specify a Region when creating a service client in the SDK for JavaScript so that the SDK accesses the service in that Region\. Some services are available only in specific Regions\.

The SDK for JavaScript doesn't select a Region by default\. However, you can set the Region using an environment variable, a shared `config` file, or the global configuration object\.

## In a Client Class Constructor<a name="setting-region-constructor"></a>

When you instantiate a service object, you can specify the Region for that resource as part of the client class constructor, as shown here\.

```
const s3Client = new S3.S3Client({region: 'us-west-2'})
```

## Using an Environment Variable<a name="setting-region-environment-variable"></a>

You can set the Region using the `AWS_REGION` environment variable\. If you define this variable, the SDK for JavaScript reads it and uses it\.

## Using a Shared Config File<a name="setting-region-config-file"></a>

Much like the shared credentials file lets you store credentials for use by the SDK, you can keep your Region and other configuration settings in a shared file named `config` that is used by SDKs\. If the `AWS_SDK_LOAD_CONFIG` environment variable has been set to a truthy value, the SDK for JavaScript automatically searches for a `config` file when it loads\. Where you save the `config` file depends on your operating system:
+ Linux, macOS, or Unix users: `~/.aws/config`
+ Windows users: `C:\Users\USER_NAME\.aws\config`

If you don't already have a shared `config` file, you can create one in the designated directory\. In the following example, the `config` file sets both the Region and the output format\.

```
[default]
   region=us-west-2
   output=json
```

For more information about using shared config and credentials files, see [Loading Credentials in Node\.js from the Shared Credentials File](loading-node-credentials-shared.md) or [Configuration and Credential Files](https://docs.aws.amazon.com/cli/latest/userguide/cli-config-files.html) in the *AWS Command Line Interface User Guide*\.

## Order of Precedence for Setting the Region<a name="setting-region-order-of-precedence"></a>

The order of precedence for Region setting is as follows:

1. If a Region is passed to a client class constructor, that Region is used\.

1. Otherwise, if the `AWS_REGION` environment variable is a [truthy](https://developer.mozilla.org/en-US/docs/Glossary/Truthy) value, that Region is used\.

1. Otherwise, if the `AMAZON_REGION` environment variable is a truthy value, that Region is used\.