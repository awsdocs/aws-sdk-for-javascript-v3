# Uploading Photos to Amazon S3 from a Browser<a name="s3-example-photo-album"></a>

![\[JavaScript code example that applies to browser execution\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/browsericon.png)

**This browser script code example shows:**
+ How to create a browser application that allows users to create photo albums in an Amazon S3 bucket and upload photos into the albums\.

## The Scenario<a name="s3-example-photo-album-scenario"></a>

In this example, a simple HTML page provides a browser\-based application for creating photo albums in an Amazon S3 bucket into which you can upload photos\. The application lets you delete photos and albums that you add\.

![\[JavaScript in a browser script using Amazon S3 buckets for photo albums.\]](http://docs.aws.amazon.com/sdk-for-javascript/v3/developer-guide/images/s3-photo-album-example.png)

The browser script uses the SDK for JavaScript to interact with an Amazon S3 bucket\. Use the following methods of the Amazon S3 client class to enable the photo album application: 
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#listObjects-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#listObjects-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#headObject-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#headObject-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putObject-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#putObject-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#upload-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#upload-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#deleteObject-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#deleteObject-property)
+ [https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#deleteObjects-property](https://docs.aws.amazon.com/AWSJavaScriptSDK/latest/AWS/S3.html#deleteObjects-property)

## Prerequisite Tasks<a name="s3-example-photo-album-scenario-prerequisites"></a>

To set up and run this example, you must first complete these tasks:
+ In the [Amazon S3 console](https://console.aws.amazon.com/s3/), create an Amazon S3 bucket that you will use to store the photos in the album\. For more information about creating a bucket in the console, see [Creating a Bucket](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/create-bucket.html) in the *Amazon Simple Storage Service Console User Guide*\. Make sure you have both **Read** and **Write** permissions on **Objects**\.
+ In the [Amazon Cognito console](https://console.aws.amazon.com/cognito/), create an Amazon Cognito identity pool using Federated Identities with access enabled for unauthenticated users in the same Region as the Amazon S3 bucket\. You need to include the identity pool ID in the code to obtain credentials for the browser script\. For more information about Amazon Cognito Federated Identities, see [Amazon Cognito Identity Pools \(Federated Identites\)](https://docs.aws.amazon.com/cognito/latest/developerguide/cognito-identity.html) in the *Amazon Cognito Developer Guide*\.
+ In the [IAM console](https://console.aws.amazon.com/iam/), find the IAM role created by Amazon Cognito for unauthenticated users\. Add the following policy to grant read and write permissions to an Amazon S3 bucket\. For more information about creating an IAM role, see [Creating a Role to Delegate Permissions to an AWS Service](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_create_for-service.html) in the *IAM User Guide*\.

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
              "s3:PutObject"
           ],
           "Resource": [
              "arn:aws:s3:::BUCKET_NAME/*"
           ]
        }
     ]
  }
  ```

## Configuring CORS<a name="s3-example-photo-album-cors-configuration"></a>

Before the browser script can access the Amazon S3 bucket, you must first set up its [CORS configuration](browser-js-considerations.md#configuring-cors-s3-bucket) as follows\.

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
    </CORSRule>
</CORSConfiguration>
```

## The Web Page<a name="s3-example-photo-album-html"></a>

The HTML for the photo upload application consists of a <div> element within which the browser script creates the upload user interface\. The first <script> element adds the SDK to the browser script\. The second <script> element adds the external JavaScript file that holds the browser script code\.

```
<!DOCTYPE html>
<html>
  <head>
    <script src="https://sdk.amazonaws.com/js/aws-sdk-2.283.1.min.js"></script>
    <script src="./app.js"></script>
    <script>
       function getHtml(template) {
          return template.join('\n');
       }
       listAlbums();
    </script>
  </head>
  <body>
    <h1>My Photo Albums App</h1>
    <div id="app"></div>
  </body>
</html>
```

## Configuring the SDK<a name="s3-example-photo-album-configure-sdk"></a>

Obtain the credentials needed to configure the SDK by calling the `CognitoIdentityCredentials` method, providing the Amazon Cognito identity pool ID\. Next, create an `AWS.S3` service object\.

```
var albumBucketName = "BUCKET_NAME";
var bucketRegion = "REGION";
var IdentityPoolId = "IDENTITY_POOL_ID";

AWS.config.update({
  region: bucketRegion,
  credentials: new AWS.CognitoIdentityCredentials({
    IdentityPoolId: IdentityPoolId
  })
});

var s3 = new AWS.S3({
  apiVersion: "2006-03-01",
  params: { Bucket: albumBucketName }
});
```

Nearly all of the rest of the code in this example is organized into a series of functions that gather and present information about the albums in the bucket, upload and display photos uploaded into albums, and delete photos and albums\. Those functions are:
+ `listAlbums`
+ `createAlbum`
+ `viewAlbum`
+ `addPhoto`
+ `deleteAlbum`
+ `deletePhoto`

## Listing Albums in the Bucket<a name="s3-example-photo-album-list-albums"></a>

The application creates albums in the Amazon S3 bucket as objects whose keys begin with a forward slash character, indicating the object functions as a folder\. To list all the existing albums in the bucket, the application's `listAlbums` function calls the `listObjects` method of the `AWS.S3` service object while using `commonPrefix` so the call returns only objects used as albums\.

The rest of the function takes the list of albums from the Amazon S3 bucket and generates the HTML needed to display the album list in the webpage\. It also enables deleting and opening individual albums\.

```
function listAlbums() {
  s3.listObjects({ Delimiter: "/" }, function(err, data) {
    if (err) {
      return alert("There was an error listing your albums: " + err.message);
    } else {
      var albums = data.CommonPrefixes.map(function(commonPrefix) {
        var prefix = commonPrefix.Prefix;
        var albumName = decodeURIComponent(prefix.replace("/", ""));
        return getHtml([
          "<li>",
          "<span onclick=\"deleteAlbum('" + albumName + "')\">X</span>",
          "<span onclick=\"viewAlbum('" + albumName + "')\">",
          albumName,
          "</span>",
          "</li>"
        ]);
      });
      var message = albums.length
        ? getHtml([
            "<p>Click on an album name to view it.</p>",
            "<p>Click on the X to delete the album.</p>"
          ])
        : "<p>You do not have any albums. Please Create album.";
      var htmlTemplate = [
        "<h2>Albums</h2>",
        message,
        "<ul>",
        getHtml(albums),
        "</ul>",
        "<button onclick=\"createAlbum(prompt('Enter Album Name:'))\">",
        "Create New Album",
        "</button>"
      ];
      document.getElementById("app").innerHTML = getHtml(htmlTemplate);
    }
  });
}
```

## Creating an Album in the Bucket<a name="s3-example-photo-album-create-album"></a>

To create an album in the Amazon S3 bucket, the application's `createAlbum` function first validates the name given for the new album to ensure it contains suitable characters\. The function then forms an Amazon S3 object key, passing it to the `headObject` method of the Amazon S3 service object\. This method returns the metadata for the specified key, so if it returns data, then an object with that key already exists\.

If the album doesn't already exist, the function calls the `putObject` method of the `AWS.S3` service object to create the album\. It then calls the `viewAlbum` function to display the new empty album\.

```
function createAlbum(albumName) {
  albumName = albumName.trim();
  if (!albumName) {
    return alert("Album names must contain at least one non-space character.");
  }
  if (albumName.indexOf("/") !== -1) {
    return alert("Album names cannot contain slashes.");
  }
  var albumKey = encodeURIComponent(albumName) + "/";
  s3.headObject({ Key: albumKey }, function(err, data) {
    if (!err) {
      return alert("Album already exists.");
    }
    if (err.code !== "NotFound") {
      return alert("There was an error creating your album: " + err.message);
    }
    s3.putObject({ Key: albumKey }, function(err, data) {
      if (err) {
        return alert("There was an error creating your album: " + err.message);
      }
      alert("Successfully created album.");
      viewAlbum(albumName);
    });
  });
}
```

## Viewing an Album<a name="s3-example-photo-album-viewing-album"></a>

To display the contents of an album in the Amazon S3 bucket, the application's `viewAlbum` function takes an album name and creates the Amazon S3 key for that album\. The function then calls the `listObjects` method of the `AWS.S3` service object to obtain a list of all the objects \(photos\) in the album\.

The rest of the function takes the list of objects \(photos\) from the album and generates the HTML needed to display the photos in the webpage\. It also enables deleting individual photos and navigating back to the album list\.

```
function viewAlbum(albumName) {
  var albumPhotosKey = encodeURIComponent(albumName) + "//";
  s3.listObjects({ Prefix: albumPhotosKey }, function(err, data) {
    if (err) {
      return alert("There was an error viewing your album: " + err.message);
    }
    // 'this' references the AWS.Response instance that represents the response
    var href = this.request.httpRequest.endpoint.href;
    var bucketUrl = href + albumBucketName + "/";

    var photos = data.Contents.map(function(photo) {
      var photoKey = photo.Key;
      var photoUrl = bucketUrl + encodeURIComponent(photoKey);
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
        "</span>"
      ]);
    });
    var message = photos.length
      ? "<p>Click on the X to delete the photo</p>"
      : "<p>You do not have any photos in this album. Please add photos.</p>";
    var htmlTemplate = [
      "<h2>",
      "Album: " + albumName,
      "</h2>",
      message,
      "<div>",
      getHtml(photos),
      "</div>",
      '<input id="photoupload" type="file" accept="image/*">',
      '<button id="addphoto" onclick="addPhoto(\'' + albumName + "')\">",
      "Add Photo",
      "</button>",
      '<button onclick="listAlbums()">',
      "Back To Albums",
      "</button>"
    ];
    document.getElementById("app").innerHTML = getHtml(htmlTemplate);
  });
}
```

## Adding Photos to an Album<a name="s3-example-photo-album-adding-photos"></a>

To upload a photo to an album in the Amazon S3 bucket, the application's `addPhoto` function uses a file picker element in the webpage to identify a file to upload\. It then forms a key for the photo to upload from the current album name and the file name\.

The function calls the `upload` method of the Amazon S3 service object to upload the photo\. The `ACL` parameter is set to `public-read` so the application can fetch the photos in an album for display by their URL in the bucket\. After uploading the photo, the function redisplays the album so the uploaded photo appears\.

```
function addPhoto(albumName) {
  var files = document.getElementById("photoupload").files;
  if (!files.length) {
    return alert("Please choose a file to upload first.");
  }
  var file = files[0];
  var fileName = file.name;
  var albumPhotosKey = encodeURIComponent(albumName) + "//";

  var photoKey = albumPhotosKey + fileName;

  // Use S3 ManagedUpload class as it supports multipart uploads
  var upload = new AWS.S3.ManagedUpload({
    params: {
      Bucket: albumBucketName,
      Key: photoKey,
      Body: file,
      ACL: "public-read"
    }
  });

  var promise = upload.promise();

  promise.then(
    function(data) {
      alert("Successfully uploaded photo.");
      viewAlbum(albumName);
    },
    function(err) {
      return alert("There was an error uploading your photo: ", err.message);
    }
  );
}
```

## Deleting a Photo<a name="s3-example-photo-album-delete-photo"></a>

To delete a photo from an album in the Amazon S3 bucket, the application's `deletePhoto` function calls the `deleteObject` method of the Amazon S3 service object\. This deletes the photo specified by the `photoKey` value passed to the function\.

```
function deletePhoto(albumName, photoKey) {
  s3.deleteObject({ Key: photoKey }, function(err, data) {
    if (err) {
      return alert("There was an error deleting your photo: ", err.message);
    }
    alert("Successfully deleted photo.");
    viewAlbum(albumName);
  });
}
```

## Deleting an Album<a name="s3-example-photo-album-delete-album"></a>

To delete an album in the Amazon S3 bucket, the application's `deleteAlbum` function calls the `deleteObjects` method of the Amazon S3 service object\.

```
function deleteAlbum(albumName) {
  var albumKey = encodeURIComponent(albumName) + "/";
  s3.listObjects({ Prefix: albumKey }, function(err, data) {
    if (err) {
      return alert("There was an error deleting your album: ", err.message);
    }
    var objects = data.Contents.map(function(object) {
      return { Key: object.Key };
    });
    s3.deleteObjects(
      {
        Delete: { Objects: objects, Quiet: true }
      },
      function(err, data) {
        if (err) {
          return alert("There was an error deleting your album: ", err.message);
        }
        alert("Successfully deleted album.");
        listAlbums();
      }
    );
  });
}
```