--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Viewing photos in an Amazon S3 bucket from a browser<a name="s3-example-photos-view"></a>

![\[JavaScript code example that applies to browser execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/browsericon.png)

**This browser script code example shows:**
+ How to create a photo album in an Amazon Simple Storage Service \(Amazon S3\) bucket and allow unauthenticated users to view the photos\.

## The scenario<a name="s3-example-photos-view-scenario"></a>

In this example, a simple HTML page provides a browser\-based application for viewing the photos in a photo album\. The photo album is in an Amazon S3 bucket into which photos are uploaded\.



The browser script uses the SDK for JavaScript to interact with an Amazon S3 bucket\. The script uses the [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#listObjects-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#listObjects-property) method of the Amazon S3 client class to enable you to view the photo albums\.

## Prerequisite tasks<a name="s3-example-photos-view-scenario-prerequisites"></a>

To set up and run this example, first complete these tasks\.

**Note**  
In this example, you must use the same AWS Region for both the Amazon S3 bucket and the Amazon Cognito identity pool\.

### Set up your local environment<a name="s3-example-photos-view-prereq-environment"></a>

Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on[ GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/photoViewer/README.md)\.

### Create the bucket<a name="s3-example-photos-view-prereq-bucket"></a>

In the [Amazon S3 console](https://console.aws.amazon.com/s3/), create an Amazon S3 bucket where you can store albums and photos\. For more information about using the console to create an S3 bucket, see [Creating a bucket](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) in the *Amazon Simple Storage Service Console User Guide*\.

As you create the Amazon S3 bucket, be sure to do the following:
+ Make note of the bucket name so you can use it in a subsequent prerequisite task, [Configure role permissions](#s3-example-photos-view-prereq-perms)\.
+ Choose an AWS Region to create the bucket in\. This must be the same Region that you'll use to create an Amazon Cognito identity pool in a subsequent prerequisite task, [Create an identity pool](#s3-example-photos-view-prereq-idpool)\.
+ In the **Create Bucket** wizard, on the **Create Bucket** page, in the **Bucket settings for block public access** section, clear these boxes: **Block public access to buckets and objects granted through new access control lists \(ACLs\)** and **Block public access to buckets and objects granted through any access control lists \(ACLs\)**\.

  For information about how to check and configure bucket permissions, see [Setting permissions for website access](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteAccessPermissionsReqd.html) in the *Amazon Simple Storage Service Console User Guide*\.

### Create an identity pool<a name="s3-example-photos-view-prereq-idpool"></a>

On the [Amazon Cognito console](https://console.aws.amazon.com/cognito/), create an Amazon Cognito identity pool\. 

As you create the identity pool:
+ Make note of the identity pool name, and the role name for the **unauthenticated** identity\.
+ On the **Sample Code** page, select "JavaScript" from the **Platform** list\. Then copy or write down the sample code\.
**Note**  
You must choose "JavaScript" from the **Platform** list for your code to work\.

### Configure role permissions<a name="s3-example-photos-view-prereq-perms"></a>

To allow viewing of albums and photos, you have to add permissions to an IAM role of the identity pool that you just created\. Start by creating a policy as follows\.

1. Open the [IAM console](https://console.aws.amazon.com/iam/)\.

1. In the navigation pane on the left, choose **Policies**, and then choose **Create policy**\.

1. On the **JSON** tab, enter the following JSON definition, but replace *BUCKET\_NAME* with the name of the bucket\.

   ```
   {
      "Version": "2012-10-17",
      "Statement": [
         {
            "Effect": "Allow",
            "Action": [
               "s3:ListBucket"
            ],
            "Resource": [
               "arn:aws:s3:::BUCKET_NAME"
            ]
         }
      ]
   }
   ```

1. Choose the **Review policy** button, name the policy and provide a description \(if you want\), and then choose the **Create policy** button\.

   Be sure to make note of the name so that you can find it and attach it to the IAM role later\.

After the policy is created, navigate back to the [IAM console](https://console.aws.amazon.com/iam/)\. Find the IAM role for the **unauthenticated** identity that Amazon Cognito created in the previous prerequisite task, [Create an identity pool](#s3-example-photos-view-prereq-idpool)\. You use the policy you just created to add permissions to this identity\.

For additional information about creating an IAM role, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

### Configure CORS<a name="s3-example-photos-view-cors-configuration"></a>

Before the browser script can access the Amazon S3 bucket, you have to set up its [CORS configuration](cors.md#configuring-cors-s3-bucket) as follows\.

**Important**  
In the new S3 console, the CORS configuration must be JSON\.

------
#### [ JSON ]

```
[
    {
        "AllowedHeaders": [
            "*"
        ],
        "AllowedMethods": [
            "HEAD",
            "GET"
        ],
        "AllowedOrigins": [
            "*"
        ]
    }
]
```

------
#### [ XML ]

```
<?xml version="1.0" encoding="UTF-8"?>
<CORSConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <CORSRule>
        <AllowedOrigin>*</AllowedOrigin>
        <AllowedMethod>GET</AllowedMethod>
        <AllowedMethod>HEAD</AllowedMethod>
        <AllowedHeader>*</AllowedHeader>
    </CORSRule>
</CORSConfiguration>
```

------

### Create albums and upload photos<a name="s3-example-photos-view-create-albums"></a>

Because this example only allows users to view the photos that are already in the bucket, you need to create some albums in the bucket and upload photos to them\.

**Note**  
For this example, the file names of the photo files must start with a single underscore \("\_"\)\. This character is important later for filtering\. In addition, be sure to respect the copyrights of the owners of the photos\.

1. On the [Amazon S3 console](https://console.aws.amazon.com/s3/), open the bucket that you created earlier\.

1. On the **Overview** tab, choose **Create folder** to create folders\. For this example, name the folders "album1", "album2", and "album3"\.

1. For **album1** and then **album2**, select the folder and then upload photos to it as follows:

   1. Choose **Upload**\.

   1. Drag or choose the photo files you want to use, and then choose **Next**\.

   1. Under **Manage public permissions**, choose **Grant public read access to this object\(s\)**\.

   1. Choose **Upload** \(in the lower\-left corner\)\.

1. Leave **album3** empty\.

### Install the required SDK clients and packages<a name="s3-example-photos-install-modules"></a>

Install the following SDK modules:
+ `client-s3`
+ `client-cognito-identity`
+ `credential-provider-cognito-identity`

**Note**  
For information on installing SDK modules, see [Installing the SDK for JavaScript](installing-jssdk.md)\.

### Install webpack<a name="s3-example-photos-install-webpack"></a>

To use V3 of the AWS SDK for JavaScript in the browser, you require `webpack` to bundle the Javascript modules and functions\.

To install `webpack`, run the following at a command prompt\.

```
npm install --save-dev webpack
```

**Important**  
To view a sample of the `package.json`for this example, see the [AWS SDK for JavaScript code samples on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/photoViewer/package.json)\.

**Note**  
For information on installing Webpack, see [Bundling applications with webpack](webpack.md)\.

## Defining the webpage<a name="s3-example-photos-view-html"></a>

The HTML for the photo\-viewing application consists of a `<div>` element in which the browser script creates the viewing interface\. 

The `<script>` element adds the `main.js` file, which contains all the required JavaScript for the example\.

**Note**  
 To generate the `main.js` file, see [Running the code](#s3-photos-view-running-the-code)\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
<!DOCTYPE html>
<html>
<head>
    <script type="text/javascript" src="./main.js"></script>
</head>
<body>
<h1>Photo album viewer</h1>
<div id="viewer" />
<script>
    listAlbums();
</script>
</body>
</html>
```

## Configuring the SDK<a name="s3-example-photos-view-config"></a>

Obtain the credentials you need to configure the SDK by calling the `CognitoIdentityCredentials` method\. You need to provide the Amazon Cognito identity pool ID\. Then create an `S3` service object\.

```
// Load the required clients and packages
const { CognitoIdentityClient } = require("@aws-sdk/client-cognito-identity");
const {
  fromCognitoIdentityPool,
} = require("@aws-sdk/credential-provider-cognito-identity");
const { S3Client, ListObjectsCommand } = require("@aws-sdk/client-s3");

// Initialize the Amazon Cognito credentials provider
const REGION = "region"; //e.g., 'us-east-1'
const s3 = new S3Client({
  region: REGION,
  credentials: fromCognitoIdentityPool({
    client: new CognitoIdentityClient({ region: REGION }),
    identityPoolId: "IDENTITY_POOL_ID", // IDENTITY_POOL_ID e.g., eu-west-1:xxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx
  }),
});
```

The remaining code in this example defines the following functions to gather and present information about the albums and photos in the bucket\.
+ `listAlbums`
+ `viewAlbum`

## Listing albums in the bucket<a name="s3-example-photos-view-list-albums"></a>

To list all of the existing albums in the bucket, the application's `listAlbums` function calls the `ListObjectsCommand` method of the `S3` client service object\. The function uses the `CommonPrefixes` property so that the call returns only objects that are used as albums \(that is, the folders\)\.

The rest of the function takes the list of albums from the Amazon S3 bucket and generates the HTML needed to display the album list on the webpage\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// A utility function to create HTML.
function getHtml(template) {
  return template.join("\n");
}
// Make the getHTML function available to the browser
window.getHTML = getHtml;

// List the photo albums that exist in the bucket
var albumBucketName = "BUCKET_NAME"; //BUCKET_NAME

const listAlbums = async () => {
  try {
    const data = await s3.send(
      new ListObjectsCommand({ Delimiter: "/", Bucket: albumBucketName })
    );
    var albums = data.CommonPrefixes.map(function (commonPrefix) {
      var prefix = commonPrefix.Prefix;
      var albumName = decodeURIComponent(prefix.replace("/", ""));
      return getHtml([
        "<li>",
        '<button style="margin:5px;" onclick="viewAlbum(\'' +
          albumName +
          "')\">",
        albumName,
        "</button>",
        "</li>",
      ]);
    });
    var message = albums.length
      ? getHtml(["<p>Click an album name to view it.</p>"])
      : "<p>You don't have any albums. You need to create an album.";
    var htmlTemplate = [
      "<h2>Albums</h2>",
      message,
      "<ul>",
      getHtml(albums),
      "</ul>",
    ];
    document.getElementById("viewer").innerHTML = getHtml(htmlTemplate);
  } catch (err) {
    return alert("There was an error listing your albums: " + err.message);
  }
};
// Make the viewAlbum function available to the browser
window.listAlbums = listAlbums;
```

## Viewing an album<a name="s3-example-photos-view-viewing-album"></a>

To display the contents of an album in the Amazon S3 bucket, the application's `viewAlbum` function takes an album name and creates the Amazon S3 key for that album\. The function then calls the `ListObjectsCommand` method of the `S3` client service object to obtain a list of all the objects \(the photos\) in the album\.

The rest of the function takes the list of objects that are in the album and generates the HTML needed to display the photos on the webpage\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Show the photos that exist in an album
const viewAlbum = async (albumName) => {
  try {
    var albumPhotosKey = encodeURIComponent(albumName) + "/";
    const data = await s3.send(
      new ListObjectsCommand({
        Prefix: albumPhotosKey,
        Bucket: albumBucketName,
      })
    );
    var href = "https://s3." + REGION + ".amazonaws.com/";
    var bucketUrl = href + albumBucketName + "/";
    var photos = data.Contents.map(function (photo) {
      var photoKey = photo.Key;
      var photoUrl = bucketUrl + encodeURIComponent(photoKey);
      return getHtml([
        "<span>",
        "<div>",
        "<br/>",
        '<img style="width:128px;height:128px;" src="' + photoUrl + '"/>',
        "</div>",
        "<div>",
        "<span>",
        photoKey.replace(albumPhotosKey, ""),
        "</span>",
        "</div>",
        "</span>",
      ]);
    });
    var message = photos.length
      ? "<p>The following photos are present.</p>"
      : "<p>There are no photos in this album.</p>";
    var htmlTemplate = [
      "<div>",
      '<button onclick="listAlbums()">',
      "Back To albums",
      "</button>",
      "</div>",
      "<h2>",
      "Album: " + albumName,
      "</h2>",
      message,
      "<div>",
      getHtml(photos),
      "</div>",
      "<h2>",
      "End of album: " + albumName,
      "</h2>",
      "<div>",
      '<button onclick="listAlbums()">',
      "Back To albums",
      "</button>",
      "</div>",
    ];
    document.getElementById("viewer").innerHTML = getHtml(htmlTemplate);
    document
      .getElementsByTagName("img")[0]
      .setAttribute("style", "display:none;");
  } catch (err) {
    return alert("There was an error viewing your album: " + err.message);
  }
};

// Make the viewAlbum function available to the browser
window.viewAlbum = viewAlbum;
```

## Running the code<a name="s3-photos-view-running-the-code"></a>

**To run the code for this example**

1. Save all the code as `s3_PhotoViewer.ts`\.
**Note**  
This file is available on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/photoViewer/src/s3_PhotoViewer.ts)\.

1. Replace *"REGION"* with your AWS Region, such as `us-west-2`\.

1. Replace *"BUCKET\_NAME"* with your Amazon S3 bucket\.

1. Replace *"IDENTITY\_POOL\_ID"* with the `IdentityPoolId` from the **Sample page** of the Amazon Cognito identity pool you created for this example\.
**Note**  
The *IDENTITY\_POOL\_ID* is displayed in red on the console as shown\.  

![\[Preparing an Amazon Cognito identity pool for the browser script\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Preparing an Amazon Cognito identity pool for the browser script\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)![\[Preparing an Amazon Cognito identity pool for the browser script\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/)

1. Run the following at the command prompt to bundle the JavaScript for this example into a file called `main.js`\.

   ```
   webpack s3_PhotoViewer.ts --mode development --target web --devtool false -o main.js
   ```
**Note**  
For information about installing `webpack`, see [Bundling applications with webpack](webpack.md)\.

1. Run the following in the command line:

   ```
   node s3_PhotoViewer.ts
   ```