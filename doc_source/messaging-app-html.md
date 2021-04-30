--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Create the HTML page<a name="messaging-app-html"></a>

This topic is part of a tutorial that create an AWS application that sends and retrieves messages by using the AWS SDK for JavaScript and Amazon Simple Queue Service \(Amazon SQS\)\. To start at the beginning of the tutorial, see [Creating an example messaging application](messaging-app.md)\.

 Now you create the HTML files that are required for the application’s graphical user interface \(GUI\)\. Create a file named `index.html`\. Copy and paste the code below in to `index.html`\. This HTML references `main.js`\. This is a bundled version of index\.js, which includes the required AWS SDK for JavaScript modules\. 

```
<!DOCTYPE html>
<html xmlns:th="http://www.thymeleaf.org" xmlns:sec="http://www.thymeleaf.org/thymeleaf-extras-springsecurity3">
<head>
    <meta charset="utf-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link rel="icon" href="./images/favicon.ico" />
    <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"/>
    <link rel="stylesheet" href="./css/styles.css"/>
    <script src="https://code.jquery.com/jquery-1.12.4.min.js"></script>
    <script src="https://code.jquery.com/ui/1.11.4/jquery-ui.min.js"></script>
    <script src="./js/main.js"></script>
    <style>
        .messageelement {
            margin: auto;
            border: 2px solid #dedede;
            background-color: #D7D1D0 ;
            border-radius: 5px;
            max-width: 800px;
            padding: 10px;
            margin: 10px 0;
        }

        .messageelement::after {
            content: "";
            clear: both;
            display: table;
        }

        .messageelement img {
            float: left;
            max-width: 60px;
            width: 100%;
            margin-right: 20px;
            border-radius: 50%;
        }

        .messageelement img.right {
            float: right;
            margin-left: 20px;
            margin-right:0;
        }
    </style>
</head>
<body>

<div class="container">
    <h2>AWS Sample Messaging Application</h2>
    <div id="messages">

    </div>

    <div class="input-group mb-3">
        <div class="input-group-prepend">
            <span class="input-group-text" id="basic-addon1">Sender:</span>
        </div>
        <select name="cars" id="username">
            <option value="Scott">Brian</option>
            <option value="Tricia">Tricia</option>
        </select>
    </div>

    <div class="input-group">
        <div class="input-group-prepend">
            <span class="input-group-text">Message:</span>
        </div>
        <textarea class="form-control" id="textarea" aria-label="With textarea"></textarea>
        <button type="button" onclick="pushMessage()" id="send" class="btn btn-success">Send</button>
        <button type="button" onclick="purge()" id="refresh" class="btn btn-success">Purge</button>
    </div>
    <!-- All of these child items are hidden and only displayed in a FancyBox ------------------------------------------------------>
    <div id="hide" style="display: none">

        <div id="base" class="messageelement">
            <img src="../public/images/av2.png"  alt="Avatar" class="right" style="width:100%;">
            <p id="text">Excellent! So, what do you want to do today?</p>
            <span class="time-right">11:02</span>
        </div>

    </div>
</div>
</body>
</html>
```

This code is also available [ here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/master/javascriptv3/example_code/cross-services/message-app/setup.yaml)\.