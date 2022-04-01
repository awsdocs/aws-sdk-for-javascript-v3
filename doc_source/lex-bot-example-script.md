--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Create the browser script<a name="lex-bot-example-script"></a>

This topic is part of a tutorial that create an Amazon Lex chatbot within a web application to engage your web site visitor\. To start at the beginning of the tutorial, see [Building an Amazon Lex chatbot](lex-bot-example.md)\.

Create a file named `index.js`\. Copy and paste the code below into `index.js`\. Import the required AWS SDK for JavaScript modules and commands\. Create clients for Amazon Lex, Amazon Comprehend, and Amazon Translate\. Replace *REGION* with AWS Region, and *IDENTITY\_POOL\_ID* with the ID of the identity pool you created in the [Create the AWS resources ](lex-bot-provision-resources.md)\. To retrieve this identity pool ID, open the identity pool in the Amazon Cognito console, choose **Edit identity pool**, and choose **Sample code** in the side menu\. The identity pool ID is shown in red text in the console\.

First, create a `libs` directory create the required service client objects by creating three files, `comprehendClient.js`, `lexClient.js`, and `transcribeClient.js`\. Paste the approriate code below into each, and replace *REGION* and *IDENTITY\_POOL\_ID* in each\. 

**Note**  
Use the ID of the Amazon Cognito identity pool you created in [Create the AWS resources using AWS CloudFormation](lex-bot-provision-resources.md#lex-bot-example-resources-cli)\.

```
import { CognitoIdentityClient } from "@aws-sdk/client-cognito-identity";
import { fromCognitoIdentityPool } from "@aws-sdk/credential-provider-cognito-identity";
import { ComprehendClient } from "@aws-sdk/client-comprehend";
const REGION = "REGION"; //e.g. "us-east-1"
const IdentityPoolId = "IDENTITY_POOL_ID";
const comprehendClient = new ComprehendClient({
  region: REGION,
  credentials: fromCognitoIdentityPool({
    client: new CognitoIdentityClient({ region: REGION }),
    identityPoolId: IdentityPoolId
  }),
});
```

```
import { CognitoIdentityClient } from "@aws-sdk/client-cognito-identity";
import { fromCognitoIdentityPool } from "@aws-sdk/credential-provider-cognito-identity";
import { LexClient } from "@aws-sdk/client-lex";
const REGION = "REGION"; //e.g. "us-east-1"
const IdentityPoolId = "IDENTITY_POOL_ID";
const lexClient = new LexRuntimeServiceClient({
  region: REGION,
  credentials: fromCognitoIdentityPool({
    client: new CognitoIdentityClient({ region: REGION }),
    identityPoolId: IdentityPoolId
  }),
});
```

```
import { CognitoIdentityClient } from "@aws-sdk/client-cognito-identity";
import { fromCognitoIdentityPool } from "@aws-sdk/credential-provider-cognito-identity";
import { TranslateClient } from "@aws-sdk/client-translate";
const REGION = "REGION"; //e.g. "us-east-1"
const IdentityPoolId = "IDENTITY_POOL_ID";
const translateClient = new TranslateClient({
  region: REGION,
  credentials: fromCognitoIdentityPool({
    client: new CognitoIdentityClient({ region: REGION }),
    identityPoolId: IdentityPoolId
  }),
});
```

This code is available [here on GitHub\.](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/cross-services/lex-bot/src/libs)\.

Next, create an `index.html` file, and paste the code below into it\.

 Replace *BOT\_ALIAS* and *BOT\_NAME* with the alias and name of your Amazon Lex bot respectively, and *USER\_ID* with a user id\. The `createResponse` asynchronous function does the following:
+ Takes the text inputted by the user into the browser and uses Amazon Comprehend to determine its language code\.
+ Takes the language code and uses Amazon Translate to translate the text into English\.
+ Takes the translated text and uses Amazon Lex to generate a response\.
+ Posts the response to the browser page\.

```
import {
  DetectDominantLanguageCommand
} from "@aws-sdk/client-comprehend";
import {
  TranslateTextCommand
} from "@aws-sdk/client-translate";
import {
  PostTextCommand
} from "@aws-sdk/client-lex-runtime-service";
import {lexClient} from ".libs/lexClient.js";
import {comprehendClient} from ".libs/comprehendClient.js";
import {translateClient} from ".libs/translateClient.js";

var g_text = "";
// set the focus to the input box
document.getElementById("wisdom").focus();

function showRequest(daText) {
  var conversationDiv = document.getElementById("conversation");
  var requestPara = document.createElement("P");
  requestPara.className = "userRequest";
  requestPara.appendChild(document.createTextNode(g_text));
  conversationDiv.appendChild(requestPara);
  conversationDiv.scrollTop = conversationDiv.scrollHeight;
};

function showResponse(lexResponse) {
  var conversationDiv = document.getElementById("conversation");
  var responsePara = document.createElement("P");
  responsePara.className = "lexResponse";

  var lexTextResponse = lexResponse;

  responsePara.appendChild(document.createTextNode(lexTextResponse));
  responsePara.appendChild(document.createElement("br"));
  conversationDiv.appendChild(responsePara);
  conversationDiv.scrollTop = conversationDiv.scrollHeight;
};

function handletext(text) {
  g_text = text;
  var xhr = new XMLHttpRequest();
  xhr.addEventListener("load", loadNewItems, false);
  xhr.open("POST", "../text", true); // A Spring MVC controller
  xhr.setRequestHeader("Content-type", "application/x-www-form-urlencoded"); //necessary
  xhr.send("text=" + text);
};

function loadNewItems(event) {
  var msg = event.target.responseText;
  showRequest();
  showResponse(msg);

  // re-enable input
  var wisdomText = document.getElementById("wisdom");
  wisdomText.value = "";
  wisdomText.locked = false;
};

// Respond to user's input.
const createResponse = async () => {
  // Confirm there is text to submit.
  var wisdomText = document.getElementById("wisdom");
  if (wisdomText && wisdomText.value && wisdomText.value.trim().length > 0) {
    // Disable input to show it is being sent.
    var wisdom = wisdomText.value.trim();
    wisdomText.value = "...";
    wisdomText.locked = true;

    const comprehendParams = {
      Text: wisdom
    };
    try {
      const data = await comprehendClient.send(
        new DetectDominantLanguageCommand(comprehendParams)
      );
      console.log("Success. The language code is: ", data.Languages[0].LanguageCode);
      const translateParams = {
        SourceLanguageCode: data.Languages[0].LanguageCode,
        TargetLanguageCode: "en", // For example, "en" for English.
        Text: wisdom
      };
      try {
        const data = await translateClient.send(
          new TranslateTextCommand(translateParams)
        );
        console.log("Success. Translated text: ", data.TranslatedText);
        const lexParams = {
          botAlias: "BOT_ALIAS",
          botName: "BOT_NAME",
          inputText: data.TranslatedText,
          userId: "USER_ID" // For example, 'chatbot-demo'.
        };
        try {
          const data = await lexClient.send(new PostTextCommand(lexParams));
          console.log("Success. Response is: ", data.message);
          document.getElementById("conversation").innerHTML = data.message;
        } catch (err) {
          console.log("Error responding to message. ", err);
        }
      } catch (err) {
        console.log("Error translating text. ", err);
      }
    } catch (err) {
      console.log("Error identifying language. ", err);
    }
  }
};
window.createResponse = createResponse;
```

This code is available [here on GitHub\.](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/cross-services/lex-bot/src/index.html)\.

Now use webpack to bundle the `index.js` and AWS SDK for JavaScript modules into a single file, `main.js`\.

1. If you haven't already, follow the [Prerequisites](lex-bot-example-prerequisites.md) for this example to install webpack\. 
**Note**  
For information about*webpack*, see [Bundling applications with webpack](webpack.md)\.

1. Run the the following in the command line to bundle the JavaScript for this example into a file called `<index.js>` :

   ```
   webpack index.js --mode development --target web --devtool false -o main.js
   ```