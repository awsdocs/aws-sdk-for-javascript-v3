--------

Help us improve the AWS SDK for JavaScript version 3 \(V3\) documentation by providing feedback using the **Feedback** link, or create an issue or pull request on [GitHub](https://github.com/awsdocs/aws-sdk-for-javascript-v3)\.

 The [AWS SDK for JavaScript V3 API Reference Guide](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/index.html) describes in detail all the API operations for the AWS SDK for JavaScript version 3 \(V3\)\.

--------

# Uploading photos to Amazon S3 from a browser<a name="s3-example-photo-album"></a>

![\[JavaScript code example that applies to browser execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/browsericon.png)

**This browser script code example shows:**
+ How to create a browser application that allows users to create photo albums in an Amazon S3 bucket and upload photos into the albums\.

## The scenario<a name="s3-example-photo-album-scenario"></a>

In this example, a simple HTML page provides a browser\-based application for creating photo albums in an Amazon S3 bucket into which you can upload photos\. The application lets you delete photos and albums that you add\.



The browser script uses the SDK for JavaScript to interact with an Amazon S3 bucket\. Use the following methods of the Amazon S3 client class to enable the photo album application: 
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/putobjectcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/putobjectcommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/putobjectcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/putobjectcommand.html)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/putobjectcommand.html](https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/clients/client-s3/classes/putobjectcommand.html)

## Prerequisite tasks<a name="s3-example-photo-album-scenario-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ Set up the project environment to run these Node TypeScript examples, and install the required AWS SDK for JavaScript and third\-party modules\. Follow the instructions on [GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/photoExample/README.md)\.
+ In the [Amazon S3 console](https://console.aws.amazon.com/s3/), create an Amazon S3 bucket that you will use to store the photos in the album\. For more information about creating a bucket in the console, see [Creating a bucket](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) in the *Amazon Simple Storage Service User Guide*\. Make sure you have both **Read** and **Write** permissions on **Objects**\. For more information about setting bucket permissions, see [Setting permissions for website access](https://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteAccessPermissionsReqd.html)\.
+ In the [Amazon Cognito console](https://console.aws.amazon.com/cognito/), create an Amazon Cognito identity pool using Federated Identities with access enabled for unauthenticated users in the same Region as the Amazon S3 bucket\. You need to include the identity pool ID in the code to obtain credentials for the browser script\. For more information about Amazon Cognito Federated Identities, see [Amazon Cognito identity pools \(federated identites\)](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-identity.html) in the *Amazon Cognito Developer Guide*\.
+ In the [IAM console](https://console.aws.amazon.com/iam/), find the IAM role created by Amazon Cognito for unauthenticated users\. Add the following policy to grant read and write permissions to an Amazon S3 bucket\. For more information about creating an IAM role, see [Creating a role to delegate permissions to an AWS service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

  Use this role policy for the IAM role created by Amazon Cognito for unauthenticated users\.
**Warning**  
If you enable access for unauthenticated users, you will grant write access to the bucket, and all objects in the bucket, to anyone in the world\. This security posture is useful in this example to keep it focused on the primary goals of the example\. In many live situations, however, tighter security, such as using authenticated users and object ownership, is highly advisable\.

  ```
  {
     "Version": "2012-10-17",
     "Statement": [
        {
           "Effect": "Allow",
           "Action": [
              "s3:DeleteObject",
              "s3:GetObject",
              "s3:ListBucket",
              "s3:PutObject",
              "s3:PutObjectAcl"
           ],
           "Resource": [
              "arn:aws:s3:::BUCKET_NAME",
              "arn:aws:s3:::BUCKET_NAME/*"
           ]
        }
     ]
  }
  ```

## Configuring CORS<a name="s3-example-photo-album-cors-configuration"></a>

Before the browser script can access the Amazon S3 bucket, you must first set up its [CORS configuration](cors.md#configuring-cors-s3-bucket) as follows\.

**Important**  
In Amazon S3 on the new AWS Web Services Management console, the CORS configuration must be JSON\.

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
            "GET",
            "PUT",
            "POST",
            "DELETE"
        ],
        "AllowedOrigins": [
            "*"
        ],
        "ExposeHeaders": [
            "ETag"
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
        <AllowedMethod>POST</AllowedMethod>
        <AllowedMethod>GET</AllowedMethod>
        <AllowedMethod>PUT</AllowedMethod>
        <AllowedMethod>DELETE</AllowedMethod>
        <AllowedMethod>HEAD</AllowedMethod>
        <AllowedHeader>*</AllowedHeader>
        <ExposeHeader>ETag</ExposeHeader>
    </CORSRule>
</CORSConfiguration>
```

------

## Install the required SDK clients and packages<a name="s3-example-photo-album-install-modules"></a>

Install the following SDK modules:

**Note**  
For information on installing SDK modules, see [Installing the SDK for JavaScript](installing-jssdk.md)\.
+ client\-s3
+ credential\-providers
+ credential\-provider\-cognito\-identity

### Install webpack<a name="s3-example-photo-album-install-webpack"></a>

To use V3 of the AWS SDK for JavaScript in the browser, you require Webpack to bundle the javascript modules and functions\.

To install web pack, run the following in the command line:

```
npm install --save-dev webpack
```

**Important**  
To view a sample of the package\.json for this example, see the [AWS SDK for JavaScript code samples on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/photoExample/package.json)\.

**Note**  
For information on installing Webpack, see [Bundling applications with webpack](webpack.md)\.

## Defining the webpage<a name="s3-example-photo-album-html"></a>

The HTML for the photo\-viewing application consists of a `<div>` element in which the browser script creates the viewing/uploading interface\. 

The `<script>` element adds the `<main.js>` file, which contains all the required JavaScript for the example\.

**Note**  
 To generate the `<main.js>` file, see [Running the code](#s3-photos-example-running-the-code) below\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
<!DOCTYPE html>
<html>
<head>
    <script src="./main.js"></script>
    <script>
        function getHtml(template) {
            return template.join("\n");
        }
        listAlbums();
    </script>
</head>
<body>
<h1>My photo albums app</h1>
<div id="app"></div>
</body>
</html>
```

## Configuring the SDK<a name="s3-example-photo-album-configure-sdk"></a>

Obtain the credentials needed to configure the SDK by calling the `CognitoIdentityCredentials` method, providing the Amazon Cognito identity pool ID\. Next, create an `S3` client service object\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Load the required clients and packages
const { CognitoIdentityClient } = require("@aws-sdk/client-cognito-identity");
const {
  fromCognitoIdentityPool,
} = require("@aws-sdk/credential-provider-cognito-identity");
const { S3Client, PutObjectCommand, ListObjectsCommand, DeleteObjectCommand, DeleteObjectsCommand } = require("@aws-sdk/client-s3");

// Set the AWS Region
const REGION = "REGION"; //REGION

// Initialize the Amazon Cognito credentials provider
const s3 = new S3Client({
  region: REGION,
  credentials: fromCognitoIdentityPool({
    client: new CognitoIdentityClient({ region: REGION }),
    identityPoolId: "IDENTITY_POOL_ID", // IDENTITY_POOL_ID
  }),
});

const albumBucketName = "BUCKET_NAME"; //BUCKET_NAME
```

Nearly all of the rest of the code in this example is organized into a series of functions that gather and present information about the albums in the bucket, upload and display photos uploaded into albums, and delete photos and albums\. Those functions are:
+ `listAlbums`
+ `createAlbum`
+ `viewAlbum`
+ `addPhoto`
+ `deleteAlbum`
+ `deletePhoto`

## Listing albums in the bucket<a name="s3-example-photo-album-list-albums"></a>

The application creates albums in the Amazon S3 bucket as objects whose keys begin with a forward slash character, indicating the object functions as a folder\. To list all the existing albums in the bucket, the application's `listAlbums` function calls the `ListObjectsCommand` method of the `S3` client service object while using `commonPrefix` so the call returns only objects used as albums\.

The rest of the function takes the list of albums from the Amazon S3 bucket and generates the HTML needed to display the album list in the web page\. It also enables deleting and opening individual albums\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// A utility function to create HTML
function getHtml(template) {
  return template.join("\n");
}
// Make getHTML function available to the browser
window.getHTML = getHtml;

// List the photo albums that exist in the bucket
const listAlbums = async () => {
  try {
    const data = await s3.send(
        new ListObjectsCommand({ Delimiter: "/", Bucket: albumBucketName })
    );

    if (data.CommonPrefixes === undefined) {
      const htmlTemplate = [
        "<p>You don't have any albums. You need to create an album.</p>",
        "<button onclick=\"createAlbum(prompt('Enter album name:'))\">",
        "Create new album",
        "</button>",
      ];
      document.getElementById("app").innerHTML = htmlTemplate;
    } else {
      var albums = data.CommonPrefixes.map(function (commonPrefix) {
        var prefix = commonPrefix.Prefix;
        var albumName = decodeURIComponent(prefix.replace("/", ""));
        return getHtml([
          "<li>",
          "<span onclick=\"deleteAlbum('" + albumName + "')\">X</span>",
          "<span onclick=\"viewAlbum('" + albumName + "')\">",
          albumName,
          "</span>",
          "</li>",
        ]);
      });
      var message = albums.length
          ? getHtml([
            "<p>Click an album name to view it.</p>",
            "<p>Click the X to delete the album.</p>",
          ])
          : "<p>You do not have any albums. You need to create an album.";
      const htmlTemplate = [
        "<h2>Albums</h2>",
        message,
        "<ul>",
        getHtml(albums),
        "</ul>",
        "<button onclick=\"createAlbum(prompt('Enter Album Name:'))\">",
        "Create new Album",
        "</button>",
      ];
      document.getElementById("app").innerHTML = getHtml(htmlTemplate);
    }
  } catch (err) {
    return alert("There was an error listing your albums: " + err.message);
  }
};

// Make listAlbums function available to the browser
window.listAlbums = listAlbums;
```

## Creating an album in the bucket<a name="s3-example-photo-album-create-album"></a>

To create an album in the Amazon S3 bucket, the application's `createAlbum` function first validates the name given for the new album to ensure it contains suitable characters\. The function then forms an Amazon S3 object key, passing it to the `headObject` method of the Amazon S3 service object\. This method returns the metadata for the specified key, so if it returns data, then an object with that key already exists\.

If the album doesn't already exist, the function calls the `PutObjectCommand` method of the `S3` client service object to create the album\. It then calls the `viewAlbum` function to display the new empty album\.

**Note**  
This example imports and uses the required AWS Service V3 package clients, V3 commands, and uses the `send` method in an async/await pattern\. You can create this example using V2 commands instead by making some minor changes\. For details, see [Using V3 commands](welcome.md#using_v3_commands)\.

```
// Create an album in the bucket
const createAlbum = async (albumName) => {
  albumName = albumName.trim();
  if (!albumName) {
    return alert("Album names must contain at least one non-space character.");
  }
  if (albumName.indexOf("/") !== -1) {
    return alert("Album names cannot contain slashes.");
  }
  var albumKey = encodeURIComponent(albumName);
  try {
    const key = albumKey + "/";
    const params = { Bucket: albumBucketName, Key: key };
    const data = await s3.send(new PutObjectCommand(params));
    alert("Successfully created album.");
    viewAlbum(albumName);
  } catch (err) {
    return alert("There was an error creating your album: " + err.message);
  }
};

// Make createAlbum function available to the browser
window.createAlbum = createAlbum;
```

## Viewing an album<a name="s3-example-photo-album-viewing-album"></a>

To display the contents of an album in the Amazon S3 bucket, the application's `viewAlbum` function takes an album name and creates the Amazon S3 key for that album\. The function then calls the `listObjects` method of the `S3` client service object to obtain a list of all the objects \(photos\) in the album\.

The rest of the function takes the list of objects \(photos\) from the album and generates the HTML needed to display the photos in the web page\. It also enables deleting individual photos and navigating back to the album list\.

```
// View the contents of an album

const viewAlbum = async (albumName) => {
  const albumPhotosKey = encodeURIComponent(albumName) + "/";
  try {
    const data = await s3.send(
        new ListObjectsCommand({
          Prefix: albumPhotosKey,
          Bucket: albumBucketName,
        })
    );
    if (data.Contents.length === 1) {
      var htmlTemplate = [
        "<p>You don't have any photos in this album. You need to add photos.</p>",
        '<input id="photoupload" type="file" accept="image/*">',
        '<button id="addphoto" onclick="addPhoto(\'' + albumName + "')\">",
        "Add photo",
        "</button>",
        '<button onclick="listAlbums()">',
        "Back to albums",
        "</button>",
      ];
      document.getElementById("app").innerHTML = getHtml(htmlTemplate);
    } else {
      console.log(data);
      const href = "https://s3." + REGION + ".amazonaws.com/";
      const bucketUrl = href + albumBucketName + "/";
      const photos = data.Contents.map(function (photo) {
        const photoKey = photo.Key;
        console.log(photo.Key);
        const photoUrl = bucketUrl + encodeURIComponent(photoKey);
        return getHtml([
          "<span>",
          "<div>",
          '<img style="width:128px;height:128px;" src="' + photoUrl + '"/>',
          "</div>",
          "<div>",
          "<span onclick=\"deletePhoto('" +
          albumName +
          "','" +
          photoKey +
          "')\">",
          "X",
          "</span>",
          "<span>",
          photoKey.replace(albumPhotosKey, ""),
          "</span>",
          "</div>",
          "</span>",
        ]);
      });
      var message = photos.length
          ? "<p>Click the X to delete the photo.</p>"
          : "<p>You don't have any photos in this album. You need to add photos.</p>";
      const htmlTemplate = [
        "<h2>",
        "Album: " + albumName,
        "</h2>",
        message,
        "<div>",
        getHtml(photos),
        "</div>",
        '<input id="photoupload" type="file" accept="image/*">',
        '<button id="addphoto" onclick="addPhoto(\'' + albumName + "')\">",
        "Add photo",
        "</button>",
        '<button onclick="listAlbums()">',
        "Back to albums",
        "</button>",
      ];
      document.getElementById("app").innerHTML = getHtml(htmlTemplate);
      document.getElementsByTagName("img")[0].remove();
    }
  } catch (err) {
    return alert("There was an error viewing your album: " + err.message);
  }
};
// Make viewAlbum function available to the browser
window.viewAlbum = viewAlbum;
```

## Adding photos to an album<a name="s3-example-photo-album-adding-photos"></a>

To upload a photo to an album in the Amazon S3 bucket, the application's `addPhoto` function uses a file picker element in the web page to identify a file to upload\. It then forms a key for the photo to upload from the current album name and the file name\.

The function calls the `putObject` method of the Amazon S3 service object to upload the photo\. After uploading the photo, the function redisplays the album so the uploaded photo appears\.

```
// Add a photo to an album
const addPhoto = async (albumName) => {
  const files = document.getElementById("photoupload").files;
  try {
    const albumPhotosKey = encodeURIComponent(albumName) + "/";
    const data = await s3.send(
        new ListObjectsCommand({
          Prefix: albumPhotosKey,
          Bucket: albumBucketName
        })
    );
    const file = files[0];
    const fileName = file.name;
    const photoKey = albumPhotosKey + fileName;
    const uploadParams = {
      Bucket: albumBucketName,
      Key: photoKey,
      Body: file
    };
    try {
      const data = await s3.send(new PutObjectCommand(uploadParams));
      alert("Successfully uploaded photo.");
      viewAlbum(albumName);
    } catch (err) {
      return alert("There was an error uploading your photo: ", err.message);
    }
  } catch (err) {
    if (!files.length) {
      return alert("Choose a file to upload first.");
    }
  }
};
// Make addPhoto function available to the browser
window.addPhoto = addPhoto;
```

## Deleting a photo<a name="s3-example-photo-album-delete-photo"></a>

To delete a photo from an album in the Amazon S3 bucket, the application's `deletePhoto` function calls the `DeleteObjectCommand` method of the Amazon S3 client service object\. This deletes the photo specified by the `photoKey` value passed to the function\.

```
// Delete a photo from an album
const deletePhoto = async (albumName, photoKey) => {
  try {
    console.log(photoKey);
    const params = { Key: photoKey, Bucket: albumBucketName };
    const data = await s3.send(new DeleteObjectCommand(params));
    console.log("Successfully deleted photo.");
    viewAlbum(albumName);
  } catch (err) {
    return alert("There was an error deleting your photo: ", err.message);
  }
};
// Make deletePhoto function available to the browser
window.deletePhoto = deletePhoto;
```

## Deleting an album<a name="s3-example-photo-album-delete-album"></a>

To delete an album in the Amazon S3 bucket, the application's `deleteAlbum` function calls the `deleteObjects` method of the Amazon S3 client service object\.

```
// Delete an album from the bucket
const deleteAlbum = async (albumName) => {
  const albumKey = encodeURIComponent(albumName) + "/";
  try {
    const params = { Bucket: albumBucketName, Prefix: albumKey };
    const data = await s3.send(new ListObjectsCommand(params));
    const objects = data.Contents.map(function (object) {
      return { Key: object.Key };
    });
    try {
      const params = {
        Bucket: albumBucketName,
        Delete: { Objects: objects },
        Quiet: true,
      };
      const data = await s3.send(new DeleteObjectsCommand(params));
      listAlbums();
      return alert("Successfully deleted album.");
    } catch (err) {
      return alert("There was an error deleting your album: ", err.message);
    }
  } catch (err) {
    return alert("There was an error deleting your album1: ", err.message);
  }
};
// Make deleteAlbum function available to the browser
window.deleteAlbum = deleteAlbum;
```

## Running the code<a name="s3-photos-example-running-the-code"></a>

**To run the code for this example**

1. Save all the code as `s3_PhotoExample.js`\.
**Note**  
This file is available [here on GitHub](https://github.com/awsdocs/aws-doc-sdk-examples/tree/master/javascriptv3/example_code/s3/photoExample/src/s3_PhotoExample.js)\.

1. Replace *"REGION"* with your AWS Region, such as 'us\-east\-1'\.

1. Replace *"BUCKET\_NAME"* with your Amazon S3 bucket\.

1. Replace *"IDENTITY\_POOL\_ID"* with the IdentityPoolId from the **Sample page** of the Amazon Cognito Identity Pool you created for this example\.
**Note**  
The *IDENTITY\_POOL\_ID* is displayed in red in the console\.

1. Run the following in the command line to bundle the JavaScript for this example in to a file called `<main.js>`:

   ```
   npx webpack ./src/s3_PhotoExample.js --mode development --target web --no-devtool -o main.js
   ```
**Note**  
For information on installing WebPack, see [Bundling applications with webpack](webpack.md)\.

Run the following in the command line:

```
node s3_PhotoExample.js
```