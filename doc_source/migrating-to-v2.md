# Migrating Your Code to SDK for JavaScript V3<a name="migrating-to-v2"></a>

There are a number of paths you can take to migrate to the SDK for JavaScript version 3\. For new code we recommend level 2\.

------
#### [ Level 0 ]

Do nothing\. The SDK supports almost all of the existing code paths\.

------
#### [ Level 1 ]

Perform minimal changes:
+ Install only the service\-specific packages you need\.
+ Create and use V3 service clients, replacing the use of any global configuration values, such as Region, with configuration values passed in as arguments to the client\.

------
#### [ Level 2 ]

Level 1 plus use the asynch/await programming model\.

------

The details of these changes are described in the following sections\.

**Note**  
Many of the code examples in the guide have not been migrated to version 3\. To take full advantage of V3 features, make the Level 2 changes to the V2 code examples before using them\.

## Level 2 Examples<a name="level2-examples"></a>

The following example installs the package for Amazon S3\. 

```
npm install @aws-sdk/client-s3
```

The following example loads the Amazon S3 service\.

```
const S3 = require('@aws-sdk/client-s3')
```

The following example creates an Amazon S3 client\.

```
const s3Client = new S3.S3Client()
```

The following example creates an Amazon S3 client in the `us-west-2` Region\. 

```
const s3Client = new S3.S3Client({region: 'us-west-2'})
```