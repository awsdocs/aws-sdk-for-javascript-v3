--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# AWS Glue examples using SDK for JavaScript \(v3\)<a name="javascript_glue_code_examples"></a>

The following code examples show you how to perform actions and implement common scenarios by using the AWS SDK for JavaScript \(v3\) with AWS Glue\.

*Actions* are code excerpts that show you how to call individual service functions\.

*Scenarios* are code examples that show you how to accomplish a specific task by calling multiple functions within the same service\.

Each example includes a link to GitHub, where you can find instructions on how to set up and run the code in context\.

**Topics**
+ [Actions](#actions)
+ [Scenarios](#scenarios)

## Actions<a name="actions"></a>

### Create a crawler<a name="glue_CreateCrawler_javascript_topic"></a>

The following code example shows how to create an AWS Glue crawler\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glue#code-examples)\. 
  

```
const createCrawler = (name, role, dbName, tablePrefix, s3TargetPath) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new CreateCrawlerCommand({
    Name: name,
    Role: role,
    DatabaseName: dbName,
    TablePrefix: tablePrefix,
    Targets: {
      S3Targets: [{ Path: s3TargetPath }],
    },
  });

  return client.send(command);
};
```
+  For API details, see [CreateCrawler](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/createcrawlercommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Create a job definition<a name="glue_CreateJob_javascript_topic"></a>

The following code example shows how to create an AWS Glue job definition\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glue#code-examples)\. 
  

```
const createJob = (name, role, scriptBucketName, scriptKey) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new CreateJobCommand({
    Name: name,
    Role: role,
    Command: {
      Name: "glueetl",
      PythonVersion: "3",
      ScriptLocation: `s3://${scriptBucketName}/${scriptKey}`,
    },
    GlueVersion: "3.0",
  });

  return client.send(command);
};
```
+  For API details, see [CreateJob](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/createjobcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Delete a crawler<a name="glue_DeleteCrawler_javascript_topic"></a>

The following code example shows how to delete an AWS Glue crawler\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glue#code-examples)\. 
  

```
const deleteCrawler = (crawlerName) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new DeleteCrawlerCommand({
    Name: crawlerName,
  });

  return client.send(command);
};
```
+  For API details, see [DeleteCrawler](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/deletecrawlercommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Delete a database from the Data Catalog<a name="glue_DeleteDatabase_javascript_topic"></a>

The following code example shows how to delete a database from the AWS Glue Data Catalog\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glue#code-examples)\. 
  

```
const deleteDatabase = (databaseName) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new DeleteDatabaseCommand({
    Name: databaseName,
  });

  return client.send(command);
};
```
+  For API details, see [DeleteDatabase](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/deletedatabasecommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Delete a job definition<a name="glue_DeleteJob_javascript_topic"></a>

The following code example shows how to delete an AWS Glue job definition and all associated runs\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glue#code-examples)\. 
  

```
const deleteJob = (jobName) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new DeleteJobCommand({
    JobName: jobName,
  });

  return client.send(command);
};
```
+  For API details, see [DeleteJob](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/deletejobcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Delete a table from a database<a name="glue_DeleteTable_javascript_topic"></a>

The following code example shows how to delete a table from an AWS Glue Data Catalog database\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glue#code-examples)\. 
  

```
const deleteTable = (databaseName, tableName) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new DeleteTableCommand({
    DatabaseName: databaseName,
    Name: tableName,
  });

  return client.send(command);
};
```
+  For API details, see [DeleteTable](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/deletetablecommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Get a crawler<a name="glue_GetCrawler_javascript_topic"></a>

The following code example shows how to get an AWS Glue crawler\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glue#code-examples)\. 
  

```
const getCrawler = (name) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new GetCrawlerCommand({
    Name: name,
  });

  return client.send(command);
};
```
+  For API details, see [GetCrawler](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/getcrawlercommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Get a database from the Data Catalog<a name="glue_GetDatabase_javascript_topic"></a>

The following code example shows how to get a database from the AWS Glue Data Catalog\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glue#code-examples)\. 
  

```
const getDatabase = (name) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new GetDatabaseCommand({
    Name: name,
  });

  return client.send(command);
};
```
+  For API details, see [GetDatabase](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/getdatabasecommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Get a job run<a name="glue_GetJobRun_javascript_topic"></a>

The following code example shows how to get an AWS Glue job run\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glue#code-examples)\. 
  

```
const getJobRun = (jobName, jobRunId) => {
  const client = new GlueClient({ region: DEFAULT_REGION });
  const command = new GetJobRunCommand({
    JobName: jobName,
    RunId: jobRunId,
  });

  return client.send(command);
};
```
+  For API details, see [GetJobRun](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/getjobruncommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Get databases from the Data Catalog<a name="glue_GetDatabases_javascript_topic"></a>

The following code example shows how to get a list of databases from the AWS Glue Data Catalog\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glue#code-examples)\. 
  

```
const getDatabases = () => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new GetDatabasesCommand({});

  return client.send(command);
};
```
+  For API details, see [GetDatabases](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/getdatabasescommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Get job from the Data Catalog<a name="glue_GetJob_javascript_topic"></a>

The following code example shows how to get a job from the AWS Glue Data Catalog\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glue#code-examples)\. 
  

```
const getJob = (jobName) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new GetJobCommand({
    JobName: jobName,
  });

  return client.send(command);
};
```
+  For API details, see [GetJob](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/getjobcommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Get runs of a job<a name="glue_GetJobRuns_javascript_topic"></a>

The following code example shows how to get runs of an AWS Glue job\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glue#code-examples)\. 
  

```
const getJobRuns = (jobName) => {
  const client = new GlueClient({ region: DEFAULT_REGION });
  const command = new GetJobRunsCommand({
    JobName: jobName,
  });

  return client.send(command);
};
```
+  For API details, see [GetJobRuns](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/getjobrunscommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Get tables from a database<a name="glue_GetTables_javascript_topic"></a>

The following code example shows how to get tables from a database in the AWS Glue Data Catalog\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glue#code-examples)\. 
  

```
const getTables = (databaseName) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new GetTablesCommand({
    DatabaseName: databaseName,
  });

  return client.send(command);
};
```
+  For API details, see [GetTables](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/gettablescommand.html) in *AWS SDK for JavaScript API Reference*\. 

### List job definitions<a name="glue_ListJobs_javascript_topic"></a>

The following code example shows how to list AWS Glue job definitions\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glue#code-examples)\. 
  

```
const listJobs = () => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new ListJobsCommand({});

  return client.send(command);
};
```
+  For API details, see [ListJobs](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/listjobscommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Start a crawler<a name="glue_StartCrawler_javascript_topic"></a>

The following code example shows how to start an AWS Glue crawler\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glue#code-examples)\. 
  

```
const startCrawler = (name) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new StartCrawlerCommand({
    Name: name,
  });

  return client.send(command);
};
```
+  For API details, see [StartCrawler](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/startcrawlercommand.html) in *AWS SDK for JavaScript API Reference*\. 

### Start a job run<a name="glue_StartJobRun_javascript_topic"></a>

The following code example shows how to start an AWS Glue job run\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glue#code-examples)\. 
  

```
const startJobRun = (jobName, dbName, tableName, bucketName) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new StartJobRunCommand({
    JobName: jobName,
    Arguments: {
      "--input_database": dbName,
      "--input_table": tableName,
      "--output_bucket_url": `s3://${bucketName}/`
    }
  });

  return client.send(command);
};
```
+  For API details, see [StartJobRun](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/startjobruncommand.html) in *AWS SDK for JavaScript API Reference*\. 

## Scenarios<a name="scenarios"></a>

### Get started running crawlers and jobs<a name="glue_Scenario_GetStartedCrawlersJobs_javascript_topic"></a>

The following code example shows how to:
+ Create and run a crawler that crawls a public Amazon Simple Storage Service \(Amazon S3\) bucket and generates a metadata database that describes the CSV\-formatted data it finds\.
+ List information about databases and tables in your AWS Glue Data Catalog\.
+ Create and run a job that extracts CSV data from the source Amazon S3 bucket, transforms it by removing and renaming fields, and loads JSON\-formatted output into another Amazon S3 bucket\.
+ List information about job runs and view some of the transformed data\.
+ Delete all resources created by the demo\.

For more information, see [Tutorial: Getting started with AWS Glue Studio](https://docs.aws.amazon.com/glue/latest/ug/tutorial-create-job.html)\.

**SDK for JavaScript \(v3\)**  
 There's more on GitHub\. Find the complete example and learn how to set up and run in the [AWS Code Examples Repository](https://github.com/awsdocs/aws-doc-sdk-examples/tree/main/javascriptv3/example_code/glue#code-examples)\. 
Create and run a crawler that crawls a public Amazon Simple Storage Service \(Amazon S3\) bucket and generates a metadata database that describes the CSV\-formatted data it finds\.  

```
const createCrawler = (name, role, dbName, tablePrefix, s3TargetPath) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new CreateCrawlerCommand({
    Name: name,
    Role: role,
    DatabaseName: dbName,
    TablePrefix: tablePrefix,
    Targets: {
      S3Targets: [{ Path: s3TargetPath }],
    },
  });

  return client.send(command);
};

const getCrawler = (name) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new GetCrawlerCommand({
    Name: name,
  });

  return client.send(command);
};

const startCrawler = (name) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new StartCrawlerCommand({
    Name: name,
  });

  return client.send(command);
};

const crawlerExists = async ({ getCrawler }, crawlerName) => {
  try {
    await getCrawler(crawlerName);
    return true;
  } catch {
    return false;
  }
};

const makeCreateCrawlerStep = (actions) => async (context) => {
  if (await crawlerExists(actions, process.env.CRAWLER_NAME)) {
    log("Crawler already exists. Skipping creation.");
  } else {
    await actions.createCrawler(
      process.env.CRAWLER_NAME,
      process.env.ROLE_NAME,
      process.env.DATABASE_NAME,
      process.env.TABLE_PREFIX,
      process.env.S3_TARGET_PATH
    );

    log("Crawler created successfully.", { type: "success" });
  }

  return { ...context };
};

const waitForCrawler = async (getCrawler, crawlerName) => {
  const waitTimeInSeconds = 30;
  const { Crawler } = await getCrawler(crawlerName);

  if (!Crawler) {
    throw new Error(`Crawler with name ${crawlerName} not found.`);
  }

  if (Crawler.State === "READY") {
    return;
  }

  log(`Crawler is ${Crawler.State}. Waiting ${waitTimeInSeconds} seconds...`);
  await wait(waitTimeInSeconds);
  return waitForCrawler(getCrawler, crawlerName);
};

const makeStartCrawlerStep =
  ({ startCrawler, getCrawler }) =>
  async (context) => {
    log("Starting crawler.");
    await startCrawler(process.env.CRAWLER_NAME);
    log("Crawler started.", { type: "success" });

    log("Waiting for crawler to finish running. This can take a while.");
    await waitForCrawler(getCrawler, process.env.CRAWLER_NAME);
    log("Crawler ready.", { type: "success" });

    return { ...context };
  };
```
List information about databases and tables in your AWS Glue Data Catalog\.  

```
const getDatabase = (name) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new GetDatabaseCommand({
    Name: name,
  });

  return client.send(command);
};

const getTables = (databaseName) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new GetTablesCommand({
    DatabaseName: databaseName,
  });

  return client.send(command);
};

const makeGetDatabaseStep =
  ({ getDatabase }) =>
  async (context) => {
    const {
      Database: { Name },
    } = await getDatabase(process.env.DATABASE_NAME);
    log(`Database: ${Name}`);
    return { ...context };
  };

const makeGetTablesStep =
  ({ getTables }) =>
  async (context) => {
    const { TableList } = await getTables(process.env.DATABASE_NAME);
    log("Tables:");
    log(TableList.map((table) => `  • ${table.Name}\n`));
    return { ...context };
  };
```
Create and run a job that extracts CSV data from the source Amazon S3 bucket, transforms it by removing and renaming fields, and loads JSON\-formatted output into another Amazon S3 bucket\.  

```
const createJob = (name, role, scriptBucketName, scriptKey) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new CreateJobCommand({
    Name: name,
    Role: role,
    Command: {
      Name: "glueetl",
      PythonVersion: "3",
      ScriptLocation: `s3://${scriptBucketName}/${scriptKey}`,
    },
    GlueVersion: "3.0",
  });

  return client.send(command);
};

const startJobRun = (jobName, dbName, tableName, bucketName) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new StartJobRunCommand({
    JobName: jobName,
    Arguments: {
      "--input_database": dbName,
      "--input_table": tableName,
      "--output_bucket_url": `s3://${bucketName}/`
    }
  });

  return client.send(command);
};

const makeCreateJobStep =
  ({ createJob }) =>
  async (context) => {
    log("Creating Job.");
    await createJob(
      process.env.JOB_NAME,
      process.env.ROLE_NAME,
      process.env.BUCKET_NAME,
      process.env.PYTHON_SCRIPT_KEY
    );
    log("Job created.", { type: "success" });

    return { ...context };
  };

const waitForJobRun = async (getJobRun, jobName, jobRunId) => {
  const waitTimeInSeconds = 30;
  const { JobRun } = await getJobRun(jobName, jobRunId);

  if (!JobRun) {
    throw new Error(`Job run with id ${jobRunId} not found.`);
  }

  switch (JobRun.JobRunState) {
    case "FAILED":
    case "STOPPED":
    case "TIMEOUT":
    case "STOPPED":
      throw new Error(
        `Job ${JobRun.JobRunState}. Error: ${JobRun.ErrorMessage}`
      );
    case "RUNNING":
      break;
    case "SUCCEEDED":
      return;
    default:
      throw new Error(`Unknown job run state: ${JobRun.JobRunState}`);
  }

  log(
    `Job ${JobRun.JobRunState}. Waiting ${waitTimeInSeconds} more seconds...`
  );
  await wait(waitTimeInSeconds);
  return waitForJobRun(getJobRun, jobName, jobRunId);
};

const promptToOpen = async (context) => {
  const { shouldOpen } = await context.prompter.prompt({
    name: "shouldOpen",
    type: "confirm",
    message: "Open the output bucket in your browser?",
  });

  if (shouldOpen) {
    return open(
      `https://s3.console.aws.amazon.com/s3/buckets/${process.env.BUCKET_NAME}?region=${DEFAULT_REGION}&tab=objects to view the output.`
    );
  }
};

const makeStartJobRunStep =
  ({ startJobRun, getJobRun }) =>
  async (context) => {
    log("Starting job.");
    const { JobRunId } = await startJobRun(
      process.env.JOB_NAME,
      process.env.DATABASE_NAME,
      process.env.TABLE_NAME,
      process.env.BUCKET_NAME
    );
    log("Job started.", { type: "success" });

    log("Waiting for job to finish running. This can take a while.");
    await waitForJobRun(getJobRun, process.env.JOB_NAME, JobRunId);
    log("Job run succeeded.", { type: "success" });

    await promptToOpen(context);

    return { ...context };
  };
```
List information about job runs and view some of the transformed data\.  

```
const getJobRuns = (jobName) => {
  const client = new GlueClient({ region: DEFAULT_REGION });
  const command = new GetJobRunsCommand({
    JobName: jobName,
  });

  return client.send(command);
};

const getJobRun = (jobName, jobRunId) => {
  const client = new GlueClient({ region: DEFAULT_REGION });
  const command = new GetJobRunCommand({
    JobName: jobName,
    RunId: jobRunId,
  });

  return client.send(command);
};

const logJobRunDetails = async (getJobRun, jobName, jobRunId) => {
  const { JobRun } = await getJobRun(jobName, jobRunId);
  log(JobRun, { type: "object" });
};

const makePickJobRunStep =
  ({ getJobRuns, getJobRun }) =>
  async (context) => {
    if (context.selectedJobName) {
      const { JobRuns } = await getJobRuns(context.selectedJobName);

      const { jobRunId } = await context.prompter.prompt({
        name: "jobRunId",
        type: "list",
        message: "Select a job run to see details.",
        choices: JobRuns.map((run) => run.Id),
      });

      logJobRunDetails(getJobRun, context.selectedJobName, jobRunId);
    }

    return { ...context };
  };
```
Delete all resources created by the demo\.  

```
const deleteJob = (jobName) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new DeleteJobCommand({
    JobName: jobName,
  });

  return client.send(command);
};

const deleteTable = (databaseName, tableName) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new DeleteTableCommand({
    DatabaseName: databaseName,
    Name: tableName,
  });

  return client.send(command);
};

const deleteDatabase = (databaseName) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new DeleteDatabaseCommand({
    Name: databaseName,
  });

  return client.send(command);
};

const deleteCrawler = (crawlerName) => {
  const client = new GlueClient({ region: DEFAULT_REGION });

  const command = new DeleteCrawlerCommand({
    Name: crawlerName,
  });

  return client.send(command);
};

const handleDeleteJobs = async (deleteJobFn, jobNames, context) => {
  const { selectedJobNames } = await context.prompter.prompt({
    name: "selectedJobNames",
    type: "checkbox",
    message: "Let's clean up jobs. Select jobs to delete.",
    choices: jobNames,
  });

  if (selectedJobNames.length === 0) {
    log("No jobs selected.");
  } else {
    log("Deleting jobs.");
    await Promise.all(
      selectedJobNames.map((n) => deleteJobFn(n).catch(console.error))
    );
    log("Jobs deleted.", { type: "success" });
  }
};

const makeCleanUpJobsStep =
  ({ listJobs, deleteJob }) =>
  async (context) => {
    const { JobNames } = await listJobs();
    if (JobNames.length > 0) {
      await handleDeleteJobs(deleteJob, JobNames, context);
    }

    return { ...context };
  };

const deleteTables = (deleteTable, databaseName, tableNames) =>
  Promise.all(
    tableNames.map((tableName) =>
      deleteTable(databaseName, tableName).catch(console.error)
    )
  );

const makeCleanUpTablesStep =
  ({ getTables, deleteTable }) =>
  async (context) => {
    const { TableList } = await getTables(process.env.DATABASE_NAME).catch(
      () => ({ TableList: null })
    );

    if (TableList && TableList.length > 0) {
      const { tableNames } = await context.prompter.prompt({
        name: "tableNames",
        type: "checkbox",
        message: "Let's clean up tables. Select tables to delete.",
        choices: TableList.map((t) => t.Name),
      });

      if (tableNames.length === 0) {
        log("No tables selected.");
      } else {
        log("Deleting tables.");
        await deleteTables(deleteTable, process.env.DATABASE_NAME, tableNames);
        log("Tables deleted.", { type: "success" });
      }
    }

    return { ...context };
  };

const deleteDatabases = (deleteDatabase, databaseNames) =>
  Promise.all(
    databaseNames.map((dbName) => deleteDatabase(dbName).catch(console.error))
  );

const makeCleanUpDatabasesStep =
  ({ getDatabases, deleteDatabase }) =>
  async (context) => {
    const { DatabaseList } = await getDatabases();

    if (DatabaseList.length > 0) {
      const { dbNames } = await context.prompter.prompt({
        name: "dbNames",
        type: "checkbox",
        message: "Let's clean up databases. Select databases to delete.",
        choices: DatabaseList.map((db) => db.Name),
      });

      if (dbNames.length === 0) {
        log("No databases selected.");
      } else {
        log("Deleting databases.");
        await deleteDatabases(deleteDatabase, dbNames);
        log("Databases deleted.", { type: "success" });
      }
    }

    return { ...context };
  };

const cleanUpCrawlerStep = async (context) => {
  log(`Deleting crawler.`);

  try {
    await deleteCrawler(process.env.CRAWLER_NAME);
    log("Crawler deleted.", { type: "success" });
  } catch (err) {
    if (err.name === "EntityNotFoundException") {
      log(`Crawler is already deleted.`);
    } else {
      throw err;
    }
  }

  return { ...context };
};
```
+ For API details, see the following topics in *AWS SDK for JavaScript API Reference*\.
  + [CreateCrawler](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/createcrawlercommand.html)
  + [CreateJob](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/createjobcommand.html)
  + [DeleteCrawler](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/deletecrawlercommand.html)
  + [DeleteDatabase](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/deletedatabasecommand.html)
  + [DeleteJob](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/deletejobcommand.html)
  + [DeleteTable](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/deletetablecommand.html)
  + [GetCrawler](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/getcrawlercommand.html)
  + [GetDatabase](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/getdatabasecommand.html)
  + [GetDatabases](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/getdatabasescommand.html)
  + [GetJob](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/getjobcommand.html)
  + [GetJobRun](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/getjobruncommand.html)
  + [GetJobRuns](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/getjobrunscommand.html)
  + [GetTables](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/gettablescommand.html)
  + [ListJobs](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/listjobscommand.html)
  + [StartCrawler](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/startcrawlercommand.html)
  + [StartJobRun](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-glue/classes/startjobruncommand.html)