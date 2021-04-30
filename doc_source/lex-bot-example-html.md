--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Create the HTML<a name="lex-bot-example-html"></a>

This topic is part of a tutorial that create an Amazon Lex chatbot within a web application to engage your web site visitor\. To start at the beginning of the tutorial, see [Building an Amazon Lex chatbot](lex-bot-example.md)\.

Create a file named `index.html`\. Copy and paste the code below in to `index.html`\. This HTML references `main.js`\. This is a bundled version of index\.js, which includes the required AWS SDK for JavaScript modules\. You'll create this file in [Create the HTML](#lex-bot-example-html)\. `index.html` also references `style.css`, which adds the styles\. 

```
<!DOCTYPE html>
<head>
    <title>Amazon Lex - Sample Application (BookTrip)</title>
    <link type="text/css" rel="stylesheet" href="style.css">
</head>

<body>
<h1 id="title">Amazon Lex - BookTrip</h1>
<p id="intro">
    This multiple language chatbot shows you how easy it is to incorporate
    <a href="https://aws.amazon.com/lex/" title="Amazon Lex (product)" target="_new">Amazon Lex</a> into your web apps.  Try it out.
</p>
<div id="conversation"></div>
<input type="text" id="wisdom" size="80" value="" placeholder="J'ai besoin d'une chambre d'hôtel">
<br>
<button onclick="createResponse()">Send Text</button>
<script type="text/javascript" src="./main.js"></script>
</body>
```

This code is also available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/resources/cdk#running-a-cdk-app)\.