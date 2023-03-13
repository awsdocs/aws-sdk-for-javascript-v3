--------

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
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/redshift/README.md)\.
+ Create a shared configurations file with your user credentials\. For more information about providing a credentials JSON file, see [Loading credentials in Node\.js from the shared credentials file](loading-node-credentials-shared.md)\.

**Important**  
These examples demonstrate how to import/export client service objects and command using ECMAScript6 \(ES6\)\.  
This requires Node\.js version 13\.x or higher\. To download and install the latest version of Node\.js, see [Node\.js downloads\.](https://nodejs.org/en/download)\.
If you prefer to use CommonJS syntax, see [JavaScript ES6/CommonJS syntax](sdk-example-javascript-syntax.md)

## Creating an Amazon Redshift cluster<a name="redshift-create-cluster"></a>

This example demonstrates how to create an Amazon Redshift cluster using the AWS SDK for JavaScript\. For more information, see [CreateCluster](https://docs.aws.amazon.com/redshift/latest/APIReference/API_CreateCluster)\.

**Important**  
*The cluster that you are about to create is live \(and not running in a sandbox\)\. You incur the standard Amazon Redshift usage fees for the cluster until you delete it\. If you delete the cluster in the same sitting as when you create it, the total charges are minimal\. * 

Create a `libs` directory, and create a Node\.js module with the file name `redshiftClient.js`\. Copy and paste the code below into it, which creates the Amazon Redshift client object\. Replace *REGION* with your AWS Region\.

```
import  { RedshiftClient }  from  "@aws-sdk/client-redshift";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create Redshift service object.
const redshiftClient = new RedshiftClient({ region: REGION });
export { redshiftClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/redshift/src/libs/redshiftClient.js)\.

Create a Node\.js module with the file name `redshift-create-cluster.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Create a parameters object, specifying the node type to be provisioned, and the master sign\-in credentials for the database instance automatically created in the cluster, and finally the cluster type\.

**Note**  
Replace *CLUSTER\_NAME* with the name of the cluster\. For *NODE\_TYPE* specify the node type to be provisioned, such as 'dc2\.large', for example\. *MASTER\_USERNAME* and *MASTER\_USER\_PASSWORD* are the sign\-in credentials of the master user of your DB instance in the cluster\. For *CLUSTER\_TYPE*, enter the type of cluster\. If you specify `single-node`, you do not require the `NumberOfNodes` parameter\. The remaining parameters are optional\. 

```
// Import required AWS SDK clients and commands for Node.js
import { CreateClusterCommand } from "@aws-sdk/client-redshift";
import { redshiftClient } from "./libs/redshiftClient.js";

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

const run = async () => {
  try {
    const data = await redshiftClient.send(new CreateClusterCommand(params));
    console.log(
      "Cluster " + data.Cluster.ClusterIdentifier + " successfully created"
    );
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node redshift-create-cluster.js  
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/redshift/src/redshift-create-cluster.js)\.

## Modifying a Amazon Redshift cluster<a name="redshift-modify-cluster"></a>

This example shows how to modify the master user password of an Amazon Redshift cluster using the AWS SDK for JavaScript\. For more information about what other setting you can modify, see [ModifyCluster](https://docs.aws.amazon.com/redshift/latest/APIReference/API_ModifyCluster.html)\.

Create a `libs` directory, and create a Node\.js module with the file name `redshiftClient.js`\. Copy and paste the code below into it, which creates the Amazon Redshift client object\. Replace *REGION* with your AWS Region\.

```
import  { RedshiftClient }  from  "@aws-sdk/client-redshift";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create Redshift service object.
const redshiftClient = new RedshiftClient({ region: REGION });
export { redshiftClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/redshift/src/libs/redshiftClient.js)\.

Create a Node\.js module with the file name `redshift-modify-cluster.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Specify the AWS Region, the name of the cluster you want to modify, and new master user password\.

**Note**  
Replace *CLUSTER\_NAME* with the name of the cluster, and *MASTER\_USER\_PASSWORD* with the new master user password\. 

```
// Import required AWS SDK clients and commands for Node.js
import { ModifyClusterCommand } from "@aws-sdk/client-redshift";
import { redshiftClient } from "./libs/redshiftClient.js";

// Set the parameters
const params = {
  ClusterIdentifier: "CLUSTER_NAME",
  MasterUserPassword: "NEW_MASTER_USER_PASSWORD",
};

const run = async () => {
  try {
    const data = await redshiftClient.send(new ModifyClusterCommand(params));
    console.log("Success was modified.", data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node redshift-modify-cluster.js 
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/redshift/src/redshift-modify-cluster.js)\.

## Viewing details of a Amazon Redshift cluster<a name="redshift-describe-cluster"></a>

This example shows how to view the details of an Amazon Redshift cluster using the AWS SDK for JavaScript\. For more information about optional, see [DescribeClusters](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DescribeClusters.html)\.

Create a `libs` directory, and create a Node\.js module with the file name `redshiftClient.js`\. Copy and paste the code below into it, which creates the Amazon Redshift client object\. Replace *REGION* with your AWS Region\.

```
import  { RedshiftClient }  from  "@aws-sdk/client-redshift";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create Redshift service object.
const redshiftClient = new RedshiftClient({ region: REGION });
export { redshiftClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/redshift/src/libs/redshiftClient.js)\.

Create a Node\.js module with the file name `redshift-describe-clusters.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Specify the AWS Region, the name of the cluster you want to modify, and new master user password\.

**Note**  
Replace *CLUSTER\_NAME* with the name of the cluster\. 

```
// Import required AWS SDK clients and commands for Node.js
import { DescribeClustersCommand }  from "@aws-sdk/client-redshift";
import { redshiftClient } from "./libs/redshiftClient.js";

const params = {
  ClusterIdentifier: "CLUSTER_NAME",
};

const run = async () => {
  try {
    const data = await redshiftClient.send(new DescribeClustersCommand(params));
    console.log("Success", data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node redshift-describe-clusters.js 
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/redshift/src/redshift-describe-clusters.js)\.

## Delete an Amazon Redshift cluster<a name="redshift-delete-cluster"></a>

This example shows how to view the details of an Amazon Redshift cluster using the AWS SDK for JavaScript\. For more information about what other setting you can modify, see [DeleteCluster](https://docs.aws.amazon.com/redshift/latest/APIReference/API_DeleteCluster.html)\.

Create a `libs` directory, and create a Node\.js module with the file name `redshiftClient.js`\. Copy and paste the code below into it, which creates the Amazon Redshift client object\. Replace *REGION* with your AWS Region\.

```
import  { RedshiftClient }  from  "@aws-sdk/client-redshift";
// Set the AWS Region.
const REGION = "REGION"; //e.g. "us-east-1"
// Create Redshift service object.
const redshiftClient = new RedshiftClient({ region: REGION });
export { redshiftClient };
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/redshift/src/libs/redshiftClient.js)\.

Create a Node\.js module with the file named `redshift-delete-clusters.js`\. Make sure to configure the SDK as previously shown, including installing the required clients and packages\. Specify the AWS Region, the name of the cluster you want to modify, and new master user password\. The specify if you want to save a final snapshot of the cluster before deleting, and if so the ID of the snapshot\.

**Note**  
Replace *CLUSTER\_NAME* with the name of the cluster\. For the *SkipFinalClusterSnapshot*, specify whether to create a final snapshot of the cluster before deleting it\. If you specify 'false', specify the id of the final cluster snapshot in *CLUSTER\_SNAPSHOT\_ID*\. You can get this ID by clicking the link in the **Snapshots** column for the cluster on the **Clusters** dashboard, and scrolling down to the **Snapshots** pane\. Note that the stem `rs:` is not part of the snapshot ID\.

```
// Import required AWS SDK clients and commands for Node.js
import { DeleteClusterCommand } from "@aws-sdk/client-redshift";
import { redshiftClient } from "./libs/redshiftClient.js";

const params = {
  ClusterIdentifier: "CLUSTER_NAME",
  SkipFinalClusterSnapshot: false,
  FinalClusterSnapshotIdentifier: "CLUSTER_SNAPSHOT_ID",
};

const run = async () => {
  try {
    const data = await redshiftClient.send(new DeleteClusterCommand(params));
    console.log("Success, cluster deleted. ", data);
    return data; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```

To run the example, enter the following at the command prompt\.

```
node redshift-delete-cluster.js  
```

This sample code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/redshift/src/redshift-delete-cluster.js)\.