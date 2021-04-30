--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Create a front\-end page for the app<a name="cross-service-submitdata-front-end"></a>

This topic is part of a larger tutorial about using the AWS SDK for JavaScript with AWS Lambda functions\. To start at the beginning of the tutorial, see [Creating and using Lambda functions](https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/sdk-cross-service-example-submitting-data.html)\. 

In this task, you create the front\-end browser page for the app, for which you require two HTML pages, `index.html` and `error.html`\. `index.html` is the landing page, and `error.html` displays if an error occurs\.

In `DynamoDBApp`, create a file named `index.html`\. The `script` element adds the `main.js` file, which contains all the required JavaScript for the example\. You create `main.js` later in this tutorial\. The remaining code in `index.html` creates the browser page that captures the data that users input\.

```
<!DOCTYPE html>
<html>
<head>
    <script type="text/javascript" src="./main.ts"></script>
</head>
<body>
<h1>Submit an item to an Amazon DynamoDB Table</h1>
<p>Enter a value for each attribute and choose Submit </p>
<table style="width:100%">
    <tr>
        <td>ID:</td>
        <td><input type="text" id="id" name="id"/></td>
    </tr>
    <tr>
        <td>Title:</td>
        <td><input type="text" id="title" name="title"/></td>
    </tr>
    <tr>
        <td>Name:</td>
        <td><input type="text" id="name" name="name"/></td>
    </tr>
    <tr>
        <td>Body:</td>
        <td><input type="text" id="body" name="body"/></td>
    </tr>
        <td></td>
        <td><button type="button" onclick="submitData();">Submit</button></td>
        <script src="./main.js"></script>
    </tr>
</table>
</body>
</html>
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/submit-data-app/src/dynamoApp/index.html)\.

Create a file named `error.html`\.

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8" />
    <title>title</title>
  </head>
  <body>
    An error occurred. Please re-check your code.
  </body>
</html>
```

This example code can be found [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/submit-data-app/src/dynamoApp/error.html)\.