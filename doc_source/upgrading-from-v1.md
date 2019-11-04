# Upgrading the SDK for JavaScript from Version 1<a name="upgrading-from-v1"></a>

The following notes help you upgrade the SDK for JavaScript from version 1 to version 2\.

## Automatic Conversion of Base64 and Timestamp Types on Input/Output<a name="upgrading-from-v1-base64-timestamp-conversion"></a>

The SDK now automatically encodes and decodes base64\-encoded values, as well as timestamp values, on the user's behalf\. This change affects any operation where base64 or timestamp values were sent by a request or returned in a response that allows for base64\-encoded values\.

User code that previously converted base64 is no longer required\. Values encoded as base64 are now returned as buffer objects from server responses and can also be passed as buffer input\. For example, the following version 1 `SQS.sendMessage` parameters:

```
var params = {
   MessageBody: 'Some Message',
   MessageAttributes: {
      attrName: {
         DataType: 'Binary',
         BinaryValue: new Buffer('example text').toString('base64')
      }
   }
};
```

Can be rewritten as follows\.

```
var params = {
   MessageBody: 'Some Message',
   MessageAttributes: {
      attrName: {
         DataType: 'Binary',
         BinaryValue: 'example text'
      }
   }
};
```

Here is how the message is read\.

```
sqs.receiveMessage(params, function(err, data) {
  // buf is <Buffer 65 78 61 6d 70 6c 65 20 74 65 78 74>
  var buf = data.Messages[0].MessageAttributes.attrName.BinaryValue;
  console.log(buf.toString()); // "example text"
});
```

## Moved response\.data\.RequestId to response\.requestId<a name="upgrading-from-v1-response-requestid"></a>

The SDK now stores request IDs for all services in a consistent place on the `response` object, rather than inside the `response.data` property\. This improves consistency across services that expose request IDs in different ways\. This is also a breaking change that renames the `response.data.RequestId` property to `response.requestId` \(`this.requestId` inside a callback function\)\.

In your code, change the following:

```
svc.operation(params, function (err, data) {
  console.log('Request ID:', data.RequestId);
});
```

To the following:

```
svc.operation(params, function () {
  console.log('Request ID:', this.requestId);
});
```

## Exposed Wrapper Elements<a name="upgrading-from-v1-exposed-wrapper-elements"></a>

If you use `AWS.ElastiCache`, `AWS.RDS`, or `AWS.Redshift`, you must access the response through the top\-level output property in the response for some operations\. 

For example, the `RDS.describeEngineDefaultParameters` method used to return the following\.

```
{ Parameters: [ ... ] }
```

It now returns the following\.

```
{ EngineDefaults: { Parameters: [ ... ] } }
```

The list of affected operations for each service are shown in the following table\.


****  

| Client Class | Operations | 
| --- | --- | 
| `AWS.ElastiCache` | `authorizeCacheSecurityGroupIngress` `createCacheCluster` `createCacheParameterGroup` `createCacheSecurityGroup` `createCacheSubnetGroup` `createReplicationGroup` `deleteCacheCluster` `deleteReplicationGroup` `describeEngineDefaultParameters` `modifyCacheCluster` `modifyCacheSubnetGroup` `modifyReplicationGroup` `purchaseReservedCacheNodesOffering` `rebootCacheCluster` `revokeCacheSecurityGroupIngress` | 
| `AWS.RDS` | `addSourceIdentifierToSubscription` `authorizeDBSecurityGroupIngress` `copyDBSnapshot` `createDBInstance` `createDBInstanceReadReplica` `createDBParameterGroup` `createDBSecurityGroup` `createDBSnapshot` `createDBSubnetGroup` `createEventSubscription` `createOptionGroup` `deleteDBInstance` `deleteDBSnapshot` `deleteEventSubscription` `describeEngineDefaultParameters` `modifyDBInstance` `modifyDBSubnetGroup` `modifyEventSubscription` `modifyOptionGroup` `promoteReadReplica` `purchaseReservedDBInstancesOffering` `rebootDBInstance` `removeSourceIdentifierFromSubscription` `restoreDBInstanceFromDBSnapshot` `restoreDBInstanceToPointInTime` `revokeDBSecurityGroupIngress` | 
| `AWS.Redshift` | `authorizeClusterSecurityGroupIngress` `authorizeSnapshotAccess` `copyClusterSnapshot` `createCluster` `createClusterParameterGroup` `createClusterSecurityGroup` `createClusterSnapshot` `createClusterSubnetGroup` `createEventSubscription` `createHsmClientCertificate` `createHsmConfiguration` `deleteCluster` `deleteClusterSnapshot` `describeDefaultClusterParameters` `disableSnapshotCopy` `enableSnapshotCopy` `modifyCluster` `modifyClusterSubnetGroup` `modifyEventSubscription` `modifySnapshotCopyRetentionPeriod` `purchaseReservedNodeOffering` `rebootCluster` `restoreFromClusterSnapshot` `revokeClusterSecurityGroupIngress` `revokeSnapshotAccess` `rotateEncryptionKey` | 

## Dropped Client Properties<a name="upgrading-from-v1-dropped-client-properties"></a>

The `.Client` and `.client` properties have been removed from service objects\. If you use the `.Client` property on a service class or a `.client` property on a service object instance, remove these properties from your code\.

The following code used with version 1 of the SDK for JavaScript:

```
var sts = new AWS.STS.Client();
// or
var sts = new AWS.STS();

sts.client.operation(...);
```

Should be changed to the following code\.

```
var sts = new AWS.STS();
sts.operation(...)
```