# Upgrading the SDK for JavaScript from Version 2<a name="upgrading-from-v2"></a>

The following notes help you upgrade the SDK for JavaScript from version 2 to version 3\.

**Note**  
Many of the code examples in the guide have not been ported to version 3\. To take advantage of V3 features in these code examples, you should make these changes to the V2 code examples before you use them\.

## Installing Packages<a name="upgrading-from-v2-installing-packages"></a>

 In V2 you installed the `aws-sdk` package thusly: 

```
npm install aws-sdk
```

 In V3 you install packages for individual services, such as Amazon S3: 

```
npm install @aws-sdk/client-s3
```

## Loading the SDK<a name="upgrading-from-v2-loading-sdk"></a>

 In V2 you loaded the entire set of services: 

```
var AWS = require('aws-sdk');
```

 In V3 you load only the services you need, such as Amazon S3: 

```
const S3 = require('@aws-sdk/client-s3');
```

## Creating Clients<a name="upgrading-from-v2-creating-clients"></a>

 In V2 you created service clients thusly: 

```
var s3Client = new AWS.S3({apiVersion: '2006-03-01'});
```

 In V3, you don't need the `AWS` prefix or the `apiVersion` argument: 

```
const s3Client = new S3.S3Client();
```

## AWS\.Config\.update<a name="upgrading-from-v2-config-update"></a>

 In V2 you used `AWS.Config.update` to apply configuration settings to any client you subsequently created\. 

```
AWS.config.update({region: 'us-west-2'});
```

 In V3, you specify configuration settings when you create the service client, such as for Amazon S3: 

```
const s3Client = new S3.S3Client({region: 'us-west-2'});
```