--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Amazon Redshift examples<a name="redshift-examples-section"></a>

In this example, a series of Node\.js modules are used to create, modify, describe the parameters of, and then delete Amazon Redshift clusters using the following methods of the `Redshift` client class:
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-redshift/classes/createclustercommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-redshift/classes/createclustercommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-redshift/classes/modifyclustercommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-redshift/classes/modifyclustercommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-redshift/classes/describeclusterscommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-redshift/classes/describeclusterscommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-redshift/classes/deleteclustercommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-redshift/classes/deleteclustercommand.html)

For more information about Amazon Redshift users, see the [Amazon Redshift getting started guide](https://docs.aws.amazon.com/redshift/latest/gsg/getting-started.html)\.

## Prerequisite tasks<a name="s3-example-configuring-buckets-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/redshift/README.md)\.
**Note**  
The AWS SDK for JavaScript \(V3\) is written in TypeScript, so for consistency these examples are presented in TypeScript\. TypeScript extends JavaScript, so these examples can also be run in JavaScript\. For more information, see [this article](https://aws.amazon.com/blogs/developer/first-class-typescript-support-in-modular-aws-sdk-for-javascript/) in the AWS Developer Blog\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

## Creating an Amazon Redshift cluster<a name="redshift-create-cluster"></a>

This example demonstrates how to create an Amazon Redshift cluster using the AWS SDK for JavaScript\. For more information, see [CreateCluster](https://docs.aws.amazon.com/redshift/latest/APIReference/API_CreateCluster)\.

**Important**  
*The cluster that you are about to create is live \(and not running in a sandbox\)\. You incur the standard Amazon Redshift usage fees for the cluster until you delete it\. If you delete the cluster in the same sitting as when you create it, the total charges are minimal\. * 

Create a Node\.js module with the file name `redshift-create-cluster.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create an `Redshift` client service object\. Create a parameters object, specifying the node type to be provisioned, and the master username and password for the database instance automatically created in the cluster, and finally the cluster type\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, and *CLUSTER\_NAME* with the name of the cluster\. For *NODE\_TYPE* specify the node type to be provisioned, such as 'dc2\.large', for example\. *MASTER\_USERNAME* and *MASTER\_USER\_PASSWORD* are the master user name and password of the master user of your DB instance in the cluster\. For *CLUSTER\_TYPE*, enter the type of cluster\. If you specify `single-node`, you do not require the `NumberOfNodes` parameter\. The remaining parameters are optional\. 

```
// Import required AWS SDK clients and commands for Node.js
const {
  RedshiftClient,
  CreateClusterCommand,
} = require("@aws-sdk/client-redshift-node");

// Set the AWS Region
const REGION = "REGION";

const params = {
  ClusterIdentifier: "CLUSTER_NAME", // Required
  NodeType: "NODE_TYPE", //Required
  MasterUsername: "MASTER_USER_NAME", // Required - must be lowercase
  MasterUserPassword: "MASTER_USER_PASSWORD", // Required - must contain at least one uppercase letter, and one number
  ClusterType: "CLUSTER_TYPE", // Required
  IAMRoleARN: "IAM_ROLE_ARN", // Optional - the ARN of an IAM role with permissions your cluster needs to access other AWS services on your behalf, such as Amazon S3.
  ClusterSubnetGroupName: "CLUSTER_SUBNET_GROUPNAME", //Optional - the name of a cluster subnet group to be associated with this cluster. Defaults to 'default' if not specified.
  DBName: "DATABASE_NAME", // Optional - defaults to 'dev' if not specified
  Port: "PORT_NUMBER", // Optional - defaults to '5439' if not specified
};

// Create an Amazon Redshift client service object
const redshift = new RedshiftClient({ region: REGION });

const run = async () => {
  try {
    const data = await redshift.send(new CreateClusterCommand(params));
    console.log(
      "Cluster " + data.Cluster.ClusterIdentifier + " successfully created"
    );
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node redshift-create-cluster.ts  // If you prefer JavaScript, enter 'node redshift-create-cluster.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/redshift/src/redshift-create-cluster.ts)\.

## Modifing a Amazon Redshift cluster<a name="redshift-modify-cluster"></a>

This example shows how to modify the master user password of an Amazon Redshift cluster using the AWS SDK for JavaScript\. For more information about what other setting you can modify, see [ModifyCluster](https://docs.aws.amazon.com/redshift/latest/APIReference/API_ModifyCluster.html)\.

Create a Node\.js module with the file name `redshift-modify-cluster.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create a `Redshift` client service object\. Specify the AWS Region, the name of the cluster you want to modify, and new master user password\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *CLUSTER\_NAME* with the name of the cluster, and *MASTER\_USER\_PASSWORD* with the new master user password\. 

```
// Import required AWS SDK clients and commands for Node.js
const {
  RedshiftClient,
  ModifyClusterCommand
} = require("@aws-sdk/client-redshift-node");

// Set the AWS Region
const REGION = "REGION";

// Set the parameters
const params = {
  ClusterIdentifier: "CLUSTER_NAME",
  MasterUserPassword: "NEW_MASTER_USER_PASSWORD",
};

// Create an Amazon Redshift client service object
const redshift = new RedshiftClient({ region: REGION });

const run = async () => {
  try {
    const data = await redshift.send(new ModifyClusterCommand(params));
    console.log(data.Cluster.ClusterIdentifier + " was modified.");
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node redshift-modify-cluster.ts  // If you prefer JavaScript, enter 'node redshift-modify-cluster.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/redshift/src/redshift-modify-cluster.ts)\.

## Viewing details of a Amazon Redshift cluster<a name="redshift-describe-cluster"></a>

This example shows how to view the details of an Amazon Redshift cluster using the AWS SDK for JavaScript\. For more information about optional, see [DescribeClusters](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeClusters.html)\.

Create a Node\.js module with the file name `redshift-descibe-clusters.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create a `Redshift` client service object\. Specify the AWS Region, the name of the cluster you want to modify, and new master user password\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *CLUSTER\_NAME* with the name of the cluster\. 

```
// Import required AWS SDK clients and commands for Node.js
const {
  RedshiftClient,
  DescribeClustersCommand
} = require("@aws-sdk/client-redshift-node");

// Set the AWS Region
const REGION = "REGION";

const params = {
  ClusterIdentifier: "CLUSTER_NAME",
};

// Create an Amazon Redshift client service object
const redshift = new RedshiftClient({ region: REGION });

const run = async () => {
  try {
    const data = await redshift.send(new DescribeClustersCommand(params));
    console.log(data);
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node redshift-describe-clusters.ts  // If you prefer JavaScript, enter 'node redshift-modify-cluster.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/redshift/src/redshift-describe-clusters.ts)\.

## Delete an Amazon Redshift cluster<a name="redshift-delete-cluster"></a>

This example shows how to view the details of an Amazon Redshift cluster using the AWS SDK for JavaScript\. For more information about what other setting you can modify, see [DeleteCluster](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DeleteCluster.html)\.

Create a Node\.js module with the file named `redshift-delete-clusters.ts`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create a `Redshift` client service object\. Specify the AWS Region, the name of the cluster you want to modify, and new master user password\. The specify if you want to save a final snapshot of the cluster before deleting, and if so the ID of the snapshot\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

**Note**  
Replace *REGION* with your AWS Region, *CLUSTER\_NAME* with the name of the cluster\. For the *SkipFinalClusterSnapshot*, specify whether to create a final snapshot of the cluster before deleting it\. If you specify 'false', specify the id of the final cluster snapshot in *CLUSTER\_SNAPSHOT\_ID*\. You can get this ID by clicking the link in the **Snapshots** column for the cluster on the **Clusters** dashboard, and scrolling down to the **Snapshots** pane\. Note that the stem `rs:` is not part of the snapshot ID\.

```
// Import required AWS SDK clients and commands for Node.js
const {
  RedshiftClient,
  DeleteClusterCommand,
} = require("@aws-sdk/client-redshift-node");

// Set the AWS Region
const REGION = "REGION";

const params = {
  ClusterIdentifier: "CLUSTER_NAME",
  SkipFinalClusterSnapshot: false,
  FinalClusterSnapshotIdentifier: "CLUSTER_SNAPSHOT_ID",
};

// Create an Amazon Redshift client service object
const redshift = new RedshiftClient({ region: REGION });

const run = async () => {
  try {
    const data = await redshift.send(new DeleteClusterCommand(params));
    console.log("Cluster deleted");
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
ts-node redshift-delete-cluster.ts  // If you prefer JavaScript, enter 'node redshift-create-cluster.js'
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/redshift/src/redshift-delete-cluster.ts)\.