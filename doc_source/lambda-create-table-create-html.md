--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Create the HTML<a name="lambda-create-table-create-html"></a>

This topic is part of a tutorial that demonstrates how to create, deploy, and run a Lambda function using the AWS SDK for JavaScript\. To start at the beginning of the tutorial, see [Tutorial: Create and using Lambda functions](lambda-create-table-example.md)\.

First, create a `LambdaApp` folder\. In this folder, create an `index.html` file, and copy and paste the content below into it\. Upload the `index.html` file to the Amazon S3 bucket you created in the [Create the AWS resources ](lambda-create-table-provision-resources.md) topic of this tutorial\.

```
<!doctype html>
<html>
<head>
<meta charset="UTF-8">
<title>Card Slots</title>

</head>

<body>
    <button type="button" onclick="createtable()">Create a table!</button>
	<script type="text/javascript" src="main.js"></script>
</body>
</html>
```

This code example is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/lambda/lambda_create_function/src/LambdaApp/index.ts)\.