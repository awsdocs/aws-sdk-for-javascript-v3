--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Delete the resources<a name="lambda-create-table-destroy"></a>

This topic is part of a tutorial that demonstrates how to create, deploy, and run a Lambda function using the AWS SDK for JavaScript\. To start at the beginning of the tutorial, see [Tutorial: Create and using Lambda functions](lambda-create-table-example.md)\.

When you finish the tutorial, you should delete the resources so you do not incur any unnecessary charges\. You can do this by deleting the AWS CloudFormation stack you created in the [Create the AWS resources ](lambda-create-table-provision-resources.md) topic of this tutorial\.

Because you created the DynamoDB table, you must delete it manually\. For more information, see tsee the `ddb_deletetable.ts` code sample [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/dynamodb/src/ddb_deletetable.ts)\. Then you can delete the remaining resources using either the [AWS Management Console](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-console-create-stack.html) or the [AWS Command Line](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-cli-creating-stack.html)\. Instructions on how to modify the stack, or to delete the stack and its associated resources when you have finished the tutorial, see [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/cross-services/transcription-app)\.