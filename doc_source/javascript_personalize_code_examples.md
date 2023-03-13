--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Amazon Personalize examples using SDK for JavaScript \(v3\)<a name="javascript_personalize_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for JavaScript \(v3\) with Amazon Personalize\.

*Actions* are code excerpts that show you how to call individual service functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple functions within the same service\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#actions)

## Actions<a name="actions"></a>

### Create a batch interface job<a name="personalize_createBatchInferenceJob_javascript_topic"></a>

The following code example shows how to create a Amazon Personalize batch interface job\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/personalize#code-examples)\. 
  

```
// Get service clients module and commands using ES6 syntax.
import { CreateBatchInferenceJobCommand } from
  "@aws-sdk/client-personalize";
import { personalizeClient } from "./libs/personalizeClients.js";

// Or, create the client here.
// const personalizeClient = new PersonalizeClient({ region: "REGION"});

// Set the batch inference job's parameters.

export const createBatchInferenceJobParam = {
  jobName: 'JOB_NAME',
  jobInput: {         /* required */
    s3DataSource: {   /* required */
      path: 'INPUT_PATH', /* required */
      // kmsKeyArn: 'INPUT_KMS_KEY_ARN' /* optional */'
    }
  },
  jobOutput: {         /* required */
    s3DataDestination: {   /* required */
      path: 'OUTPUT_PATH', /* required */
      // kmsKeyArn: 'OUTPUT_KMS_KEY_ARN' /* optional */'
    }
  },
  roleArn: 'ROLE_ARN', /* required */
  solutionVersionArn: 'SOLUTION_VERSION_ARN', /* required */
  numResults: 20 /* optional integer*/
};

export const run = async () => {
  try {
    const response = await personalizeClient.send(new CreateBatchInferenceJobCommand(createBatchInferenceJobParam));
    console.log("Success", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  For API details, see [CreateBatchInferenceJob](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-personalize/classes/createbatchinferencejobcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create a batch segment job<a name="personalize_createBatchSegmentJob_javascript_topic"></a>

The following code example shows how to create a Amazon Personalize batch segment job\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/personalize#code-examples)\. 
  

```
// Get service clients module and commands using ES6 syntax.
import { CreateBatchSegmentJobCommand } from
  "@aws-sdk/client-personalize";
import { personalizeClient } from "./libs/personalizeClients.js";

// Or, create the client here.
// const personalizeClient = new PersonalizeClient({ region: "REGION"});

// Set the batch segment job's parameters.

export const createBatchSegmentJobParam = {
  jobName: 'NAME',
  jobInput: {         /* required */
    s3DataSource: {   /* required */
      path: 'INPUT_PATH', /* required */
      // kmsKeyArn: 'INPUT_KMS_KEY_ARN' /* optional */'
    }
  },
  jobOutput: {         /* required */
    s3DataDestination: {   /* required */
      path: 'OUTPUT_PATH', /* required */
      // kmsKeyArn: 'OUTPUT_KMS_KEY_ARN' /* optional */'
    }
  },
  roleArn: 'ROLE_ARN', /* required */
  solutionVersionArn: 'SOLUTION_VERSION_ARN', /* required */
  numResults: 20 /* optional */
};

export const run = async () => {
  try {
    const response = await personalizeClient.send(new CreateBatchSegmentJobCommand(createBatchSegmentJobParam));
    console.log("Success", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  For API details, see [CreateBatchSegmentJob](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-personalize/classes/createbatchsegmentjobcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create a campaign<a name="personalize_createCampaign_javascript_topic"></a>

The following code example shows how to create a Amazon Personalize campaign\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/personalize#code-examples)\. 
  

```
// Get service clients module and commands using ES6 syntax.

import { CreateCampaignCommand } from
  "@aws-sdk/client-personalize";
import { personalizeClient } from "./libs/personalizeClients.js";

// Or, create the client here.
// const personalizeClient = new PersonalizeClient({ region: "REGION"});

// Set the campaign's parameters.
export const createCampaignParam = {
  solutionVersionArn: 'SOLUTION_VERSION_ARN', /* required */
  name: 'NAME',  /* required */
  minProvisionedTPS: 1    /* optional integer */
}

export const run = async () => {
  try {
    const response = await personalizeClient.send(new CreateCampaignCommand(createCampaignParam));
    console.log("Success", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  For API details, see [CreateCampaign](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-personalize/classes/createcampaigncommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create a dataset<a name="personalize_createDataset_javascript_topic"></a>

The following code example shows how to create a Amazon Personalize dataset\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/personalize#code-examples)\. 
  

```
// Get service clients module and commands using ES6 syntax.
import { CreateDatasetCommand } from
  "@aws-sdk/client-personalize";
import { personalizeClient } from "./libs/personalizeClients.js";

// Or, create the client here.
// const personalizeClient = new PersonalizeClient({ region: "REGION"});

// Set the dataset's parameters.
export const createDatasetParam = {
  datasetGroupArn: 'DATASET_GROUP_ARN', /* required */
  datasetType: 'DATASET_TYPE', /* required */
  name: 'NAME', /* required */
  schemaArn: 'SCHEMA_ARN' /* required */
}

export const run = async () => {
  try {
    const response = await personalizeClient.send(new CreateDatasetCommand(createDatasetParam));
    console.log("Success", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  For API details, see [CreateDataset](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-personalize/classes/createdatasetcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create a dataset export job<a name="personalize_createDatasetExportJob_javascript_topic"></a>

The following code example shows how to create a Amazon Personalize dataset export job\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/personalize#code-examples)\. 
  

```
// Get service clients module and commands using ES6 syntax.
import { CreateDatasetExportJobCommand } from
  "@aws-sdk/client-personalize";
import { personalizeClient } from "./libs/personalizeClients.js";

// Or, create the client here.
// const personalizeClient = new PersonalizeClient({ region: "REGION"});

// Set the export job parameters.
export const datasetExportJobParam = {
  datasetArn: 'DATASET_ARN', /* required */
  jobOutput: {
    s3DataDestination: {
        path: 'S3_DESTINATION_PATH' /* required */
        //kmsKeyArn: 'ARN'  /* include if your bucket uses AWS KMS for encryption
    } 
  },
  jobName: 'NAME',/* required */
  roleArn: 'ROLE_ARN' /* required */
}

export const run = async () => {
  try {
    const response = await personalizeClient.send(new CreateDatasetExportJobCommand(datasetExportJobParam));
    console.log("Success", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  For API details, see [CreateDatasetExportJob](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-personalize/classes/createdatasetexportjobcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create a dataset group<a name="personalize_createDatasetGroup_javascript_topic"></a>

The following code example shows how to create a Amazon Personalize dataset group\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/personalize#code-examples)\. 
  

```
// Get service clients module and commands using ES6 syntax.

import { CreateDatasetGroupCommand } from
  "@aws-sdk/client-personalize";
import { personalizeClient } from "./libs/personalizeClients.js";

// Or, create the client here.
// const personalizeClient = new PersonalizeClient({ region: "REGION"});

// Set the dataset group parameters.
export const createDatasetGroupParam = { 
  name: 'NAME' /* required */
}

export const run = async (createDatasetGroupParam) => {
  try {
    const response = await personalizeClient.send(new CreateDatasetGroupCommand(createDatasetGroupParam));
    console.log("Success", response);
    return "Run successfully"; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run(createDatasetGroupParam);
```
Create a domain dataset group\.  

```
// Get service clients module and commands using ES6 syntax.
import {  CreateDatasetGroupCommand } from
  "@aws-sdk/client-personalize";
import { personalizeClient } from "./libs/personalizeClients.js";

// Or, create the client here.
// const personalizeClient = new PersonalizeClient({ region: "REGION"});

// Set the domain dataset group parameters.
export const domainDatasetGroupParams = {
  name: 'NAME',  /* required */
  domain: 'DOMAIN'   /* required for a domain dsg, specify ECOMMERCE or VIDEO_ON_DEMAND */
}

export const run = async () => {
  try {
    const response = await personalizeClient.send(new CreateDatasetGroupCommand(domainDatasetGroupParams));
    console.log("Success", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  For API details, see [CreateDatasetGroup](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-personalize/classes/createdatasetgroupcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create a dataset import job<a name="personalize_createDatasetImportJob_javascript_topic"></a>

The following code example shows how to create a Amazon Personalize dataset import job\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/personalize#code-examples)\. 
  

```
// Get service clients module and commands using ES6 syntax.
import {CreateDatasetImportJobCommand } from
  "@aws-sdk/client-personalize";
import { personalizeClient } from "./libs/personalizeClients.js";

// Or, create the client here.
// const personalizeClient = new PersonalizeClient({ region: "REGION"});

// Set the dataset import job parameters.
export const datasetImportJobParam = {
  datasetArn: 'DATASET_ARN', /* required */
  dataSource: {  /* required */
    dataLocation: 'S3_PATH'
  },
  jobName: 'NAME',/* required */
  roleArn: 'ROLE_ARN' /* required */
}

export const run = async () => {
  try {
    const response = await personalizeClient.send(new CreateDatasetImportJobCommand(datasetImportJobParam));
    console.log("Success", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  For API details, see [CreateDatasetImportJob](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-personalize/classes/createdatasetimportjobcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create a domain schema<a name="personalize_createDomainSchema_javascript_topic"></a>

The following code example shows how to create a Amazon Personalize domain schema\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/personalize#code-examples)\. 
  

```
// Get service clients module and commands using ES6 syntax.
import { CreateSchemaCommand } from
  "@aws-sdk/client-personalize";
import { personalizeClient } from "./libs/personalizeClients.js";

// Or, create the client here.
// const personalizeClient = new PersonalizeClient({ region: "REGION"});

import fs from 'fs';

let schemaFilePath = "SCHEMA_PATH";
let mySchema = "";

try {
  mySchema = fs.readFileSync(schemaFilePath).toString();
} catch (err) {
  mySchema = 'TEST' // for unit tests.
}

// Set the domain schema parameters.
export const createDomainSchemaParam = {
  name: 'NAME', /* required */
  schema: mySchema, /* required */
  domain: 'DOMAIN'   /* required for a domain dataset group, specify ECOMMERCE or VIDEO_ON_DEMAND */
};

export const run = async () => {
  try {
    const response = await personalizeClient.send(new CreateSchemaCommand(createDomainSchemaParam));
    console.log("Success", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  For API details, see [CreateSchema](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-personalize/classes/createschemacommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create a filter<a name="personalize_createFilter_javascript_topic"></a>

The following code example shows how to create a Amazon Personalize filter\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/personalize#code-examples)\. 
  

```
// Get service clients module and commands using ES6 syntax.
import { CreateFilterCommand } from
  "@aws-sdk/client-personalize";
import { personalizeClient } from "./libs/personalizeClients.js";
// Or, create the client here.
// const personalizeClient = new PersonalizeClient({ region: "REGION"});

// Set the filter's parameters.
export const createFilterParam = {
  datasetGroupArn: 'DATASET_GROUP_ARN', /* required */
  name: 'NAME', /* required */
  filterExpression: 'FILTER_EXPRESSION' /*required */
}

export const run = async () => {
  try {
    const response = await personalizeClient.send(new CreateFilterCommand(createFilterParam));
    console.log("Success", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  For API details, see [CreateFilter](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-personalize/classes/createfiltercommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create a recommender<a name="personalize_createRecommender_javascript_topic"></a>

The following code example shows how to create a Amazon Personalize recommender\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/personalize#code-examples)\. 
  

```
// Get service clients module and commands using ES6 syntax.
import { CreateRecommenderCommand } from
  "@aws-sdk/client-personalize";
import { personalizeClient } from "./libs/personalizeClients.js";

// Or, create the client here.
// const personalizeClient = new PersonalizeClient({ region: "REGION"});

// Set the recommender's parameters.
export const createRecommenderParam = {
  name: 'NAME', /* required */
  recipeArn: 'RECIPE_ARN', /* required */
  datasetGroupArn: 'DATASET_GROUP_ARN'  /* required */
}

export const run = async () => {
  try {
    const response = await personalizeClient.send(new CreateRecommenderCommand(createRecommenderParam));
    console.log("Success", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  For API details, see [CreateRecommender](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-personalize/classes/createrecommendercommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create a schema<a name="personalize_createSchema_javascript_topic"></a>

The following code example shows how to create a Amazon Personalize schema\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/personalize#code-examples)\. 
  

```
// Get service clients module and commands using ES6 syntax.
import { CreateSchemaCommand } from
  "@aws-sdk/client-personalize";
import { personalizeClient } from "./libs/personalizeClients.js";

// Or, create the client here.
// const personalizeClient = new PersonalizeClient({ region: "REGION"});

import fs from 'fs';

let schemaFilePath = "SCHEMA_PATH";
let mySchema = "";

try {
  mySchema = fs.readFileSync(schemaFilePath).toString();
} catch (err) {
  mySchema = 'TEST' // For unit tests.
}
// Set the schema parameters.
export const createSchemaParam = {
  name: 'NAME', /* required */
  schema: mySchema /* required */
};

export const run = async () => {
  try {
    const response = await personalizeClient.send(new CreateSchemaCommand(createSchemaParam));
    console.log("Success", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  For API details, see [CreateSchema](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-personalize/classes/createschemacommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create a solution<a name="personalize_createSolution_javascript_topic"></a>

The following code example shows how to create a Amazon Personalize solution\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/personalize#code-examples)\. 
  

```
// Get service clients module and commands using ES6 syntax.
import { CreateSolutionCommand } from
  "@aws-sdk/client-personalize";
import { personalizeClient } from "./libs/personalizeClients.js";
// Or, create the client here.
// const personalizeClient = new PersonalizeClient({ region: "REGION"});

// Set the solution parameters.
export const createSolutionParam = {
  datasetGroupArn: 'DATASET_GROUP_ARN', /* required */
  recipeArn: 'RECIPE_ARN', /* required */
  name: 'NAME' /* required */
}

export const run = async () => {
  try {
    const response = await personalizeClient.send(new CreateSolutionCommand(createSolutionParam));
    console.log("Success", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  For API details, see [CreateSolution](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-personalize/classes/createsolutioncommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create a solution version<a name="personalize_createSolutionVersion_javascript_topic"></a>

The following code example shows how to create a Amazon Personalize solution\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/personalize#code-examples)\. 
  

```
// Get service clients module and commands using ES6 syntax.
import { CreateSolutionVersionCommand } from
  "@aws-sdk/client-personalize";
import { personalizeClient } from "./libs/personalizeClients.js";
// Or, create the client here.
// const personalizeClient = new PersonalizeClient({ region: "REGION"});

// Set the solution version parameters.
export const solutionVersionParam = {
  solutionArn: 'SOLUTION_ARN' /* required */
}

export const run = async () => {
  try {
    const response = await personalizeClient.send(new CreateSolutionVersionCommand(solutionVersionParam));
    console.log("Success", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  For API details, see [CreateSolutionVersion](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-personalize/classes/createsolutionversioncommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create an event tracker<a name="personalize_createEventTracker_javascript_topic"></a>

The following code example shows how to create a Amazon Personalize event tracker\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/personalize#code-examples)\. 
  

```
// Get service clients module and commands using ES6 syntax.
import { CreateEventTrackerCommand } from
  "@aws-sdk/client-personalize";
import { personalizeClient } from "./libs/personalizeClients.js";

// Or, create the client here.
// const personalizeClient = new PersonalizeClient({ region: "REGION"});

// Set the event tracker's parameters.
export const createEventTrackerParam = {
  datasetGroupArn: 'DATASET_GROUP_ARN', /* required */
  name: 'NAME', /* required */
}

export const run = async () => {
  try {
    const response = await personalizeClient.send(new CreateEventTrackerCommand(createEventTrackerParam));
    console.log("Success", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  For API details, see [CreateEventTracker](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-personalize/classes/createeventtrackercommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Import real\-time interaction event data<a name="personalize-events_putItems_javascript_topic"></a>

The following code example shows how to import real\-time interaction event data into Amazon Personalize Events\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/personalize#code-examples)\. 
  

```
// Get service clients module and commands using ES6 syntax.
import { PutItemsCommand } from
  "@aws-sdk/client-personalize-events";
import { personalizeEventsClient } from "./libs/personalizeClients.js";
// Or, create the client here.
// const personalizeEventsClient = new PersonalizeEventsClient({ region: "REGION"});

// Set the put items parameters. For string properties and values, use the \ character to escape quotes.
var putItemsParam = {
    datasetArn: 'DATASET_ARN', /* required */
    items: [    /* required */
      {
        'itemId': 'ITEM_ID',  /*  required */
        'properties': "{\"PROPERTY1_NAME\": \"PROPERTY1_VALUE\", \"PROPERTY2_NAME\": \"PROPERTY2_VALUE\", \"PROPERTY3_NAME\": \"PROPERTY3_VALUE\"}"   /* optional */
      }
    ]
};
export const run = async () => {
  try {
    const response = await personalizeEventsClient.send(new PutItemsCommand(putItemsParam));
    console.log("Success!", response);
    return response; // For unit tests.
  } catch (err) {
    console.log("Error", err);
  }
};
run();
```
+  For API details, see [PutItems](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-personalize/classes/putitemscommand.html) in *AWS SDK for JavaScript API Reference*\. 