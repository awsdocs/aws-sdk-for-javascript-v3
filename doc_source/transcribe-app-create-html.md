--------

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\. 

--------

# Create the HTML<a name="transcribe-app-create-html"></a>

This topic is part of a tutorial about building an app that transcribes and displays voice messages for authenticated users\. To start at the beginning of the tutorial, see [Build a transcription app with authenticated users](transcribe-app.md)\. 

Create an `index.html` file, and copy and paste the content below into it\. The page features panel of buttons for recording voice messages, and a table displaying the current user’s previously transcribed messages\. The script tag at the end of the `body` element invokes the `main.js`, which contain all the browser script for the app\. You create the main\.js using Webpack, as described in the following section of this tutorial\.

```
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <title>title</title>
    <link rel="stylesheet" type="text/css" href="recorder.css">
    <style>
        table, td {
            border: 1px solid black;
        }
    </style>
</head>
<body>
<h2>Record</h2>
<p>
    <button id="record" onclick="startRecord()"></button>
    <button id="stopRecord" disabled onclick="stopRecord()">Stop</button>
<p id="demo" style="visibility: hidden;"></p>
</p>
<p>
    <audio id="recordedAudio"></audio>
</p>

<h2>My transcriptions</h2>
<table id="myTable1" style ="width:678px;">
</table>
<table id="myTable" style ="width:678px;">
        <tr>
    <td style = "font-weight:bold">Time created</td>
    <td style = "font-weight:bold">Transcription</td>
    <td style = "font-weight:bold">Download</td>
    <td style = "font-weight:bold">Delete</td>
       </tr>
</table>

<script type="text/javascript" src="./main.js"></script>
</body>

</html>
```

This code example is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/blob/main/javascriptv3/example_code/cross-services/transcription-app/src/index.js)\.