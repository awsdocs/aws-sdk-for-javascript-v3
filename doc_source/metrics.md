# SDK Metrics in the AWS SDK for JavaScript<a name="metrics"></a>

AWS SDK Metrics for Enterprise Support \(SDK Metrics\) enables Enterprise customers to collect metrics from AWS SDKs on their hosts and clients that are shared with AWS Enterprise Support\. SDK Metrics provides information that helps speed up detection and diagnosis of issues occurring in connections to AWS services for AWS Enterprise Support customers\.

As telemetry is collected on each host, it is relayed via UDP to 127\.0\.0\.1 \(localhost\), where the Amazon CloudWatch agent aggregates the data and sends it to the SDK Metrics service\. Therefore, to receive metrics, the CloudWatch agent must be added to your instance\.

Learn more about [SDK Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch-Agent-SDK-Metrics.html) in the *Amazon CloudWatch User Guide*\.

The following topics describe how to configure and manage SDK Metrics for the SDK for JavaScript\.

**Topics**
+ [Authorize SDK Metrics to Collect and Send Metrics](#metrics-set-permissions)
+ [Set Up SDK Metrics for the SDK for JavaScript](#metrics-configure)
+ [Enable SDK Metrics for the AWS SDK for JavaScript](#metrics-enable)
+ [Enabling Client\-Side Monitoring with Scope](#metrics-client-side)
+ [Update a CloudWatch Agent](#metrics-update-cw)
+ [Disable SDK Metrics](#metrics-disable)
+ [Definitions for SDK Metrics](#metrics-definitions)

## Authorize SDK Metrics to Collect and Send Metrics<a name="metrics-set-permissions"></a>

Enterprise customers who want to collect metrics from AWS SDKs using AWS SDK Metrics for Enterprise Support must create an AWS Identity and Access Management \(IAM\) role that gives the CloudWatch agent permission to gather data from their Amazon Elastic Compute Cloud \(Amazon EC2\) instance or production environment\.

For details about how to create an IAM policy and role to access SDK Metrics, see [IAM Permissions for SDK Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Set-IAM-Permissions-For-SDK-Metrics.html) in the *Amazon CloudWatch User Guide*\.

## Set Up SDK Metrics for the SDK for JavaScript<a name="metrics-configure"></a>

The following steps demonstrate how to set up SDK Metrics for the AWS SDK for JavaScript\. These steps apply to an Amazon EC2 instance running Amazon Linux for a client application that is using the SDK for JavaScript\. SDK Metrics is also available for your production environments if you enable it while configuring the SDK for JavaScript\.

To use SDK Metrics, run the latest version of the CloudWatch agent\. Learn how to [Configure the CloudWatch agent for SDK Metrics](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/Configure-CloudWatch-Agent-SDK-Metrics.html) in the *Amazon CloudWatch User Guide*\.

**To set up SDK Metrics with the SDK for JavaScript**

1. Create an application with an AWS SDK for JavaScript client to use an AWS service\.

1. Host your project on an Amazon EC2 instance or in your local environment\.

1. Install and use the latest version of the SDK for JavaScript\.

1. Install and configure a CloudWatch agent on an Amazon EC2 instance or in your local environment\.

1. Authorize SDK Metrics to collect and send metrics\.

1. [Enable SDK Metrics for the SDK for JavaScript\.](#metrics-enable)

See also:
+ [Update a CloudWatch Agent](#metrics-update-cw)
+ [Disable SDK Metrics](#metrics-disable)

## Enable SDK Metrics for the AWS SDK for JavaScript<a name="metrics-enable"></a>

By default, SDK Metrics is turned off and the port is set to 31000\. The following are the default parameters\.

```
//default values
[
    'enabled' => false,
    'port' => 31000,

]
```

Enabling SDK Metrics is independent of configuring your credentials to use an AWS service\.

You can enable SDK Metrics by [setting environment variables](#metrics-enable-env-var) or by using the [shared AWS config file](#metrics-enable-config-file)\.

### Option 1: Set Environment Variables<a name="metrics-enable-env-var"></a>

If `AWS_CSM_ENABLED` isn't set, the SDK checks the profile specified in the environment variable under `AWS_PROFILE` to determine if SDK Metrics is enabled\. By default this is set to `false`\.

To turn on SDK Metrics, add the following to your environmental variables\.

```
export AWS_CSM_ENABLED=true
```

**Note**  
[Other configuration settings](#metrics-update-cw) are available\.
Enabling SDK Metrics is independent of configuring your credentials to use an AWS service\.

### Option 2: Use the Shared AWS Config File<a name="metrics-enable-config-file"></a>

If no CSM configuration is found in the environment variables, the SDK looks for your default AWS profile field\. If `AWS_DEFAULT_PROFILE` is set to something other than `default`, update that profile\. 

To enable SDK Metrics, add `csm_enabled` to the shared config file located at `~/.aws/config`\.

```
[default]
csm_enabled = true

[profile aws_csm]
csm_enabled = true
```

**Note**  
[Other configuration settings](#metrics-update-cw) are available\.
Enabling SDK Metrics is independent of configuring your credentials to use an AWS service\. You can use a different profile to authenticate\.

## Enabling Client\-Side Monitoring with Scope<a name="metrics-client-side"></a>

In addition to enabling SDK Metrics through an environment variable or config file, as shown previously, you can also enable the collection of metrics for a specific scope\.

The following scenarios show the scopes for which you can enable SDK Metrics\.

For all service clients, you can turn on the client\-side monitoring in the service configuration as follows\.

```
const AWS = require('aws-sdk');
const s3 = new AWS.S3({clientSideMonitoring: true});
```

For subclasses, such as `AWS.S3.ManagedUpload` and `AWS.DynamoDB.DocumentClient`, you can turn on the client\-side monitoring, as shown in the following example\.

```
const AWS = require('aws-sdk');
const documentClient = new AWS.DynamoDB.DocumentClient();
documentClient.service.config.update({clientSideMonitoring: true});
```

You can also turn on the client\-side monitoring for all AWS services\.

```
AWS.config.clientSideMonitoring = true;
```

Or you can turn on the client\-side monitoring for all the clients created from the specific service\. For example:

```
AWS.S3.config.clientSideMonitoring = true;
```

## Update a CloudWatch Agent<a name="metrics-update-cw"></a>

To make changes to the port, you need to [set the value](#metrics-update-cw-add-strings) and then [restart](#metrics-update-cw-restart) any AWS jobs that are currently active\.

Most services use the default port\. But if your service requires a unique port ID, add that to the host's environment variables or the shared AWS config file\.

### Add a Port ID<a name="metrics-update-cw-add-strings"></a>

#### Option 1: Set Environment Variables<a name="metrics-update-cw-env-var"></a>

Add a unique port ID, `AWS_CSM_PORT=[some_port_number]`, to the host's environment variables\. 

For example:

```
export AWS_CSM_ENABLED=true

export AWS_CSM_PORT=1234
```

#### Option 2: Use a Shared AWS Config File<a name="metrics-update-cw-config-file"></a>

Add a unique port ID, `csm_port = [some_port_number]`, to the shared AWS config file at `~/.aws/config`\. 

For example:

```
[default]
csm_enabled = false

csm_port = 1234

[profile aws_csm]
csm_enabled = false

csm_port = 1234
```

### Restart SDK Metrics<a name="metrics-update-cw-restart"></a>

To restart a job, run the following commands\.

```
$ amazon-cloudwatch-agent-ctl –a stop;
$ amazon-cloudwatch-agent-ctl –a start;
```

## Disable SDK Metrics<a name="metrics-disable"></a>

To turn off SDK Metrics, [set](#metrics-disable-set-string) `csm_enabled` to `false` in your environment variables or in your shared AWS config file located at `~/.aws/config`\. Then [stop and restart](#metrics-disable-restart) your CloudWatch agent so that the changes can take effect\.

### Set csm\_enabled to false<a name="metrics-disable-set-string"></a>

#### Option 1: Set Environment Variables<a name="metrics-disable-env-var"></a>

```
export AWS_CSM_ENABLED=false
```

#### Option 2: Use the Shared AWS Config File<a name="metrics-disable-config-file"></a>

Remove `csm_enabled` from the profiles in your shared AWS config file located at `~/.aws/config`\.

**Note**  
Environment variables override the shared AWS config file\. If SDK Metrics is enabled in the environment variables, SDK Metrics remain enabled\.

```
[default]
csm_enabled = false

[profile aws_csm]
csm_enabled = false
```

### Stop SDK Metrics and Restart the CloudWatch Agent<a name="metrics-disable-restart"></a>

To disable SDK Metrics, use the following command to stop the CloudWatch agent\.

```
$ sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a stop && echo "Done"
```

If you are using other CloudWatch features, restart the CloudWatch agent using the following command\.

```
$ amazon-cloudwatch-agent-ctl –a start;
```

## Definitions for SDK Metrics<a name="metrics-definitions"></a>

To interpret your results, you can use the following descriptions of SDK Metrics\. In general, these metrics are available for review with your Technical Account Manager during regular business reviews\. 

AWS Support resources and your Technical Account Manager should have access to SDK Metrics data to help you resolve cases\. However, if you discover data that is confusing or unexpected but doesn’t seem to be negatively impacting your application performance, it is best to review that data during scheduled business reviews\.


| Metric: | CallCount | 
| --- |--- |
|  Definition  |  Total number of successful or failed API calls from your code to AWS services\.  | 
|  How to use it  |  Use as a baseline to correlate with other metrics, such as errors or throttling\.  | 


| Metric: | ClientErrorCount | 
| --- |--- |
|  Definition  |  Number of API calls that fail with client errors \(4xx HTTP response codes\)\.  Examples: Throttling, Access denied, S3 bucket does not exist, and Invalid parameter value\.  | 
|  How to use it  |  Except in certain cases related to throttling \(for example, when throttling occurs due to a limit that needs to be increased\) this metric can indicate something in your application that needs to be fixed\.  | 


| Metric: | ConnectionErrorCount | 
| --- |--- |
|  Definition  |  Number of API calls that fail because of errors connecting to the service\. These can be caused by network issues between the customer application and AWS services, including load balancers, DNS failures, and transit providers\. In some cases, AWS issues might result in this error\.  | 
|  How to use it  |  Use to determine whether issues are specific to your application or are caused by your infrastructure or network\. High `ConnectionErrorCount` could also indicate short timeout values for API calls\.  | 


| Metric: | ThrottleCount | 
| --- |--- |
|  Definition  |  Number of API calls that fail due to throttling by AWS services\.  | 
|  How to use it  |  Use this metric to assess if your application has reached throttle limits, and to determine the cause of retries and application latency\. Consider distributing calls over a window of time instead of batching your calls\.  | 


| Metric: | ServerErrorCount | 
| --- |--- |
|  Definition  |  Number of API calls that fail due to server errors \(5xx HTTP response codes\) from AWS services\. These are typically caused by AWS services\.  | 
|  How to use it  |  Determine cause of SDK retries or latency\. This metric does not always indicate that AWS services are at fault, because some AWS teams classify latency as an HTTP 503 response\.  | 


| Metric: |  EndToEndLatency | 
| --- |--- |
|  Definition  |  Total time for your application to make a call using the AWS SDK, inclusive of retries\. In other words, regardless of whether it is successful after several attempts, or as soon as a call fails due to an unretriable error\.  | 
|  How to use it  |  Determine how AWS API calls contribute to your application’s overall latency\. Higher than expected latency can be caused by issues with network, firewall, or other configuration settings, or by latency that occurs as a result of SDK retries\.  | 